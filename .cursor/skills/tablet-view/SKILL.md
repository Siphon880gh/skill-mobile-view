---
name: tablet-view
description: >-
  Emulate a real tablet browser in the Cursor IDE browser: user agent, viewport,
  device pixel ratio, and touch — not CSS width alone. Use when the user asks to
  view a site as tablet, iPad, Pixel Tablet, or to check tablet-specific markup
  (iPad themes, Android tablet UA without "Mobile", split layouts).
---

# Tablet view (UA + device, not just width)

Resizing the viewport is not enough. Many themes, CDNs, and WordPress `wp_is_mobile()` branch on the **request User-Agent**. Tablets are not large phones: iPad UAs include `iPad` (and usually `Mobile`), while Android tablets include `Android` but typically **omit** `Mobile`. Set UA **before** navigation so the HTTP request is a tablet.

## Do this (Cursor IDE browser CDP)

1. Lock the tab.
2. Apply a preset with `Emulation.setUserAgentOverride`, `Emulation.setDeviceMetricsOverride`, and `Emulation.setTouchEmulationEnabled`.
3. **Then** `browser_navigate` (use `newTab: true` if an admin session is already open).
4. Confirm with `Runtime.evaluate`: `navigator.userAgent`, `innerWidth`, `ontouchstart`, `navigator.maxTouchPoints`.
5. Screenshot / snapshot the flow. Reset when done.

Do not use CSS-only tricks (`iframe` width, `window.resizeTo`) as a substitute.

## Presets

### iPad Pro 11 (default)

```
Emulation.setUserAgentOverride
  userAgent: Mozilla/5.0 (iPad; CPU OS 17_5 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/17.5 Mobile/15E148 Safari/604.1
  platform: iPad

Emulation.setDeviceMetricsOverride
  width: 834, height: 1194, deviceScaleFactor: 2, mobile: true

Emulation.setTouchEmulationEnabled
  enabled: true
```

Landscape: swap to `width: 1194, height: 834`.

### Pixel Tablet

```
userAgent: Mozilla/5.0 (Linux; Android 14; Pixel Tablet) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36
platform: Linux armv8l
width: 800, height: 1280, deviceScaleFactor: 2, mobile: true
```

This UA has `Android` and **no** `Mobile` token — that is how Chrome identifies an Android tablet vs a phone.

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
  iPad: /iPad/.test(navigator.userAgent),
  androidTablet: /Android/.test(navigator.userAgent) && !/Mobile/.test(navigator.userAgent),
  width: innerWidth,
  height: innerHeight,
  dpr: devicePixelRatio,
  touch: 'ontouchstart' in window,
  maxTouchPoints: navigator.maxTouchPoints,
  platform: navigator.platform
})
```

If neither `iPad` nor `androidTablet` is true, the UA override did not stick — do not treat the page as a tablet render.

## Notes

- `wp_is_mobile()` matches this iPad UA (`Mobile`) and this Pixel Tablet UA (`Android`). It does **not** match iPadOS desktop-class Safari, which reports as Macintosh.
- Do not use the iPhone preset from `mobile-view` as a stand-in. Phone UAs and ~390px widths trigger phone layouts, not tablet split views.
- Media queries still need the metrics override (`mobile: true` + tablet width).
- If a CDP Emulation method is denied, say so and fall back to Playwright `devices['iPad Pro 11']` (viewport + UA + `isMobile` + `hasTouch`).
