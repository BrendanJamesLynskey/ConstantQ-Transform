# The Constant-Q Transform — A Visual Guide

An interactive, animated guide to the Constant-Q Transform (CQT), the frequency-domain representation that mirrors how we perceive musical pitch. Built entirely with vanilla HTML Canvas and JavaScript — zero dependencies.

**[→ View the Live Guide](https://brendanjameslynskey.github.io/ConstantQ-Transform/)**

---

## What's Inside

Seven chapters take you from first principles to real-world applications, each with real-time animated Canvas visualisations.

### 1 · The Core Intuition
Interactive controls let you vary the number of bins per octave and the frequency range. Watch how CQT bin bandwidths grow proportionally with frequency (constant Q), compared to the FFT's fixed bandwidth. An animated overlay pulses through the bin layout.

### 2 · FFT vs CQT
A three-note chord (C3 + E4 + G5) is analysed with both transforms side by side. The FFT crams the low notes into a tiny sliver of its linear axis; the CQT gives each note equal visual weight on its logarithmic axis. Note labels mark the peaks in both views.

### 3 · Log-Frequency Geometry
An animated piano-roll spectrogram plays an arpeggio while plotting its CQT in real time. Notes appear as horizontal bands on a log-frequency axis where each octave spans the same vertical distance — matching the piano keyboard alongside it.

### 4 · The CQT Kernel
Drag a slider across all 48 bins to see how the analysis kernel changes. Low bins have long, slowly-oscillating kernels (precise pitch, poor timing). High bins have short, fast kernels (precise timing, coarser pitch). Both the real part and the Hann window envelope are drawn, with an animated phase cursor.

### 5 · Computing the CQT
Watch a CQT spectrogram build column by column as the analysis window hops through a signal containing a note sequence. A frame indicator on the waveform tracks the current position, and frequency labels map bins to Hz. Adjustable animation speed.

### 6 · From CQT to Chromagram
Switch between C major, A minor, G7, and D minor chords. The full CQT spectrum is shown on top; below it, the bins are folded across octaves into a 12-bar chromagram. Active pitch classes pulse with animated intensity, showing how octave-invariant harmonic profiles emerge.

### 7 · Real-World Applications
A continuously-running CQT spectrogram displays three selectable signals: a scalar melody, a chord progression, and a pitched + percussion mix. Pitched content appears as clean horizontal bands; percussion shows as broadband vertical splashes — demonstrating why the CQT is the standard front-end for music analysis.



---

## Technical Details

- **Zero dependencies.** Pure HTML + Canvas + vanilla JavaScript. No npm, no bundler, no framework.
- **All maths computed in real-time.** CQT coefficients, FFT spectra, chromagrams, and kernel shapes are all calculated on the fly — no pre-baked data.
- **Responsive.** Canvases use `devicePixelRatio` scaling for crisp rendering on HiDPI displays.
- **Distinct design language.** Warm amber/coral palette with Playfair Display + IBM Plex Mono typography, deliberately differentiated from the companion Wavelet Transform guide.

### What's computed where

| Visualisation | Algorithm | Notes |
|---|---|---|
| CQT spectra | Direct CQT with Hann window per bin | O(KN) per frame, K up to 96 bins |
| FFT spectrum | Naive DFT | Sufficient for N ≤ 2048 demo signals |
| Chromagram | CQT energy folded into 12 pitch classes | Sum of squared magnitudes across octaves |
| Piano-roll spectrogram | Frame-by-frame CQT with scrolling history | Synthetic note sequences with harmonics |
| Kernel visualisation | Windowed complex exponential at f_k | Variable length N_k = Q·f_s/f_k |

### Key constants

| Parameter | Default | Purpose |
|---|---|---|
| Bins per octave (B) | 24 | Quarter-tone resolution |
| Q factor | 1/(2^(1/B)−1) ≈ 33.5 at B=24 | Frequency/bandwidth ratio |
| f_min | 65.41 Hz (C2) | Lowest analysis frequency |
| Sample rate (f_s) | 8000 Hz | Sufficient for demo range |

---

## Customisation

**Change the colour scheme** — edit the CSS custom properties:

```css
:root {
  --accent:  #e8875b;   /* warm coral  */
  --accent2: #d4a843;   /* gold        */
  --accent3: #5bbce8;   /* cool blue   */
  --accent4: #c75b8e;   /* rose pink   */
}
```

**Change musical tuning** — modify `midiToFreq()` and `fMin` in the script. The default is A440 equal temperament.

**Add chords to the chromagram** — extend the `chords` object with MIDI note arrays:

```javascript
const chords = {
  C:   [60, 64, 67],
  Am:  [57, 60, 64],
  G7:  [55, 59, 62, 65],
  Dm:  [50, 53, 57],
  // Add your own:
  Fmaj7: [53, 57, 60, 64],
};
```

**Increase frequency range** — adjust `numOct` and `fMin` in each section's IIFE. More octaves means more bins and heavier computation, but it's fine up to about 6–7 octaves on modern hardware.

---


---

## License

MIT — use it however you like. Attribution appreciated but not required.
