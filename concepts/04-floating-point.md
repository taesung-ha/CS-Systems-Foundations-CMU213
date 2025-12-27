# 04 - Floating Point (IEEE 754)

## 1. Fractional Binary Numbers
Binary fractions follow the same principle as decimal, but with powers of 2.
- <strong>Representation:</strong> $d_i d_{i-1} \dots d_0 . d_{-1} d_{-2} \dots d_{-j}$ where $d_{-j}$ is $2^{-j}$.
- <strong>Shifting:</strong> 
  - Left shift: Multiplies by 2.
  - Right shift: Divides by 2.
- <strong>Precision Limits:</strong> Fractions like $1/10$ or $1/3$ cannot be represented exactly in finite binary bits; they become repeating bit patterns ($0.00011[0011]\dots$ for $1/10$).

## 2. IEEE 754 Standard Structure
Standardized in 1985 to provide uniform behavior for hardware. 
<strong>Value Formula:</strong> $V = (-1)^s \times M \times 2^E$

### Bit Layout
- <strong>Sign bit ($s$):</strong> 1 bit (0 for positive, 1 for negative).
- <strong>Exponent field ($exp$):</strong> Encodes $E$ (weighted value).
- <strong>Fraction field ($frac$):</strong> Encodes $M$ (significand/mantissa).

| Precision | Total Bits | $exp$ Bits | $frac$ Bits | Bias |
|---|---|---|---|---|
| <strong>Single</strong> | 32 | 8 | 23 | 127 |
| <strong>Double</strong> | 64 | 11 | 52 | 1023 |

---

## 3. The Three Cases of Representation

### Case 1: Normalized Values ($exp \neq 0$ and $exp \neq 255/2047$)
- <strong>Exponent ($E$):</strong> $E = exp - Bias$ (Allows negative exponents).
- <strong>Significand ($M$):</strong> $M = 1.xxxxx\dots$ (Implicit leading 1 saves a bit).
- <strong>Range:</strong> $M$ is in $[1.0, 2.0)$.

### Case 2: Denormalized Values ($exp = 0$)
Used to represent values very close to zero and to avoid a "sudden jump" at zero.
- <strong>Exponent ($E$):</strong> $E = 1 - Bias$ (Crucial for smooth transition from normalized).
- <strong>Significand ($M$):</strong> $M = 0.xxxxx\dots$ (Implicit leading 0).
- <strong>Special Values:</strong> 
  - $exp=0, frac=0$ represents <strong>0.0</strong> (both +0.0 and -0.0 exist).

### Case 3: Special Values ($exp = 111\dots1$)
- <strong>Infinity ($\infty$):</strong> $frac = 0$. Used for overflow or $1.0/0.0$.
- <strong>NaN (Not a Number):</strong> $frac \neq 0$. Result of invalid operations like $\sqrt{-1}$, $\infty - \infty$.

---

## 4. Rounding Logic (Round-to-Nearest Even)
When a value falls exactly halfway between two representable numbers, the system rounds to the one where the <strong>least significant bit is 0 (even)</strong>.

### Hardware Implementation (GRS Bits)
- <strong>Guard bit (G):</strong> The last bit of the target precision.
- <strong>Round bit (R):</strong> The first bit removed.
- <strong>Sticky bit (S):</strong> Logical OR of all bits beyond the Round bit.
- <strong>Rule:</strong> 
  - If $R=1$ and $S=1$, round up.
  - If $R=1, S=0$, it's a tie—round up only if $G=1$ (to make it even).

---

## 5. Floating Point Operations & Math Properties
### Basic Steps (Add/Mult)
1. Compute the exact result.
2. <strong>Fixing:</strong> Shift $M$ to fit the range $[1.0, 2.0)$ and adjust $E$.
3. <strong>Check:</strong> Handle Overflow/Underflow.
4. <strong>Rounding:</strong> Round $M$ to fit $frac$ bits.

### Mathematical Realities
- <strong>Commutative:</strong> $a + b = b + a$ (YES).
- <strong>Associative:</strong> $(a + b) + c = a + (b + c)$ (<strong>NO</strong>). Rounding error or overflow can occur differently.
- <strong>Distributive:</strong> $a \times (b + c) = a \times b + a \times c$ (<strong>NO</strong>).
- <strong>Monotonicity:</strong> $a \geq b \Rightarrow a + c \geq b + c$ (YES, except for NaNs).

---

## 6. Casting in C Language
Casting between types can lead to subtle bugs:
- <strong>`double/float` to `int`:</strong> Truncates toward zero. Undefined if out of range.
- <strong>`int` to `double`:</strong> Exact conversion (double has 53 bits of precision, more than 32-bit int).
- <strong>`int` to `float`:</strong> Can lead to <strong>rounding</strong> (float only has 23 bits of fraction).

### Practice Puzzles
- `(float)x == (float)((int)((float)x))` : <strong>False</strong> (due to rounding/precision loss).
- `d + f - d == f` : <strong>False</strong> (Large `d` can "swamp" small `f`, causing $d+f=d$).