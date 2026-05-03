# Routh–Hurwitz Stability Analyzer

A browser-based tool for analyzing the stability of linear control systems using the Routh–Hurwitz criterion. No install, no dependencies, no build step — just a single HTML file.

**[Live Demo →](https://yourusername.github.io/routh-hurwitz)**

---

## What it does

Given the characteristic equation of a closed-loop system:

```
a₀sⁿ + a₁sⁿ⁻¹ + … + aₙ = 0
```

The tool builds the Routh array and determines how many roots lie in the right-half plane (RHP). A system is stable if and only if all roots are in the left-half plane — i.e. zero sign changes in the first column of the Routh array.

---

## Special cases handled

### Case 1 — Zero in the first column only
A zero in the first column means the standard pivot is undefined and the table cannot continue. The tool handles this by multiplying the original characteristic polynomial by `(s + 1)` and rebuilding the entire Routh array from scratch. Since `(s + 1)` contributes one known stable pole at `s = −1`, the sign change count still correctly reflects the original system's RHP poles.

### Case 2 — Entire row of zeros
An all-zero row indicates the presence of symmetric root pairs (e.g. purely imaginary or real roots mirrored about the origin). The tool handles this by:
1. Forming the **auxiliary polynomial** `A(s)` from the row directly above the zero row, using the correct powers of s for that row
2. Differentiating `A(s)` with respect to s
3. Substituting the derivative coefficients as the replacement row, then continuing the table

Both A(s) and dA/ds are shown explicitly in the output so you can verify the working.

---

## Arithmetic

All table values are computed using **exact integer fraction arithmetic** (numerator/denominator pairs with GCD reduction). This avoids floating-point drift that would cause false zero detections in deeper rows — a common failure mode when using decimals for this algorithm.

---

## Usage

1. Enter the polynomial degree `n`
2. Click **Set coefficients** and fill in `a₀` through `aₙ`
3. Click **Build Routh table**

The output shows:
- The completed Routh array with highlighted cells for sign changes, Case 1 pivots, and Case 2 replacement rows
- The auxiliary polynomial and its derivative (when Case 2 applies)
- A clear stable/unstable verdict with the RHP pole count

---

## Example

For the characteristic polynomial `s⁵ + 2s⁴ + 24s³ + 48s² − 24s − 50`:

| Coefficients | a₀=1, a₁=2, a₂=24, a₃=48, a₄=−24, a₅=−50 |
|---|---|
| Degree | 5 |
| Case 2 at | s³ row (all-zero) |
| Aux poly | A(s) = 2s⁴ + 48s² − 50 |
| dA/ds | 8s³ + 96s |
| Result | Unstable |

---

## Hosting

This is a single `index.html` file with no external dependencies beyond two Google Fonts. It can be hosted anywhere static files are served.

To host on GitHub Pages: push `index.html` to the root of a public repository, then enable Pages under **Settings → Pages → Deploy from branch → main / root**.

---

## Tech

- Vanilla HTML, CSS, JavaScript — no frameworks
- Exact fraction arithmetic for all Routh array values
- Dark theme with `Space Mono` (numeric content) and `Syne` (headings)
