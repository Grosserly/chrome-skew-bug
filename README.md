# Chrome Skew Bug
Breaks a tab's rendering in Chrome by setting an element's skew to 90deg in a weird way.

[A look at the Chromium source](https://cs.chromium.org/chromium/src/ui/gfx/transform.cc?q=skew&sq=package:chromium&dr=CSs&l=203) suggests:
- Chrome calculates the tangent of the parameters passed into `skew()`
- The tangent of 90 degrees is `undefined`
- Chrome seems to catch this under most circumstances, but for some reason it doesn't properly do so here
- (Speculation) the renderer breaks down as things go looking for a number and get `undefined` instead

It's possible that this isn't the only place in the code that this can happen. I suspect other places where trigonometric functions are used might also be vulnerable.

## Found to cause problems with:
- Chrome (Chrome OS, Windows 7, Windows 10)
- Chrome for Android

## Found to _not_ cause problems with:
- Firefox Quantum (on Windows 10) - faint lines that maybe shouldn't be there, but at least the page doesn't get demolished
- Edge

---

## Demo here (activates after five seconds)
[https://grosserly.github.io/chrome-skew-bug/break.html](https://grosserly.github.io/chrome-skew-bug/break.html)
