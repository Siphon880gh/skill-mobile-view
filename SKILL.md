---
name: mobile-view
description: >-
  Emulate a real mobile browser: user agent and viewport, not just CSS width.
  Use when checking a site as iPhone, Android, or Pixel.
---

# Mobile view

Resizing the viewport is not enough. WordPress `wp_is_mobile()` and many themes
branch on the request User-Agent. Set UA before navigation.

## iPhone 14

```
Emulation.setUserAgentOverride
  userAgent: Mozilla/5.0 (iPhone; CPU iPhone OS 16_6 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/16.6 Mobile/15E148 Safari/604.1
  platform: iPhone

Emulation.setDeviceMetricsOverride
  width: 390, height: 844, deviceScaleFactor: 3, mobile: true

Emulation.setTouchEmulationEnabled
  enabled: true
```

## Pixel 7

```
userAgent: Mozilla/5.0 (Linux; Android 13; Pixel 7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Mobile Safari/537.36
platform: Linux armv8l
width: 412, height: 915, deviceScaleFactor: 2.625, mobile: true
```

## Reset to desktop

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

`wp_is_mobile()` is server-side. Changing UA after load without a reload still shows desktop PHP.
Media queries still need the metrics override (`mobile: true` + width).
