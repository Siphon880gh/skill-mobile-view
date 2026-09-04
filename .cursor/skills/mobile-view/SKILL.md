---
name: mobile-view
description: >-
  Emulate a real mobile browser in the Cursor IDE browser: user agent, viewport,
  device pixel ratio, and touch — not CSS width alone. Use when the user asks to
  view a site as mobile, iPhone, Android, Pixel, or to check user-agent-specific
  markup (WordPress wp_is_mobile, theme mobile templates, different carousels).
---

# Mobile view (UA + device, not just width)

Resizing the viewport is not enough. WordPress `wp_is_mobile()`, many themes, and some CDNs branch on the **request User-Agent**. Set UA **before** navigation so the HTTP request is mobile.

## Do this (Cursor IDE browser CDP)

1. Lock the tab.
2. Apply a preset with `Emulation.setUserAgentOverride`, `Emulation.setDeviceMetricsOverride`, and `Emulation.setTouchEmulationEnabled`.
3. **Then** `browser_navigate` (use `newTab: true` if an admin session is already open).
4. Confirm with `Runtime.evaluate`: `navigator.userAgent`, `innerWidth`, `ontouchstart`.
5. Screenshot / snapshot the flow. Reset when done.

Do not use CSS-only tricks (`iframe` width, `window.resizeTo`) as a substitute.

## Presets

### iPhone 14 (default)

```
Emulation.setUserAgentOverride
  userAgent: Mozilla/5.0 (iPhone; CPU iPhone OS 16_6 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/16.6 Mobile/15E148 Safari/604.1
  platform: iPhone

Emulation.setDeviceMetricsOverride
  width: 390, height: 844, deviceScaleFactor: 3, mobile: true

Emulation.setTouchEmulationEnabled
  enabled: true
```

### Pixel 7

```
userAgent: Mozilla/5.0 (Linux; Android 13; Pixel 7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Mobile Safari/537.36
platform: Linux armv8l
width: 412, height: 915, deviceScaleFactor: 2.625, mobile: true
```

### Reset to desktop

```
Emulation.clearDeviceMetricsOverride
Emulation.setTouchEmulationEnabled { enabled: false }
Emulation.setUserAgentOverride { userAgent: "" }
```

Then reload or navigate again so the server sees a desktop UA.

## Verify

```js
({
  ua: navigator.userAgent,
  mobileUA: /Mobile|Android|iPhone/.test(navigator.userAgent),
  width: innerWidth,
  height: innerHeight,
  dpr: devicePixelRatio,
  touch: 'ontouchstart' in window,
  platform: navigator.platform
})
```

If `mobileUA` is false, the UA override did not stick — do not treat the page as a mobile render.

## Notes

- `wp_is_mobile()` is server-side. Changing UA after load without a reload still shows desktop PHP.
- Media queries still need the metrics override (`mobile: true` + width).
- If a CDP Emulation method is denied, say so and fall back to Playwright `devices['iPhone 14']` (viewport + UA + `isMobile` + `hasTouch`).
