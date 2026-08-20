---
name: mobile-view
description: >-
  Emulate a real mobile browser: user agent and viewport, not just CSS width.
  Use when checking a site as iPhone or mobile.
---

# Mobile view

Resizing the viewport is not enough. WordPress `wp_is_mobile()` and many themes
branch on the request User-Agent. Set UA before navigation.

## iPhone 14

```
Emulation.setUserAgentOverride
  userAgent: Mozilla/5.0 (iPhone; CPU iPhone OS 16_6 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/16.6 Mobile/15E148 Safari/604.1

Emulation.setDeviceMetricsOverride
  width: 390, height: 844, deviceScaleFactor: 3, mobile: true
```

## Verify

```js
({
  ua: navigator.userAgent,
  mobileUA: /Mobile|Android|iPhone/.test(navigator.userAgent),
  width: innerWidth,
  height: innerHeight
})
```

If `mobileUA` is false, the UA override did not stick — do not treat the page as a mobile render.

`wp_is_mobile()` is server-side. Changing UA after load without a reload still shows desktop PHP.
