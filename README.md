# Machine 3 — IEEE 754 Binary64 (Double-Precision) Floating-Point Machine

**CSARCH2 Simulation Project · 3rd Term, AY 2025-2026 · Group G3_S40**

A web-based simulator of a 64-bit IEEE 754 binary floating-point computing machine. It converts decimal numbers to double-precision format, demonstrates the four classical rounding methods, and performs addition and multiplication using the **Guard–Round–Sticky (GRS)** method with a complete step-by-step trace of the computation.

🔗 **Live deployment:** *ADD LINK HERE (also place it in the repository's About → Website field)*
🎬 **Video walkthrough (5–8 min):** *ADD YOUTUBE LINK HERE*

## Features

**1 · Decimal → IEEE 754 double-precision converter.** Accepts any decimal number (plain or scientific notation) as well as the special values NaN, ±Infinity and ±0, and displays the stored 64-bit word as a color-coded register, as binary with proper field spacing (1 sign · 11 exponent · 52 mantissa bits, mantissa grouped in nibbles), and as 16 hexadecimal digits. Every conversion includes the full derivation: sign determination, normalization to 1.f × 2^e (or the denormalized 0.f × 2^−1022 form), exponent biasing with bias 1023, mantissa extraction, and final assembly. Out-of-range inputs are handled correctly: values above ~1.798 × 10^308 overflow to ±Infinity and values below 2^−1022 are stored as denormalized numbers, down to 2^−1074.

**2 · Rounding method demonstrator.** Rounds a number given in decimal or binary to a target number of digits or bits — counted either as fractional digits or as significant digits — using all four methods: chopping (truncation toward zero), round-up (toward +∞), round-down (toward −∞), and round-to-nearest ties-to-even. The kept and discarded digits are shown explicitly, and the ties-to-even decision is explained (below ½ ULP, above ½ ULP, or an exact tie resolved by the parity of the last kept digit). Negative numbers, carry propagation (e.g. 0.9999 → 1.00), and cuts that fall inside the integer part (e.g. 1999.9 at 2 significant digits → 2000) are all supported.

**3 · Addition and multiplication with the GRS method.** Operands may be entered in decimal or as 16-digit IEEE 754 hexadecimal. The machine decodes both operands, aligns exponents (addition) or adds exponents and multiplies significands exactly (multiplication), normalizes, extracts the Guard, Round and Sticky bits, and applies round-to-nearest ties-to-even, showing every step. Results are presented in binary with proper spacing, in hexadecimal, and in decimal. All IEEE special cases are implemented: NaN propagation, ∞ + (−∞) → NaN, ∞ × 0 → NaN, signed-zero rules, exact cancellation → +0, overflow → ±Infinity, and gradual underflow into denormalized results.

## Correctness

The arithmetic engine represents finite doubles exactly as integers scaled by a power of two, performs the operation exactly, and rounds once using the GRS bits — the same result a hardware FPU produces. It was fuzz-tested against the host CPU's native IEEE 754 arithmetic on **more than 500,000 cases** (random bit patterns across the full range, denormals, signed zeros, infinities, NaN, exact ties, massive cancellation, overflow and underflow) with **zero mismatches**. In addition, every computation performed in the app displays a live verification badge comparing the simulated result bit-for-bit against the browser's native floating-point hardware.

## Extra features (beyond the specification)

Interactive color-coded 64-bit register with per-bit tooltips (bit index and field); live bit-exact hardware verification badge on every arithmetic result; one-click quick-test chips for all normal, special and edge cases; copy-to-clipboard for every output format; support for both fractional-digit and significant-digit rounding modes; scientific-notation input; and a fully responsive layout that works on mobile.

## Running and deploying

The entire application is a single dependency-free `index.html` (vanilla HTML/CSS/JS). To run locally, simply open `index.html` in any modern browser. To deploy on GitHub Pages: repository **Settings → Pages → Deploy from a branch → main / (root)**, then wait a minute and the site is served at `https://<username>.github.io/<repo>/`. Place that URL in the About → Website section of the repository as required by the specification.

## Test cases

See [`TEST_CASES.md`](TEST_CASES.md) for the full list of normal, special and edge cases with expected outputs, used for the screenshots in [`screenshots/`](screenshots/) and demonstrated in the video walkthrough.

## Group G3_S40

Alvarez, James Edsel Crestoria · Obcena, Hans Gabriel David · Suerte, Lorenzo Enrique Evangelista · Tayzon, Hannah Moriah Luna · Viray, Jarick Klein Lacsamana
