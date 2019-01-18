# render-breaker (needs testing in other browsers)
Breaks a tab's rendering in Chrome by setting an element's skew to 90deg in a weird way.

My attempt at an explanation:
1. Chrome knows rendering an element with `transform: skew(90deg)` messes things up, so it catches when a website tries to do so and just doesn't display the element.
2. By modifying the style of an element that's already been rendered and "passed" by this theoretical check, maybe Chrome doesn't check again when it should and lets an element with `transform skew(90deg)` be rendered.
3. The 90deg skew goes under the radar, Chrome tries to render it, and madness ensues.

## Found to cause problems with:
- Chrome (Chrome OS, Windows 7, Windows 10)
- Chrome for Android

## Found to _not_ cause problems with:
- Firefox Quantum (on Windows 10) - faint lines that maybe shouldn't be there, but at least the page doesn't get demolished
- Edge

---

## Demo here (activates immediately)
[https://grosserly.github.io/render-breaker/break.html](https://grosserly.github.io/render-breaker/break.html)
