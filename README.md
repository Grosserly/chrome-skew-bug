# Chrome Skew Bug
Breaks a tab's rendering in Chrome by setting an element's skew to 90deg in a weird way.

[A look at the Chromium source](https://cs.chromium.org/chromium/src/ui/gfx/transform.cc?q=skew&sq=package:chromium&dr=CSs&l=203) suggests:
- Chrome calculates the tangent of the parameters passed into `skew()`
- The Tangent of 90 degrees is `undefined`
- Chrome seems to catch this under most circumstances, but here it doesn't properly do so
- The renderer breaks down as things go looking for a number and get `undefined` instead

It's possible that this isn't the only place in the code that this can happen. I suspect other places where trigonometric functions are used might also be vulnerable.

~~My attempt at an explanation:~~
1. ~~Chrome knows rendering an element with `transform: skew(90deg)` messes things up, so it catches when a website tries to do so and just doesn't display the element.~~
2. ~~By modifying the style of an element that's already been rendered and "passed" by this theoretical check, maybe Chrome doesn't check again when it should and lets an element with `transform skew(90deg)` be rendered.~~
3. ~~The 90deg skew goes under the radar, Chrome tries to render it, and madness ensues.~~

## Found to cause problems with:
- Chrome (Chrome OS, Windows 7, Windows 10)
- Chrome for Android

## Found to _not_ cause problems with:
- Firefox Quantum (on Windows 10) - faint lines that maybe shouldn't be there, but at least the page doesn't get demolished
- Edge

---

## Demo here (activates immediately)
[https://grosserly.github.io/chrome-skew-bug/break.html](https://grosserly.github.io/chrome-skew-bug/break.html)
