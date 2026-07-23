# Machine 3 — Test Cases and Expected Outputs

All expected values below were verified bit-for-bit against native IEEE 754 hardware arithmetic. Take one screenshot per row (store them in `screenshots/`) and demonstrate them in the video in this order.

## Function 1 · Decimal → double-precision conversion

| # | Case type | Input | Expected hex | Notes |
|---|-----------|-------|--------------|-------|
| 1 | Normal, negative | `-12.375` | `C028 C000 0000 0000` | 1.100011₂ × 2³, E = 1026 |
| 2 | Normal, inexact | `0.1` | `3FB9 9999 9999 999A` | repeating binary fraction, rounded (…999A) |
| 3 | Normal | `85.125` | `4055 4800 0000 0000` | 1.0101010010₂ × 2⁶ |
| 4 | Scientific notation | `6.02e23` | `44DF DE9F 10A8 D361` | large magnitude |
| 5 | Special: NaN | `NaN` | `7FF8 0000 0000 0000` | exponent all 1s, mantissa ≠ 0 |
| 6 | Special: −Infinity | `-Infinity` | `FFF0 0000 0000 0000` | exponent all 1s, mantissa = 0 |
| 7 | Special: signed zero | `-0` | `8000 0000 0000 0000` | sign bit preserved |
| 8 | Edge: denormal | `1e-310` | `0000 1268 8B70 E62B` | below 2⁻¹⁰²², exponent field 0 |
| 9 | Edge: largest double | `1.7976931348623157e308` | `7FEF FFFF FFFF FFFF` | MAX_DOUBLE |
| 10 | Edge: overflow | `1e309` | `7FF0 0000 0000 0000` | rounds to +Infinity |

## Function 2 · Rounding methods

| # | Case type | Input | Base | Keep | Mode | Chop | Up (+∞) | Down (−∞) | Ties-to-even |
|---|-----------|-------|------|------|------|------|---------|-----------|--------------|
| 1 | Binary, below half | `0.110101` | 2 | 4 | fractional | 0.1101 | 0.1110 | 0.1101 | 0.1101 |
| 2 | Binary, tie + odd LSB | `0.11011` | 2 | 4 | fractional | 0.1101 | 0.1110 | 0.1101 | **0.1110** |
| 3 | Binary, tie + even LSB | `0.11001` | 2 | 4 | fractional | 0.1100 | 0.1101 | 0.1100 | **0.1100** |
| 4 | Decimal, significant | `2.7182818` | 10 | 4 | significant | 2.718 | 2.719 | 2.718 | 2.718 |
| 5 | Decimal, tie (2.5) | `2.5` | 10 | 1 | significant | 2 | 3 | 2 | **2** (even) |
| 6 | Decimal, tie (3.5) | `3.5` | 10 | 1 | significant | 3 | 4 | 3 | **4** (even) |
| 7 | Negative direction test | `-1.2345` | 10 | 3 | fractional | -1.234 | **-1.234** | **-1.235** | -1.234 |
| 8 | Carry chain | `0.9999` | 10 | 2 | fractional | 0.99 | 1.00 | 0.99 | 1.00 |
| 9 | Cut inside integer part | `1999.9` | 10 | 2 | significant | 1900 | 2000 | 1900 | 2000 |

Row 7 demonstrates that round-up/round-down are directed toward +∞/−∞ (not "away from zero"), so they swap roles for negative numbers.

## Function 3 · Arithmetic with GRS

| # | Case type | A | B | Op | Expected decimal | Expected hex |
|---|-----------|---|---|----|------------------|--------------|
| 1 | Normal add (rounding up) | `0.1` | `0.2` | + | 0.30000000000000004 | `3FD3 3333 3333 3334` |
| 2 | Exact add | `2.5` | `2.5` | + | 5 | `4014 0000 0000 0000` |
| 3 | Sticky-bit tie | `1e16` | `1` | + | 10000000000000000 | `4341 C379 37E0 8000` (tie → even, addend absorbed) |
| 4 | Massive cancellation | `1` | `-0.9999999999999999` | + | 1.1102230246251565e-16 | `3CA0 0000 0000 0000` |
| 5 | Hex input | `3FF0000000000000` | `4000000000000000` | + | 3 | `4008 0000 0000 0000` |
| 6 | ∞ + (−∞) | `Infinity` | `-Infinity` | + | NaN | `7FF8 0000 0000 0000` |
| 7 | Normal multiply | `0.1` | `0.1` | × | 0.010000000000000002 | `3F84 7AE1 47AE 147C` |
| 8 | Overflow | `1e308` | `10` | × | +Infinity | `7FF0 0000 0000 0000` |
| 9 | Gradual underflow | `1e-308` | `1e-10` | × | 1e-318 (denormal) | `0000 0000 0003 16A2` |
| 10 | Sign of zero | `-0` | `5` | × | -0 | `8000 0000 0000 0000` |

Every arithmetic screenshot should also show the green "verified bit-exact against host CPU IEEE 754 hardware" badge and the GRS step trace.
