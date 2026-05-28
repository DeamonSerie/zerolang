# ZeroX Math Library — User Guide

## Overview

The `std.math` library provides a comprehensive set of mathematical operations for ZeroX programs — all written in pure ZeroX `.0` source code with `f64` (double-precision floating-point) as the primary numeric type.

The library is organized into eleven sub-modules:

| Module | File | Purpose |
|--------|------|---------|
| `std.math` | `math.0` | Constants, comparisons, basic utilities |
| `std.math.algebra` | `algebra.0` | Polynomials, trig, exp/log, roots, GCD/LCM, hyperbolic functions |
| `std.math.calculus` | `calculus.0` | Numerical differentiation and integration |
| `std.math.matrix` | `matrix.0` | Matrix ops, determinant, inverse, LU/QR/Cholesky solve |
| `std.math.numerical` | `numerical.0` | Root-finding, interpolation, regression, special functions |
| `std.math.complex` | `complex.0` | Complex number arithmetic and transcendental functions |
| `std.math.vector` | `vector.0` | 2D/3D vector math: dot, cross, normalize, reflect, refract |
| `std.math.stats` | `stats.0` | Moments, hypothesis tests, probability distributions |
| `std.math.ode` | `ode.0` | ODE solvers: Euler, RK4, Adams-Bashforth |
| `std.math.optim` | `optim.0` | 1D optimization: golden-section, Brent, gradient descent |
| `std.math.quaternion` | `quaternion.0` | Quaternion ops, rotation, spherical interpolation (slerp) |
| `std.math.debug` | `debug.0` | Computation tracing and debugging |

---

## Getting Started

Import the desired sub-module at the top of your `.0` file:

```
use std.math
use std.math.algebra
use std.math.calculus
use std.math.matrix
use std.math.numerical
use std.math.complex
use std.math.vector
use std.math.stats
use std.math.ode
use std.math.optim
use std.math.quaternion
use std.math.debug
```

All public functions are accessed via their fully-qualified path:

```
let x f64 std.math.algebra.sin std.math.PI_2
```

---

## Core Module (`std.math`)

### Constants

| Constant | Value | Description |
|----------|-------|-------------|
| `PI` | 3.14159265358979323846 | π |
| `TAU` | 6.28318530717958647692 | 2π |
| `E` | 2.71828182845904523536 | e |
| `LN2` | 0.69314718055994530942 | ln(2) |
| `LN10` | 2.30258509299404568402 | ln(10) |
| `LOG2E` | 1.44269504088896340736 | log₂(e) |
| `LOG10E` | 0.43429448190325182765 | log₁₀(e) |
| `SQRT2` | 1.41421356237309504880 | √2 |
| `SQRT1_2` | 0.70710678118654752440 | 1/√2 |
| `SQRT3` | 1.73205080756887729353 | √3 |
| `PHI` | 1.61803398874989484820 | φ (golden ratio) |
| `PI_2` | 1.57079632679489661923 | π/2 |
| `PI_4` | 0.78539816339744830962 | π/4 |
| `EPSILON` | 2.220446049250313e-16 | Machine epsilon for f64 |
| `MAX` | 1.7976931348623157e308 | Maximum finite f64 |
| `MIN` | 2.2250738585072014e-308 | Minimum positive normalized f64 |
| `DEFAULT_TOLERANCE` | 1e-12 | Default ε for comparisons |
| `LOOSE_TOLERANCE` | 1e-6 | Loose tolerance |
| `GAMMA` | 0.57721566490153286060 | Euler-Mascheroni constant (γ) |
| `CATALAN` | 0.91596559417721901505 | Catalan's constant (G) |
| `APERY` | 1.20205690315959428540 | Apéry's constant ζ(3) |

### Comparison Functions

- `approxEq(a, b, tolerance)` — Check approximate equality within tolerance
- `approxEqDefault(a, b)` — Check with DEFAULT_TOLERANCE
- `isZero(value)` — Check if ≈ 0
- `isZeroTol(value, tolerance)` — Check if ≈ 0 with custom tolerance

### Utility Functions

- `clamp(value, min, max)` — Constrain to [min, max]
- `sign(value)` — Return -1.0, 0.0, or 1.0
- `signTol(value, tolerance)` — Sign with tolerance for near-zero values
- `min(a, b)`, `max(a, b)`, `min3(a, b, c)`, `max3(a, b, c)`
- `lerp(a, b, t)` — Linear interpolation
- `mapRange(value, inMin, inMax, outMin, outMax)`
- `degToRad(degrees)`, `radToDeg(radians)`
- `step(x)`, `smoothStep(x)` — Step functions
- `factorial(n)` — n! as f64
- `abs(x)`, `sq(x)`, `cube(x)`
- `powi(base, exp)` — Integer exponent (exponentiation by squaring)
- `arraySum(values, start, count)` — Sum over Span<f64>

---

## Algebra Module (`std.math.algebra`)

### Polynomial Operations

- `polyEval(coeffs, degree, x)` — Horner's method evaluation
- `polyEvalWithDerivative(coeffs, degree, x)` — Returns `PolyValue { f, df }`
- `polyVal(coeffs, numCoeffs, x)` — Convenience wrapper

### Equation Solving

- `solveQuadratic(a, b, c, roots)` — Real roots of ax² + bx + c = 0
- `solveQuadraticRobust(a, b, c, roots)` — Handles degenerate cases (a=0)
- `solveDepressedCubic(p, q, roots)` — Real roots of x³ + px + q = 0

### Elementary Functions

- `sqrt(x)` — Square root via Newton's method
- `cbrt(x)` — Cube root via Newton's method
- `sin(x)`, `cos(x)`, `tan(x)` — Trigonometric (Taylor series)
- `asin(x)`, `acos(x)`, `atan(x)`, `atan2(y, x)` — Inverse trig
- `exp(x)` — eˣ via Taylor series
- `exp2(x)` — 2ˣ
- `exp10(x)` — 10ˣ
- `expm1(x)` — eˣ − 1 (accurate for small x)
- `ln(x)` — Natural log via AGM method
- `log10(x)`, `log2(x)` — Base-10 and base-2 logs
- `log1p(x)` — ln(1 + x) (accurate for small x)
- `pow(a, b)` — Real exponent aᵇ
- `hypot(x, y)` — √(x² + y²) (avoids unnecessary overflow)
- `logb(x)` — Extract exponent as f64 (floor(log₂|x|))
- `ilogb(x)` — Extract exponent as i32

### Hyperbolic Functions

- `sinh(x)`, `cosh(x)`, `tanh(x)` — Hyperbolic sine, cosine, tangent
- `asinh(x)`, `acosh(x)`, `atanh(x)` — Inverse hyperbolic functions

### Reciprocal Trigonometric Functions

- `csc(x)`, `sec(x)`, `cot(x)` — Cosecant, secant, cotangent
- `acsc(x)`, `asec(x)`, `acot(x)` — Inverse reciprocal trig

### Number Theory

- `gcd(a, b)` — Greatest common divisor (i64)
- `lcm(a, b)` — Least common multiple (i64)
- `binomial(n, k)` — Binomial coefficient C(n, k) as f64

### Other

- `agm(a, b)` — Arithmetic-geometric mean

---

## Calculus Module (`std.math.calculus`)

All operations are numerical approximations requiring a function `(fn f64) f64`.

### Differentiation

- `derivative(fn, x, h)` — First derivative (central difference)
- `derivative5(fn, x, h)` — First derivative (5-point stencil)
- `secondDerivative(fn, x, h)` — Second derivative
- `secondDerivative5(fn, x, h)` — Second derivative (5-point stencil)
- `thirdDerivative(fn, x, h)` — Third derivative
- `diffData(y, count, h, out)` — Derivative from sampled data

### Integration

- `integrateTrapezoid(fn, a, b, n)` — Trapezoidal rule
- `integrateSimpson(fn, a, b, n)` — Simpson's rule (n must be even)
- `integrateSimpson38(fn, a, b, n)` — Simpson's 3/8 rule
- `integrateAdaptive(fn, a, b, tolerance, maxDepth)` — Adaptive Simpson
- `integrateRomberg(fn, a, b, order)` — Romberg integration (returns `IntegrateResult`)
- `riemannLeft(fn, a, b, n)`, `riemannRight(fn, a, b, n)`, `riemannMidpoint(fn, a, b, n)`

### Types

- `IntegrateResult` — Contains `value f64` and `errorEstimate f64`

---

## Matrix Module (`std.math.matrix`)

Matrices are stored in row-major order as flat `[M*N]f64` arrays and accessed via `Matrix`/`MatrixMut` view types.

### Types

- `Matrix` — `{ rows u32, cols u32, data Span<f64>, cap usize }`
- `MatrixMut` — `{ rows u32, cols u32, data MutSpan<f64>, cap usize }`
- `LUResult` — `{ ok Bool, pivot u32 }`

### Construction

- `matrixView(rows, cols, data)` — Create Matrix from Span
- `matrixMutView(rows, cols, data)` — Create MatrixMut from MutSpan

### Element Access

- `get(mat, row, col)` — Get element
- `set(mat, row, col, value)` — Set element

### Basic Operations

- `add(a, b, out)` — Element-wise addition
- `sub(a, b, out)` — Element-wise subtraction
- `scale(a, scalar, out)` — Scalar multiplication
- `mul(a, b, out)` — Matrix multiplication (sparse-entry optimized)
- `transpose(a, out)` — Matrix transpose

### Norms

- `normFrobenius(mat)` — Frobenius norm (√Σaᵢⱼ²)
- `normInf(mat)` — Infinity norm (max row sum)

### Determinant & Inverse

- `det2(a)` — 2×2 determinant
- `det3(a)` — 3×3 determinant
- `inv2(a, out)` — 2×2 inverse
- `inv3(a, out)` — 3×3 inverse

### Linear Systems

- `identity(n, out)` — Fill n×n identity matrix
- `trace(mat)` — Sum of diagonal elements
- `lu(a, lufactor, perm, permCount)` — PA = LU decomposition (partial pivoting)
- `forwardSub(lufactor, b, y)` — Forward substitution (Ly = b)
- `backSub(lufactor, y, x)` — Back substitution (Ux = y)
- `solve(a, b, x, lufactor, perm, y)` — Solve Ax = b

### Advanced Decompositions

- `qr(a, q, r, rows, cols)` — QR decomposition via Gram-Schmidt
- `cholesky(a, l, n)` — Cholesky decomposition (symmetric positive-definite)
- `powerIteration(a, v, n, tol, maxIter)` — Dominant eigenvalue (power iteration)
- `conjugateGradient(a, b, x, n, tol, maxIter)` — Conjugate gradient solver (sparse systems)

---

## Numerical Analysis Module (`std.math.numerical`)

### Root Finding

- `bisect(fn, a, b, tol)` — Bisection method
- `newton(fn, dfn, guess, tol, maxIter)` — Newton's method
- `secant(fn, x0, x1, tol, maxIter)` — Secant method

### Interpolation

- `interpolateLinear(xData, yData, count, x)` — Piecewise linear
- `interpolateLagrange(xData, yData, count, x)` — Lagrange polynomial

### Curve Fitting

- `linearRegression(xData, yData, count)` — Returns `LinearFit { a, b, r2 }`
- `polyFit2(xData, yData, count, coeffs)` — Quadratic polynomial fit

### Types

- `LinearFit` — `{ a f64, b f64, r2 f64 }`

### Finite Differences

- `forwardDiff(fn, x, h)` — Forward difference
- `backwardDiff(fn, x, h)` — Backward difference
- `fdCoeffs1(stencil, h, coeffs)` — Finite difference coefficients
- `trapz(y, count, h)` — Trapezoidal integration of discrete data

### Special Functions

- `sinc(x)` — sin(πx) / (πx), with sinc(0) = 1
- `erf(x)` — Error function (Abramowitz & Stegun approximation)
- `erfc(x)` — Complementary error function
- `gamma(x)` — Gamma function (Lanczos approximation)
- `beta(x, y)` — Beta function
- `digamma(x)` — Digamma function ψ(x)
- `trigamma(x)` — Trigamma function ψ₁(x)
- `lowerIncompleteGamma(s, x)` — Lower incomplete gamma γ(s, x)
- `upperIncompleteGamma(s, x)` — Upper incomplete gamma Γ(s, x)
- `incompleteBeta(x, a, b)` — Incomplete beta function Iₓ(a, b)

### Bessel Functions

| Function | Description |
|----------|-------------|
| `besselJ0(x)` | Bessel J₀ (Miller's downward recurrence) |
| `besselJ1(x)` | Bessel J₁ |
| `besselJ(n, x)` | Bessel Jₙ (integer order) |
| `besselY0(x)` | Bessel Y₀ (x > 0) |
| `besselY1(x)` | Bessel Y₁ |
| `besselY(n, x)` | Bessel Yₙ |
| `besselI0(x)` | Modified Bessel I₀ |
| `besselI1(x)` | Modified Bessel I₁ |
| `besselI(n, x)` | Modified Bessel Iₙ |
| `besselK0(x)` | Modified Bessel K₀ (x > 0) |
| `besselK1(x)` | Modified Bessel K₁ |
| `besselK(n, x)` | Modified Bessel Kₙ |

### Transcendental Functions

- `lambertW(x)` — Lambert W function (principal branch)
- `zeta(s)` — Riemann zeta function ζ(s) (real s > 0, s ≠ 1)

### Elliptic Integrals

| Function | Description |
|----------|-------------|
| `ellipticK(k)` | Complete elliptic integral of the first kind K(k) |
| `ellipticE(k)` | Complete elliptic integral of the second kind E(k) |
| `ellipticPi(n, k)` | Complete elliptic integral of the third kind Π(n, k) |
| `ellipticPiIncomplete(phi, n, k)` | Incomplete elliptic integral of the third kind Π(φ, n, k) |

### Types

- `EllipticPiResult` — `{ ok Bool, value f64 }`

---

## Complex Number Module (`std.math.complex`)

Complex numbers use the `Complex` type with `.real` and `.imag` fields.

### Type

- `Complex` — `{ real f64, imag f64 }`

### Construction

- `cmp(real, imag)` — Create from real and imaginary parts
- `cmpReal(real)` — Create real-only complex number
- `cmpImag(imag)` — Create pure imaginary number
- `fromPolar(r, theta)` — Create from polar coordinates: r · e^(iθ)

### Conversion

- `toPolar(a)` — Convert to polar form, returns `Polar { r f64, theta f64 }`

### Basic Arithmetic

- `cadd(a, b)` — Complex addition
- `csub(a, b)` — Complex subtraction
- `cmul(a, b)` — Complex multiplication (FOIL)
- `cdiv(a, b)` — Complex division
- `cneg(a)` — Complex negation
- `cscale(a, s)` — Scale by real scalar

### Unary Operations

- `conj(a)` — Complex conjugate
- `cmod(a)` — Modulus |z|
- `carg(a)` — Argument (phase angle)

### Transcendental Functions

- `cexp(a)` — Complex exponential e^z
- `clog(a)` — Complex natural logarithm ln(z)
- `cpow(a, b)` — Complex power a^b
- `csqrt(a)` — Complex square root

### Complex Trigonometric

- `csin(a)`, `ccos(a)`, `ctan(a)` — Complex sine, cosine, tangent
- `casin(a)`, `cacos(a)`, `catan(a)` — Complex inverse trig

### Complex Hyperbolic

- `csinh(a)`, `ccosh(a)`, `ctanh(a)` — Complex hyperbolic sine, cosine, tangent
- `casinh(a)`, `cacosh(a)`, `catanh(a)` — Complex inverse hyperbolic

---

## Vector Module (`std.math.vector`)

2D and 3D vector mathematics. All vectors use `f64` components.

### 2D Vector (`Vec2`)

- `vec2(x, y)` — Create a 2D vector
- `vec2Zero` — Zero vector (0, 0)
- `add2(a, b)` — Vector addition
- `sub2(a, b)` — Vector subtraction
- `scale2(a, s)` — Scalar multiplication
- `dot2(a, b)` — Dot product
- `length2(a)` — Magnitude
- `lengthSq2(a)` — Squared magnitude (avoids sqrt)
- `normalize2(a)` — Unit vector (returns zero if input is zero)
- `distance2(a, b)` — Distance between two points
- `reflect2(v, n)` — Reflect about surface normal
- `lerp2(a, b, t)` — Linear interpolation

### 3D Vector (`Vec3`)

- `vec3(x, y, z)` — Create a 3D vector
- `vec3Zero` — Zero vector (0, 0, 0)
- `add3(a, b)` — Vector addition
- `sub3(a, b)` — Vector subtraction
- `scale3(a, s)` — Scalar multiplication
- `dot3(a, b)` — Dot product
- `cross3(a, b)` — Cross product
- `length3(a)` — Magnitude
- `lengthSq3(a)` — Squared magnitude (avoids sqrt)
- `normalize3(a)` — Unit vector
- `distance3(a, b)` — Distance between two points
- `reflect3(v, n)` — Reflect about surface normal
- `refract3(v, n, eta)` — Refract using Snell's law
- `lerp3(a, b, t)` — Linear interpolation

---

## Statistics Module (`std.math.stats`)

### Descriptive Statistics

- `mean(data, count)` — Arithmetic mean
- `variance(data, count)` — Sample variance (Bessel's correction, n−1)
- `variancePop(data, count)` — Population variance (n)
- `stddev(data, count)` — Sample standard deviation
- `stddevPop(data, count)` — Population standard deviation
- `skewness(data, count)` — Skewness (measure of asymmetry)
- `kurtosis(data, count)` — Excess kurtosis (measure of tailedness)

### Probability Distributions

| Function | Description |
|----------|-------------|
| `normalPDF(x)` | Standard normal PDF φ(x) |
| `normalPDFParam(x, mu, sigma)` | Normal PDF with parameters |
| `normalCDF(x)` | Standard normal CDF Φ(x) (via erf) |
| `normalCDFParam(x, mu, sigma)` | Normal CDF with parameters |
| `uniformPDF(x, a, b)` | Uniform PDF on [a, b] |
| `uniformCDF(x, a, b)` | Uniform CDF on [a, b] |
| `exponentialPDF(x, lambda)` | Exponential PDF |
| `exponentialCDF(x, lambda)` | Exponential CDF |

### Hypothesis Tests

| Function | Description |
|----------|-------------|
| `tTestOneSample(data, count, mu0)` | One-sample t-test |
| `tTest(data1, count1, data2, count2)` | Independent two-sample t-test (equal variance) |
| `tTestWelch(data1, count1, data2, count2)` | Welch's t-test (unequal variances) |
| `tTestPaired(data1, data2, count)` | Paired t-test |
| `chiSquaredTest(observed, expected, count)` | Chi-squared goodness-of-fit |
| `chiSquaredIndependence(table, rows, cols)` | Chi-squared test of independence |

### Types

- `HypothesisResult` — `{ tStat f64, df f64, pValue f64 }`
- `ChiResult` — `{ chiStat f64, df f64, pValue f64 }`

---

## ODE Module (`std.math.ode`)

Numerical integration of ordinary differential equations: dy/dt = f(t, y), y(t₀) = y₀.

### Single-Step Methods

- `eulerStep(fn, t, y, h)` — One Euler step
- `solveEuler(fn, t0, y0, h, t1)` — Euler method to t1
- `solveEulerRecord(fn, t0, y0, h, n, tOut, yOut)` — Euler with step recording
- `rk4Step(fn, t, y, h)` — One classical RK4 step
- `solveRK4(fn, t0, y0, h, t1)` — RK4 to t1
- `solveRK4Record(fn, t0, y0, h, n, tOut, yOut)` — RK4 with step recording

### Multi-Step Methods

- `adamsBashforth2Step(fn, t, y, h, fPrev)` — Adams-Bashforth 2nd-order step
- `solveAdamsBashforth2(fn, t0, y0, h, t1)` — AB2 solver (bootstrap via RK4)
- `adamsBashforth4Step(fn, t, y, h, f1, f2, f3)` — Adams-Bashforth 4th-order step

### System Solvers (Vector State)

- `systemEulerStep(fn, t, h, n, y, dy)` — Euler step for vector ODEs
- `solveSystemEuler(fn, t0, y0, n, h, steps, tOut, yOut, dy)` — Euler for systems with recording
- `systemRK4Step(fn, t, h, n, y, k1, k2, k3, k4, temp, dy)` — RK4 step for vector ODEs
- `solveSystemRK4(fn, t0, y0, n, h, steps, tOut, yOut, k1, k2, k3, k4, temp, dy)` — RK4 for systems

### Types

- `AB2Step` — `{ yNext f64, fCurr f64 }`
- `AB4Step` — `{ yNext f64, fCurr f64 }`

---

## Optimization Module (`std.math.optim`)

### 1D Methods

- `goldenSection(fn, a, b, tol)` — Golden-section search (unimodal, no derivatives)
- `brent(fn, a, b, tol)` — Brent's method (parabolic + golden-section)
- `gradientDescent(fn, dfn, guess, alpha, tol, maxIter)` — Gradient descent (1D)
- `nelderMead1D(fn, x0, x1, tol, maxIter)` — Nelder-Mead simplex (1D)

---

## Quaternion Module (`std.math.quaternion`)

Quaternions use the `Quaternion` type: q = w + xi + yj + zk where w is the scalar part and (x, y, z) is the vector part.

### Type

- `Quaternion` — `{ w f64, x f64, y f64, z f64 }`

### Construction

- `quat(w, x, y, z)` — Create from components
- `quatIdentity` — Identity quaternion (no rotation)
- `fromAxisAngle(axisX, axisY, axisZ, angle)` — Quaternion from axis-angle

### Basic Operations

- `quatAdd(a, b)` — Quaternion addition
- `quatSub(a, b)` — Quaternion subtraction
- `quatMul(a, b)` — Hamilton product
- `quatScale(a, s)` — Scalar multiplication
- `quatNeg(a)` — Quaternion negation
- `quatConj(a)` — Conjugate
- `quatNorm(a)` — Norm (magnitude)
- `quatNormSq(a)` — Squared norm
- `quatNormalize(a)` — Unit quaternion
- `quatInv(a)` — Inverse (q⁻¹ = conj(q) / |q|²)

### Rotation Operations

- `rotateVector(q, vx, vy, vz)` — Rotate a 3D vector by quaternion
- `toAxisAngle(q)` — Convert to axis-angle representation

### Interpolation

- `slerp(a, b, t)` — Spherical linear interpolation

### Types

- `Vector3D` — `{ x f64, y f64, z f64 }`
- `AxisAngle` — `{ x f64, y f64, z f64, angle f64 }`

---

## Debug Module (`std.math.debug`)

The debug module enables step-by-step tracing of mathematical computations without modifying the computation logic.

### Types

- `MathContext` — Debug context with trace log
- `TraceEntry` — `{ label f64, input f64, output f64, residual f64 }`
- `TraceLog` — `{ entries TraceEntry, capacity usize }`

### Functions

- `new(capacity)` — Create a new debug context
- `trace(ctx, label, input, output)` — Record step, returns output
- `traceExplicit(ctx, label, input, output, residual)` — Record with custom residual
- `stepCount(ctx)` — Number of recorded steps
- `hasConverged(ctx, tolerance)` — Check if latest step converged
- `reset(ctx)` — Clear all trace entries
- `debugEval(ctx, label, x, fn)` — Trace function evaluation
- `iterate(ctx, initial, maxIter, tolerance, stepFn)` — Trace iterative computation
- `printTrace(ctx, world)` — Print trace entries to stdout

### Debugging Pattern

The key design principle is that trace functions return their `output` argument unchanged. This allows insertion into expressions without affecting results:

```
# Without debug:
let x f64 / (+ x (/ a x)) 2.0_f64

# With debug (same result, traced):
let x f64 std.math.debug.trace (&mut ctx) (as f64 iter) x (/ (+ x (/ a x)) 2.0_f64)
```

---

## File Format (.0)

All math library modules are written in ZeroX `.0` format following the standard conventions:

- Comments begin with `#`
- Constants use `const name type value`
- Types use `type Name ...`
- Functions use `pub fn name returnType params...`
- Inline tests use `test "description" ... expect ...`
- Mutable parameters use `mutref<Type>`

---

## Examples

The following example files demonstrate each module:

| File | Description |
|------|-------------|
| `examples/math-algebra-demo.0` | Polynomials, trig, roots, logs |
| `examples/math-calculus-demo.0` | Derivatives, integrals, sampled data |
| `examples/math-matrix-demo.0` | Add, multiply, determinant, inverse |
| `examples/math-numerical-demo.0` | Root-finding, interpolation, regression, special functions |
| `examples/math-debug-demo.0` | Tracing Newton iteration, convergence monitoring |

Run with:

```sh
bin/zerox check examples/math-algebra-demo.0
bin/zerox test examples/math-algebra-demo.0
```

---

## Performance Considerations

- All math functions are pure ZeroX implementations — no native runtime helpers required.
- Elementary functions use Taylor series expansions with 15–20 terms for accuracy.
- Matrix multiplication uses the standard O(n³) algorithm with a sparse-entry optimization (skips zero elements in the left matrix).
- LU decomposition uses partial pivoting for numerical stability.
- QR decomposition uses Gram-Schmidt orthogonalization.
- Cholesky decomposition requires a symmetric positive-definite matrix.
- Bessel functions use Miller's downward recurrence for numerical stability.
- The debug module stores trace entries in flat arrays with bounded capacity — no runtime allocation on the tracing path.
- For performance-critical code, the trace calls can be conditionally compiled or removed during optimization.

---

## Integration with ZeroX

The library integrates seamlessly with ZeroX:

1. **No external dependencies** — Pure ZeroX code, no C extensions required
2. **Standard import mechanism** — Uses `std.math.*` namespace
3. **Type-safe API** — All functions use ZeroX's static type system
4. **Testable** — Inline `test` blocks verify correctness
5. **Debuggable** — The `debug` module enables step-by-step inspection
6. **Composable** — Functions from different modules can be combined freely
7. **.0 format compliant** — All source files follow the `.0` specification

---

## API Stability

The `std.math` library follows the same API stability conventions as the rest of the ZeroX standard library:
- All public symbols are considered "bootstrap-stable"
- Breaking changes will be documented in the changelog
- The library is designed to evolve as ZeroX's type system and capabilities grow
