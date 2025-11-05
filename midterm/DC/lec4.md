# EENG537 Lecture 4 - Quick Study Guide

## Quadrature Amplitude Modulation (QAM)

---

## WHY QAM?

**Problem with PAM**: Bandwidth inefficient

- |W(f)| is symmetric
- Requires bandwidth B for baseband
- Requires 2B for passband (copies at ±fc)

**QAM Solution**: Send TWO messages in SAME 2B bandwidth!

- Uses orthogonal carriers (cos and sin)
- Halves bandwidth requirements
- More clever modulation strategy

---

## 1. QAM MODULATION

### Basic Principle

**Send two messages simultaneously**:

```
v(t) = m₁(t)cos(2πfct + φ) - m₂(t)sin(2πfct + φ)
```

Where:

- m₁(t), m₂(t) = two different baseband messages
- fc = carrier frequency
- φ = fixed arbitrary transmitter carrier phase

### With Pulse Shaping

**Messages**:

```
mᵢ(t) = Σ sᵢ[k]·p(t - kT)
       k
```

Where i = 1 or 2

**Transmitted Signal**:

```
v(t) = Σ s₁[k]p(t-kT)cos(2πfct+φ) - Σ s₂[k]p(t-kT)sin(2πfct+φ)
      k                              k
```

### Complex Representation

**Complex message**:

```
m(t) = m₁(t) + j·m₂(t)
```

**Complex symbol**:

```
s[k] = s₁[k] + j·s₂[k]
```

**Transmitted signal (compact form)**:

```
v(t) = Re{m(t)·e^(j(2πfct+φ))}
```

---

## 2. QAM DIMENSIONS

### Two Axes

- **In-phase (I)**: s₁ axis (cosine component)
- **Quadrature (Q)**: s₂ axis (sine component)

### Two Interpretation Methods

#### Method 1: Two Separate Messages

- User 1 sends: s₁[k] ∈ {±1}
- User 2 sends: s₂[k] ∈ {±1}
- Together: 4-QAM constellation {±1 ± j}

#### Method 2: Joint Encoding

- Single message encoded jointly
- Example: 4-PAM {±1, ±3} → 4-QAM
  - +1 → +1 + j
  - -1 → -1 + j
  - +3 → +1 - j
  - -3 → -1 - j

**Assumption**: s₁[k] and s₂[k] are:

- Zero average
- Uncorrelated (average product = 0)

---

## 3. M-QAM CONSTELLATIONS

### Square QAM (M = 2^(2n))

**Examples**: M = 4, 16, 64, 256, ...

**Structure**: Can be represented as two √M-PAM transmissions

- Each dimension: √M-PAM
- Total symbols: M = (√M)²

**16-QAM Example**:

```
In-phase: {-3, -1, +1, +3}
Quadrature: {-3, -1, +1, +3}
Total: 16 points in 4×4 grid
```

### Rectangular QAM

- Popular due to simple receiver
- Can be seen as two orthogonal PAMs
- Receiver implements two detectors (one per dimension)

### Circular QAM

- Symbols on nested circles
- Different structure than rectangular
- Less common

---

## 4. QAM DEMODULATION (IDEAL)

### Assumptions

- Ideal channel
- Phase φ = 0
- Frequency and phase known at receiver

### Demodulation Process

**Received signal**:

```
v(t) = m₁(t)cos(2πfct) - m₂(t)sin(2πfct)
```

**I-channel (In-phase)**:

```
x₁(t) = v(t)·cos(2πfct)
      = m₁(t)cos²(2πfct) - m₂(t)sin(2πfct)cos(2πfct)
      = m₁(t)/2·[1 + cos(4πfct)] - m₂(t)/2·sin(4πfct)

After LPF: s₁(t) = m₁(t)/2
```

**Q-channel (Quadrature)**:

```
x₂(t) = v(t)·sin(2πfct)
      = m₁(t)cos(2πfct)sin(2πfct) - m₂(t)sin²(2πfct)
      = m₁(t)/2·sin(4πfct) - m₂(t)/2·[1 - cos(4πfct)]

After LPF(-x₂): s₂(t) = m₂(t)/2
```

### Block Diagram

```
v(t) → [×cos(2πfct)] → [LPF] → s₁(t) = m₁(t)/2
    ↓
    → [×sin(2πfct)] → [-1] → [LPF] → s₂(t) = m₂(t)/2
```

### Complex-Valued Demodulator

```
s(t) = s₁(t) + j·s₂(t) = (1/2)[m₁(t) + j·m₂(t)] = m(t)/2
```

---

## 5. PSK MODULATION

### Phase-Shift Keying (PSK)

**General Form**:

```
v(t) = g·Σ p(t-kT)cos(2πfct + α[k])
      k
```

Where α[k] is chosen from M possible phases

**Alternative Form**:

```
v(t) = g·Σ p(t-kT)[cos(α[k])cos(2πfct) - sin(α[k])sin(2πfct)]
      k
```

**Complex Symbol**:

```
s[k] = cos(α[k]) + j·sin(α[k])
```

### Phase Choices

```
α[k] ∈ {2πm/M}, m = 0, 1, 2, ..., M-1
```

**Constellation**: Symbols located on a CIRCLE (constant amplitude)

---

## 6. COMMON PSK SCHEMES

### BPSK (Binary PSK, M=2)

- Phases: {0, π}
- Equivalent to 2-PAM
- One-dimensional

### QPSK (Quaternary PSK, M=4)

- Phases: {π/4, 3π/4, 5π/4, 7π/4}
- OR: {0, π/2, π, 3π/2}
- Two bits per symbol
- **Same as 4-QAM** (different realization)

### 8-PSK (M=8)

- Phases: {0, π/4, π/2, 3π/4, π, 5π/4, 3π/2, 7π/4}
- Three bits per symbol
- Symbols on circle

**Gray Coding**: Used for phase assignment to minimize bit errors

---

## 7. ENERGY & MINIMUM DISTANCE

### Baseband PAM

**Symbol Energy**:

```
ε = s²ₘ·εₚ
```

Where εₚ = ∫p²(t)dt (pulse energy)

**Average Energy per Symbol**:

```
εavg = [(M²-1)/3]·εₚ
```

**Average Energy per Bit**:

```
εbavg = [(M²-1)/(3·log₂M)]·εₚ
```

**Minimum Distance**:

```
dmin = 2·√εₚ
```

**In terms of εbavg**:

```
dmin = √[12·log₂M/(M²-1)]·√εbavg
```

---

### Passband PAM

**Pulse Energy** (for cos² averaging):

```
εₚ ≈ (1/2)∫p²(t)dt  (half of baseband)
```

**All formulas same** but εₚ is halved

---

### Square QAM (M = 2^(2k))

**Average Energy per Symbol**:

```
εavg = [2(M-1)/3]·εₚ
```

**Average Energy per Bit**:

```
εbavg = [2(M-1)/(3·log₂M)]·εₚ
```

**Minimum Distance** (for amplitudes ±1, ±3, ..., ±(√M-1)):

```
dmin = 2·√εₚ
```

**In terms of εbavg**:

```
dmin = √[6·log₂M/(M-1)]·√εbavg
```

---

## 8. ENERGY EFFICIENCY COMPARISON

### Example: 16-QAM vs 16-PAM

**16-PAM** (M=16):

```
εavg = (16²-1)/3 = 85 units
εbavg = 85/4 = 21.25 units
dmin = 2
```

**16-QAM** (M=16):

```
εavg = 2(16-1)/3 = 10 units
εbavg = 10/4 = 2.5 units
dmin = 2
```

**Conclusion**:

- **εavg-QAM << εavg-PAM** (10 vs 85!)
- **dmin same** for both
- **16-QAM much more energy efficient** than 16-PAM

### Key Insight

**As M increases** (for constant εbavg):

- dmin decreases
- More susceptible to noise
- But: QAM always more efficient than PAM for same M

---

## 9. NON-IDEAL DEMODULATION

### Frequency and Phase Offsets

**General Case**:

```
Receiver uses: f₀ and θ
Transmitter uses: fc and φ
```

**Demodulator Output**:

```
s(t) = (1/2)·e^(j(2π(fc-f₀)t + φ-θ))·m(t)
```

### Three Cases

#### Case 1: f₀ = fc, θ = φ (IDEAL)

```
s(t) = m(t)/2
```

Perfect recovery (scaled)

#### Case 2: f₀ = fc, θ ≠ φ (Phase Error)

```
s(t) = (1/2)·e^(j(φ-θ))·m(t)
```

**Effect**: Constellation rotated by angle (φ-θ)
**Solution**: Need carrier phase recovery

#### Case 3: f₀ ≠ fc, θ = φ (Frequency Error)

```
s(t) = (1/2)·e^(j·2π(fc-f₀)t)·m(t)
```

**Effect**: Constellation spins at rate proportional to (fc-f₀)
**Solution**: Need carrier frequency recovery

### Geometric Interpretation

- **Multiplication by e^(jα)**: Rotation by angle α in complex plane
- **Time-varying phase**: Continuous rotation (spinning constellation)

---

## 10. QAM IMPLEMENTATION

### Two Methods

#### Method 1: Real (Sine/Cosine)

```matlab
vreal = sp1.*cos(2*pi*fc*t+th) - sp2.*sin(2*pi*fc*t+th)
```

#### Method 2: Complex

```matlab
vcomp = real((sp1+j*sp2).*exp(j*(2*pi*fc*t+th)))
```

**Both methods identical**: max(abs(vcomp-vreal)) = 0

### Matlab Implementation (qamcompare.m)

```matlab
s1 = pam(N,2);                    % Binary ±1
s2 = pam(N,2);                    % Binary ±1
ps = hamming(M);                  % Pulse shape
sp1 = filter(ps,1,s1up);          % Pulse shape I
sp2 = filter(ps,1,s2up);          % Pulse shape Q
v = real((sp1+j*sp2).*exp(j*2*pi*fc*t));  % Modulate
```

---

## 11. QAM DEMODULATION IMPLEMENTATION

### Process Steps

1. **Complex Mixing**:

```matlab
x = v.*exp(-j*(2*pi*f0*t+ph));
```

2. **Lowpass Filtering**:

```matlab
b = firls(l,f,a);     % Design LPF
s = filter(b,1,x);    % Apply filter
```

3. **Result**:

- Shifted in time (by filter length/2)
- Attenuated (by factor of 2)
- Otherwise: s ≈ mp (recovered message)

### Matlab Implementation (qamdemod.m)

```matlab
m = pam(N,2)+j*pam(N,2);                     % Complex symbols
mp = filter(ps,1,mup);                       % Pulse shape
v = real(mp.*exp(j*(2*pi*fc*t+th)));         % Modulate
x = v.*exp(-j*(2*pi*f0*t+ph));               % Demodulate
s = filter(b,1,x);                           % LPF
```

---

## KEY FORMULAS SUMMARY

### Data Rates (Same as PAM)

```
Symbol rate: Rs = 1/T
Bit rate: Rb = log₂M/T = log₂M·Rs
Bits per symbol: b = log₂M
```

### Energy (Baseband PAM)

```
εavg = [(M²-1)/3]·εₚ
εbavg = [(M²-1)/(3·log₂M)]·εₚ
dmin = 2√εₚ
```

### Energy (Square QAM)

```
εavg = [2(M-1)/3]·εₚ
εbavg = [2(M-1)/(3·log₂M)]·εₚ
dmin = 2√εₚ
```

### Modulation/Demodulation

```
QAM TX: v(t) = Re{m(t)·e^(j(2πfct+φ))}
QAM RX: s(t) = (1/2)·e^(j(2π(fc-f₀)t+φ-θ))·m(t)
Ideal: s(t) = m(t)/2
```

---

## IMPORTANT CONCEPTS

### QAM Advantages

1. **Bandwidth efficient**: Two messages in bandwidth of one
2. **Energy efficient**: Much better than PAM for same M
3. **Flexible**: Can implement as two PAMs or circular constellation
4. **Practical**: Widely used in standards (WiFi, LTE, cable modems)

### QAM vs PAM

- QAM halves bandwidth requirement
- QAM more energy efficient (especially for large M)
- QAM more complex receiver (two branches)
- PAM simpler but less efficient

### QAM vs PSK

- QAM: Variable amplitude, better energy efficiency
- PSK: Constant amplitude, simpler power amplifier
- QPSK = 4-QAM (same thing, different view)
- For M>4: QAM typically better than PSK

### Carrier Recovery Critical

- Phase error → constellation rotation
- Frequency error → spinning constellation
- Both prevent proper symbol detection
- Need carrier synchronization!

---

## EXAM ESSENTIALS

### Formulas to Memorize

1. **v(t) = m₁(t)cos(2πfct+φ) - m₂(t)sin(2πfct+φ)**
2. **εavg-QAM = 2(M-1)εₚ/3**
3. **εavg-PAM = (M²-1)εₚ/3**
4. **dmin = 2√εₚ** (for both PAM and QAM)
5. **s(t) = (1/2)e^(j(φ-θ))m(t)** (phase error case)

### Key Concepts

1. QAM sends two messages in same bandwidth (orthogonal carriers)
2. In-phase (I) and Quadrature (Q) components
3. Square QAM: M = 2^(2n), two √M-PAM
4. QPSK = 4-QAM (same constellation)
5. QAM more energy efficient than PAM
6. Phase error rotates constellation
7. Frequency error spins constellation

### Common Exam Questions

1. Draw 16-QAM constellation (4×4 grid)
2. Calculate εavg for M-QAM and M-PAM, compare
3. What happens with phase error θ ≠ φ?
4. What happens with frequency error f₀ ≠ fc?
5. Why is QAM more bandwidth efficient than PAM?
6. Compare 16-QAM vs 16-PAM energy
7. Draw QAM demodulator block diagram
8. Explain I and Q components
9. What is QPSK? How related to QAM?
10. Calculate minimum distance for given constellation

---

## PRACTICAL INSIGHTS

### Why QAM Used Everywhere

- **WiFi**: 64-QAM, 256-QAM, even 1024-QAM
- **LTE/5G**: Up to 256-QAM
- **Cable modems**: 256-QAM standard
- **Satellite**: QPSK/8PSK (constant amplitude for power amp)

### Design Trade-offs

- **Higher M**: More bits/symbol but smaller dmin (more errors)
- **Energy vs Bandwidth**: Can trade one for other
- **Complexity vs Performance**: More complex receiver for better efficiency

### Practical Issues

1. **I/Q imbalance**: Real hardware not perfectly orthogonal
2. **Carrier recovery**: Critical and difficult
3. **Timing recovery**: Must sample at correct times
4. **Channel estimation**: Needed for practical systems

---

**Pro Tip**: Master the ENERGY CALCULATIONS - they're critical for understanding QAM advantage over PAM. Also, understand the GEOMETRIC interpretation of phase/frequency errors (rotation and spinning) - makes the math intuitive!
