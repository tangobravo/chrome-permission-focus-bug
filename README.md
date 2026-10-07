# Chrome permission prompt focus bug

A static test page to reproduce a Chrome on Android bug: after a permission prompt is dismissed, `document.hasFocus()` doesn't return to `true` until the page is tapped again.

Since Chrome 153, sensors are suspended while the page is unfocused, so `devicemotion` events also stop after a permission prompt. For example, a gyro-driven AR page on a camera background loses its motion data when the camera permission prompt is shown.

- **Chromium issue:** https://issues.chromium.org/issues/568009418
- **Demo:** https://tangobravo.github.io/chrome-permission-focus-bug/

## What the page shows

- **Visibility**: `document.visibilityState`, green when `visible`
- **Focus**: `document.hasFocus()`, green when `true`
- **Motion**: `devicemotion` events per second; grey when the listener is off, red when it's on but no events are arriving, green when events are arriving

State updates on `focus`, `blur` and `visibilitychange` events. Every event is written to the log with a timestamp. **Polling** (off by default) also reads the getters every second and logs each reading, so you can check that the event listeners aren't missing a change.

## Reproducing

1. Open the page in Chrome on Android.
2. Tap **Enable motion** and wait for the Motion box to turn green.
3. Tap **Request geolocation**. After a 5 second countdown the prompt appears; allow or block it.
4. Without touching the page, watch the Focus and Motion boxes and the log.

**Expected:** focus returns and motion events resume once the prompt closes.

**Actual:** `hasFocus()` stays `false` and motion events stay stopped until the page is tapped again.

Once the location permission has been granted or blocked, Chrome won't prompt again. Reset it in the site settings (Permissions → Location) before running the test again.
