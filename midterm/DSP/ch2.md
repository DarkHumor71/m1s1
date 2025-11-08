# DSP Chapter 2 - Quick Study Guide

## Discrete-Time Signals and Systems

---

## 1. ELEMENTARY DISCRETE-TIME SIGNALS

### Unit Sample (Impulse) δ(n)

```
δ(n) = 1 for n = 0
δ(n) = 0 for n ≠ 0
```

### Unit Step u(n)

```
u(n) = 1 for n ≥ 0
u(n) = 0 for n < 0
```

**Relationship**: u(n) = Σδ(k) from k=0 to n

### Unit Ramp

```
r(n) = n·u(n) = n for n ≥ 0, 0 for n < 0
```

### Exponential

```
x(n) = a^n for all n (or a^n·u(n) for causal)
```

---

## 2. SIGNAL CLASSIFICATION

### Energy vs Power Signals

**Energy**:

```
E = Σ|x(n)|² from n=-∞ to +∞
```

**Average Power**:

```
P = lim[N→∞] (1/(2N+1)) Σ|x(n)|² from n=-N to N
```

**Classification**:

- **Energy Signal**: E < ∞ (then P = 0)
- **Power Signal**: E = ∞, P < ∞ (finite, non-zero)
- Neither: E = ∞, P = ∞

**Key Rule**: If E finite → P = 0; If P finite & non-zero → E = ∞

---

### Periodic vs Aperiodic

**Periodic**: x(n+N) = x(n) for all n

- N = fundamental period (smallest positive N)
- **Periodic signals are POWER signals**

**Average power of periodic signal**:

```
P = (1/N) Σ|x(n)|² from n=0 to N-1
```

(Average energy in one period)

**Aperiodic**: No N satisfies periodicity condition

---

### Even vs Odd Signals

**Even**: x(-n) = x(n) for all n
**Odd**: x(-n) = -x(n) for all n

**Decomposition**:

```
x(n) = x_e(n) + x_o(n)

x_e(n) = [x(n) + x(-n)]/2  (even part)
x_o(n) = [x(n) - x(-n)]/2  (odd part)
```

---

## 3. SIGNAL MANIPULATIONS

### Time Shifting

- **Delay**: x(n-k) where k > 0
- **Advance**: x(n+k) where k > 0

### Time Reversal (Folding)

- Replace n with -n: x(n) → x(-n)
- Reflection about origin n = 0

### Time Scaling (Down-sampling)

- Replace n with mn: x(n) → x(mn) where m is integer
- **Down-sampling by m**: Keep every m-th sample

---

## 4. DISCRETE-TIME SYSTEMS

### General Form

```
y(n) = T[x(n)]
```

- x(n): Input (excitation)
- y(n): Output (response)
- T[·]: Transformation operator

### Block Diagram Elements

- **Adder**: Sums signals
- **Multiplier**: Multiplies by constant
- **Delay**: D or z^(-1), shifts by one sample
- **Unit delay**: y(n) = x(n-1)

---

## 5. SYSTEM CLASSIFICATION (CRITICAL!)

### 1. Static vs Dynamic (Memory)

**Static (Memoryless)**:

- Output depends ONLY on current input
- Example: y(n) = 2x(n) + 3

**Dynamic (Has Memory)**:

- Output depends on past/future inputs or past outputs
- Example: y(n) = x(n) + x(n-1)

---

### 2. Time-Invariant vs Time-Variant

**Time-Invariant**:

- Properties constant over time
- If x(n) → y(n), then x(n-k) → y(n-k)
- Example: y(n) = 2x(n) + 3 ✓

**Time-Variant**:

- Properties change with time
- Example: y(n) = n·x(n) ✗

**Test**: Delay input, check if output is equally delayed

---

### 3. Linear vs Nonlinear (SUPER IMPORTANT!)

**Linear System** satisfies:

1. **Additivity**: T[x₁(n) + x₂(n)] = T[x₁(n)] + T[x₂(n)]
2. **Homogeneity**: T[a·x(n)] = a·T[x(n)]

**Superposition**: T[ax₁(n) + bx₂(n)] = aT[x₁(n)] + bT[x₂(n)]

**Examples**:

- Linear: y(n) = 2x(n) ✓
- Nonlinear: y(n) = x²(n) ✗ (violates homogeneity)
- Nonlinear: y(n) = x(n) + 3 ✗ (constant term!)

---

### 4. Causal vs Non-Causal

**Causal**:

- Output depends ONLY on present & past inputs
- No future values
- Example: y(n) = x(n) + x(n-1) ✓

**Non-Causal**:

- Depends on future inputs
- Example: y(n) = x(n+1) + x(n) ✗

**General causal form**:

```
y(n) = F[x(n), x(n-1), ..., y(n-1), y(n-2), ...]
```

---

### 5. Stable vs Unstable (BIBO)

**BIBO Stable**: Bounded Input → Bounded Output

**Condition**: For all n,

```
|x(n)| < M  →  |y(n)| < N
```

Where M, N are finite constants

**Instability**: ∃ bounded input where output is unbounded/oscillatory

---

## 6. SYSTEM INTERCONNECTIONS

### Cascade (Series)

```
x(n) → [System 1] → [System 2] → y(n)
y(n) = T₂[T₁[x(n)]]
```

### Parallel

```
       ┌─[System 1]─┐
x(n) ──┤            ├─→ Σ → y(n)
       └─[System 2]─┘
y(n) = T₁[x(n)] + T₂[x(n)]
```

---

## 7. LTI SYSTEMS (Linear Time-Invariant)

### Why Important?

- Simplifies analysis
- Characterized completely by impulse response h(n)
- Convolution describes input-output relationship

### Impulse Response h(n)

**Definition**: System output when input is δ(n) (system relaxed)

```
x(n) = δ(n)  →  y(n) = h(n)
```

---

## 8. CONVOLUTION SUM (CRITICAL!)

### Formula

```
y(n) = x(n) * h(n) = Σ x(k)h(n-k) from k=-∞ to +∞
```

OR equivalently:

```
y(n) = Σ h(k)x(n-k) from k=-∞ to +∞
```

### Computing Convolution (Step-by-Step)

**For y(n₀)**:

1. **Fold**: h(k) → h(-k)
2. **Shift**: h(-k) → h(n₀-k)
3. **Multiply**: x(k) × h(n₀-k)
4. **Sum**: y(n₀) = Σ x(k)h(n₀-k)

### Length of Convolution

If x(n) has N₁ terms and h(n) has N₂ terms:

```
y(n) has N = N₁ + N₂ - 1 terms
```

**Index range**:

- x(n): n₁_min ≤ n ≤ n₁_max
- h(n): n₂_min ≤ n ≤ n₂_max
- y(n): (n₁_min + n₂_min) ≤ n ≤ (n₁_max + n₂_max)

---

## 9. CONVOLUTION PROPERTIES

### Identity

```
x(n) * δ(n) = x(n)
x(n) * δ(n-k) = x(n-k)  (shifting)
```

### Commutative

```
x(n) * h(n) = h(n) * x(n)
```

### Associative

```
[x(n) * h₁(n)] * h₂(n) = x(n) * [h₁(n) * h₂(n)]
```

### Distributive

```
x(n) * [h₁(n) + h₂(n)] = x(n)*h₁(n) + x(n)*h₂(n)
```

---

## 10. LTI SYSTEM PROPERTIES

### Causality

**LTI system is causal IF AND ONLY IF**:

```
h(n) = 0 for n < 0
```

**For causal x(n) and h(n)**:

```
y(n) = Σ h(k)x(n-k) from k=0 to n
```

### Stability

**LTI system is BIBO stable IF AND ONLY IF**:

```
Σ|h(k)| < ∞ from k=-∞ to +∞
```

(Impulse response is **absolutely summable**)

**This is necessary AND sufficient!**

---

## 11. FIR vs IIR SYSTEMS

### FIR (Finite Impulse Response)

```
y(n) = Σ b_k x(n-k) from k=0 to M
```

- **Finite memory**: M samples
- **Always stable** (finite h(n))
- **Can be non-recursive**
- Example: y(n) = x(n) + x(n-1) + x(n-2)

### IIR (Infinite Impulse Response)

```
y(n) = Σ a_k y(n-k) + Σ b_k x(n-k)
```

- **Infinite memory**
- **May be unstable**
- **Requires recursion**
- Example: y(n) = 0.5y(n-1) + x(n)

---

## 12. DIFFERENCE EQUATIONS

### General Form

```
Σ a_k y(n-k) = Σ b_k x(n-k)
k=0 to N         k=0 to M
```

**Standard form**:

```
y(n) = Σ b_k x(n-k) - Σ a_k y(n-k)
       k=0 to M      k=1 to N
```

### Recursive vs Non-Recursive

**Non-Recursive** (FIR):

```
y(n) = Σ b_k x(n-k)
```

Output depends ONLY on inputs

**Recursive** (IIR):

```
y(n) = Σ b_k x(n-k) + Σ c_k y(n-k)
```

Output depends on inputs AND past outputs

---

## 13. SOLVING DIFFERENCE EQUATIONS

### Method 1: Direct Method

**Total Solution**:

```
y(n) = y_h(n) + y_p(n)
```

**Homogeneous Solution y_h(n)**:

1. Set input to zero
2. Find characteristic polynomial
3. Find roots {λ₁, λ₂, ..., λ_N}
4. Form solution:
   - Simple root λᵢ: Cᵢλᵢⁿ
   - Root of order m: (C₁ + C₂n + ... + C_m n^(m-1))λᵢⁿ

**Particular Solution y_p(n)**:
Depends on form of input x(n)

| Input x(n) | Try y_p(n)   |
| ---------- | ------------ |
| δ(n)       | 0            |
| Constant A | A constant   |
| Aⁿ         | B·Aⁿ         |
| n·Aⁿ       | (B₀ + B₁n)Aⁿ |

**Find constants**: Use initial conditions

---

### Method 2: Iterative Method

**Example**: y(n) = ay(n-1) + x(n)

Starting from n=0:

```
y(0) = ay(-1) + x(0)
y(1) = ay(0) + x(1) = a²y(-1) + ax(0) + x(1)
y(2) = ay(1) + x(2) = a³y(-1) + a²x(0) + ax(1) + x(2)
...
```

**Zero-state response** (y(-1) = 0):

```
y_zs(n) = Σ a^k x(n-k) from k=0 to n
        = x(n) * h(n)  where h(n) = aⁿu(n)
```

**Zero-input response** (x(n) = 0):

```
y_zi(n) = aⁿ y(-1)
```

**Total response**:

```
y(n) = y_zi(n) + y_zs(n)
```

---

## 14. IMPULSE RESPONSE FROM DE

**For LTI recursive system**:

- Input: x(n) = δ(n)
- System initially relaxed
- Output: h(n) = impulse response

**Example**: y(n) = 0.5y(n-1) + x(n)

```
h(n) = (0.5)ⁿ u(n)
```

---

## 15. STABILITY FROM CHARACTERISTIC ROOTS

**For causal LTI system described by DE**:

**BIBO Stable IF AND ONLY IF**:

```
|λᵢ| < 1 for ALL roots
```

**Unstable IF**:

```
|λᵢ| ≥ 1 for one or more roots
```

**Location**:

- **Inside unit circle**: Stable
- **On or outside unit circle**: Unstable

---

## 16. SYSTEM IMPLEMENTATION

### Direct Form I

```
      [Feed-forward (zeros)] → [Feed-backward (poles)]
x(n) → Σb_k z^(-k) → w(n) → Σa_k z^(-k) → y(n)
```

- Two delay chains
- M + N delays total

### Direct Form II (Canonical)

```
      [Feed-backward] → [Feed-forward]
x(n) → Σa_k z^(-k) → w(n) → Σb_k z^(-k) → y(n)
```

- One delay chain
- max(M, N) delays
- **More efficient** (fewer delays)

---

## 17. CORRELATION

### Purpose

Measure **similarity** between two signals

### Applications

- Signal detection
- Radar/sonar (noisy environments)
- Target distance measurement
- Channel multiplexing

### Cross-Correlation

```
r_xy(l) = Σ x(n)y(n+l) from n=-∞ to +∞
```

- l = lag (time shift)
- Peak indicates similarity at that lag

### Auto-Correlation

```
r_xx(l) = Σ x(n)x(n+l)
```

- Measure of signal with itself
- Maximum at l = 0
- Symmetric: r_xx(l) = r_xx(-l)

---

## KEY FORMULAS SUMMARY

### Energy & Power

```
E = Σ|x(n)|²
P = lim[N→∞] (1/(2N+1)) Σ|x(n)|²
P_periodic = (1/N) Σ|x(n)|² over one period
```

### Even/Odd Decomposition

```
x_e(n) = [x(n) + x(-n)]/2
x_o(n) = [x(n) - x(-n)]/2
```

### Convolution

```
y(n) = Σ x(k)h(n-k) = Σ h(k)x(n-k)
Length: N = N₁ + N₂ - 1
```

### LTI Properties

```
Causality: h(n) = 0 for n < 0
Stability: Σ|h(n)| < ∞
```

### Difference Equation

```
y(n) = -Σa_k y(n-k) + Σb_k x(n-k)
```

---

## EXAM ESSENTIALS

### Definitions to Memorize

1. Energy vs Power signal
2. Even vs Odd signal
3. Causal system
4. Stable system (BIBO)
5. LTI system
6. Impulse response
7. Convolution
8. FIR vs IIR

### Key Concepts

1. **Linearity test**: Check superposition (additivity + homogeneity)
2. **Time-invariance test**: Delay input, check if output delayed equally
3. **Causality**: h(n) = 0 for n < 0
4. **Stability**: Σ|h(n)| < ∞
5. **Convolution length**: N₁ + N₂ - 1
6. **Characteristic roots inside unit circle** → Stable

### Common Exam Questions

1. Classify system (static/dynamic, linear/nonlinear, etc.)
2. Test linearity (use superposition)
3. Compute convolution (graphical or analytical)
4. Find even/odd components
5. Solve difference equation
6. Find impulse response from DE
7. Check stability (from h(n) or roots)
8. Determine if signal is energy or power
9. Draw system block diagram
10. Compute correlation

---

## PROBLEM-SOLVING STRATEGIES

### Testing Linearity

1. Apply input x₁(n) → get y₁(n)
2. Apply input x₂(n) → get y₂(n)
3. Apply input ax₁(n)+bx₂(n) → get y₃(n)
4. Check if y₃(n) = ay₁(n) + by₂(n)
5. **If yes**: Linear ✓, **If no**: Nonlinear ✗

### Graphical Convolution

1. Sketch x(k) and h(k)
2. Fold h(k) to get h(-k)
3. For each n:
   - Shift h(-k) to get h(n-k)
   - Multiply x(k) × h(n-k)
   - Sum all products
4. Repeat for all n

### Solving First-Order DE

**Example**: y(n) - 0.5y(n-1) = x(n)

1. **Homogeneous**: λ - 0.5 = 0 → λ = 0.5

   - y_h(n) = C(0.5)ⁿ

2. **Particular**: Depends on x(n)

3. **Total**: y(n) = C(0.5)ⁿ + y_p(n)

4. **Find C**: Use initial condition

---

## COMMON MISTAKES TO AVOID

1. **Constant term in linear test**: y(n) = x(n) + 3 is **NOT linear!**
2. **Forgetting absolute value in stability**: Σ|h(n)| not Σh(n)
3. **Convolution index confusion**: Use dummy variable k
4. **Wrong convolution length**: N₁ + N₂ - 1 (not N₁ × N₂)
5. **Causality vs stability**: Different concepts!
6. **Initial conditions**: Zero-state vs zero-input responses
7. **Characteristic equation**: From homogeneous part only
8. **Unit circle test**: |λ| < 1 for stability (strictly less than!)

---

**Pro Tip**: Master CONVOLUTION (both graphical and analytical) and SYSTEM CLASSIFICATION (the 5 properties) - they're fundamental to everything in DSP! Also, practice DIFFERENCE EQUATIONS extensively - they appear in every exam!
