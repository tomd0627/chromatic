# Chromatic — Visual Impairment Accessibility Simulator

An interactive tool that lets designers and developers experience interfaces through the eyes of users with visual impairments. Select a condition, drag the comparison handle, and read exactly what breaks and how to fix it.

---

## What it does

Chromatic renders a deliberately flawed e-commerce UI — one packed with real accessibility failures — and applies physiologically accurate simulations of ten visual conditions. A split-screen slider lets you compare normal vision against the filtered view in real time. Each condition comes with a breakdown of the specific WCAG violations it exposes and copyable CSS fixes.

Upload your own screenshot to run it through any filter — drag a file onto the slider or use the toolbar button. A download button captures the current comparison view as a 2× PNG.

A screenshot gallery at the bottom shows the same failure patterns appearing across four different UI types: form validation, an analytics chart, a navigation bar, and an editorial article — making the point that these aren't edge cases confined to one component.

## Conditions simulated

| Condition            | Mechanism                     | Prevalence                     |
| -------------------- | ----------------------------- | ------------------------------ |
| Normal Vision        | Baseline reference            | —                              |
| Refractive Error     | CSS blur + contrast           | ~2.2 billion globally          |
| Severe Low Vision    | Heavy CSS blur                | 295 million moderate–severe    |
| Protanopia           | Brettel/Viénot SVG matrix     | ~1% of males                   |
| Deuteranopia         | Brettel/Viénot SVG matrix     | ~1% of males (most common CVD) |
| Tritanopia           | Brettel/Viénot SVG matrix     | ~0.01%                         |
| Achromatopsia        | CSS grayscale                 | ~0.003%                        |
| Cataracts            | CSS blur + brightness + sepia | ~100 million globally          |
| Macular Degeneration | Canvas radial overlay         | ~200 million globally          |
| Glaucoma             | Canvas tunnel overlay         | ~80 million globally           |

## Technical approach

**Colour vision deficiency filters** use the Brettel/Viénot dichromacy model encoded as `<feColorMatrix>` elements in an inline SVG. `color-interpolation-filters="linearRGB"` is required so the matrix is applied in linear light rather than gamma-encoded sRGB — without it the simulation is physiologically wrong.

**Blur and optical conditions** (refractive error, cataracts, severe low vision) use CSS `filter` chains.

**Macular degeneration and glaucoma** can't be represented with CSS alone. They use Canvas elements positioned as overlays, drawing radial gradients that update when the slider container is resized.

**No build step.** The entire tool is a single `index.html`. Open it in a browser and it works.

## Accessibility of the tool itself

It would be embarrassing if a tool about accessibility wasn't accessible.

- **Keyboard navigation** — filter pills use a radiogroup with roving tabindex and arrow-key navigation; the comparison slider is keyboard-operable with arrow keys, Home, and End
- **Screen reader support** — filter changes are announced via an `aria-live` region; the filtered pane is `aria-hidden` to avoid reading duplicate content
- **Font choice** — body text uses [Atkinson Hyperlegible](https://brailleinstitute.org/freefont), designed by the Braille Institute specifically for low-vision readability
- **Motion** — all transitions are gated behind `@media (prefers-reduced-motion: no-preference)`
- **Theme** — full light/dark mode support, with theme persisted to `localStorage` and initialised synchronously to prevent flash

## Design notes

Palette: warm linen (`#f5f1eb`) + deep teal (`#1a6b7a`). This deliberately inverts the dark-navy / bright-mint palette on [tomdeluca.dev](https://tomdeluca.dev/) — same visual family, opposite luminance register.

Display font: [Fraunces](https://github.com/undercasetype/Fraunces) — an optical-variable serif chosen for warmth and editorial precision.

## Running locally

No install required. Clone the repo and open `index.html` in a browser:

```sh
git clone https://github.com/tomdeluca/chromatic.git
cd chromatic
open index.html   # macOS
start index.html  # Windows
```

Or serve it with any static file server if you prefer:

```sh
npx serve .
```

## License

MIT — see [LICENSE](LICENSE).

---
