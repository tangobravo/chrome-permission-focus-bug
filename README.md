# Chrome permission prompt focus bug

A static test page to reproduce a Chrome on Android bug: after a permission prompt is dismissed, `document.hasFocus()` doesn't return to `true` until the page is tapped again.

Because Chrome only delivers `devicemotion` events to a focused document, motion data also stops after a permission prompt. For example, a gyro-driven AR page on a camera background loses its motion data when the camera permission prompt is shown.

## What the page shows

- **Visibility**: `document.visibilityState`, green when `visible`
- **Focus**: `document.hasFocus()`, green when `true`
- **Motion**: `devicemotion` events per second; grey when the listener is off, red when it's on but no events are arriving, green when events are arriving

State updates on `focus`, `blur` and `visibilitychange` events, and on a 1s poll of the getters, which you can switch off. Every event and poll is written to the log with a timestamp.

## Reproducing

1. Open the page in Chrome on Android.
2. Tap **Enable motion** and wait for the Motion box to turn green.
3. Optionally, turn **Polling** off so the log only shows events.
4. Tap **Request geolocation**, then allow or block the prompt.
5. Without touching the page, watch the Focus and Motion boxes and the log.

**Expected:** focus returns and motion events resume once the prompt closes.

**Actual:** `hasFocus()` stays `false` and motion events stay stopped until the page is tapped again.

Once the location permission has been granted or blocked, Chrome won't prompt again. Reset it in the site settings (Permissions → Location) before running the test again.
