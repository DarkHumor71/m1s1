# DSP Chapter 1 - Quick Study Guide

## Introduction to Digital Signal Processing

---

## GRADING SCHEME

- Participation: 10%
- Midterm: 35%
- Final: 40%
- Project: 15%

---

## 1. FUNDAMENTAL DEFINITIONS

### Signal

**Physical quantity** (function of space/time) carrying information about a phenomenon

**Examples**: Audio, video, speech, image, sonar, radar, temperature, ECG, EEG

### System

Mathematical model or physical device that:

- **Input**: Signal (excitation)
- **Process**: Performs operations
- **Output**: Signal (response)

**Signal Processing**: Act of processing signal using a system

### Energy vs Power Signals

- **Energy signals**: Limited energy and duration (real-life signals)
- **Power signals**: Infinite energy (practically impossible to generate)

---

## 2. BASIC DSP SYSTEM

**Complete Chain**:

```
Analog Signal → [ADC] → Digital Signal → [DSP] → Digital Signal → [DAC] → Analog Signal
```

**Components**:

1. **ADC**: Analog-to-Digital Converter
2. **DSP**: Digital Signal Processor
3. **DAC**: Digital-to-Analog Converter

---

## 3. ADVANTAGES: DIGITAL vs ANALOG

| Advantage       | Description                         |
| --------------- | ----------------------------------- |
| **Flexibility** | Reconfigurable vs hardware redesign |
| **Accuracy**    | Better control of precision         |
| **Storage**     | Magnetic media (tape/disk)          |
| **Portability** | Easy to transport                   |
| **Algorithms**  | Sophisticated processing possible   |
| **Cost**        | DSP chips cheaper                   |

---

## 4. SIGNAL CLASSIFICATION

### By Dimensions

**M-dimensional signal**: Value is function of M independent variables

### By Time

- **Continuous-time**: x(t) defined for every t
- **Discrete-time**: x[n] defined at equally spaced time values

### By Values

- **Continuous-valued**: Can take any value in range
- **Discrete-valued (Digital)**: Takes values from finite set

### By Predictability

- **Deterministic**: Can be modeled exactly by formula
- **Random**: Values subject to variability

---

## 5. CONTINUOUS-TIME SINUSOIDAL SIGNALS

### General Form

```
x(t) = A cos(Ωt + φ) = A cos(2πFt + φ)
```

**Parameters**:

- **A**: Amplitude
- **Ω = 2πF**: Angular frequency (radians/second)
- **F**: Frequency (Hz)
- **φ**: Phase angle (radians)
- **T = 1/F = 2π/Ω**: Period

**Note**: Use both positive and negative frequencies for mathematical convenience

---

## 6. DISCRETE-TIME SINUSOIDAL SIGNALS

### General Form

```
x[n] = A cos(ωn + φ) = A cos(2πfn + φ)
```

**Parameters**:

- **n**: Integer variable (sample index)
- **A**: Amplitude
- **ω = 2πf**: Angular frequency (radians/sample)
- **f**: Frequency (cycles/sample)
- **φ**: Phase angle (radians)

---

## 7. DISCRETE-TIME SIGNAL PROPERTIES (CRITICAL!)

### Property 1: PERIODICITY

**Discrete-time sinusoid is periodic ONLY if frequency is RATIONAL number**

**Rational number**: Can be expressed as fraction f = k/N (k, N integers)

**Example**:

- f = 1/12 → Periodic ✓
- f = π/6 → Not periodic ✗ (π is irrational)

### Property 2: ALIASING

**Sinusoids with frequencies separated by integer multiple of 2π are IDENTICAL**

```
cos(ωn) = cos((ω + 2πk)n) for any integer k
```

**Example**:

```
cos(π/3·n) = cos((π/3 + 2π)n) = cos(7π/3·n)
```

### Property 3: HIGHEST OSCILLATION RATE

**Maximum oscillation** occurs at **ω = ±π**

**Fundamental frequency range**: -π ≤ ω ≤ π OR -0.5 ≤ f ≤ 0.5

---

## 8. ANALOG-TO-DIGITAL CONVERSION (ADC)

### Three-Step Process

#### Step 1: SAMPLING

**Continuous-time x_a(t) → Discrete-time x[n]**

**Parameters**:

- **T**: Sampling period (seconds)
- **F_s = 1/T**: Sampling rate/frequency (samples/sec or Hz)

**Process**: Take samples every T seconds

#### Step 2: QUANTIZATION

**Discrete-time x[n] → Quantized x_q[n]**

**Process**: Convert continuous values to finite discrete levels

**Quantization Error/Noise**:

```
e_q[n] = x_q[n] - x[n]
```

#### Step 3: CODING

**Quantized x_q[n] → Digital Signal (binary)**

**Process**: Represent quantized values as binary codes

---

## 9. SAMPLING THEOREM (NYQUIST-SHANNON)

### Critical Theorem

**x_a(t) can be reconstructed from x[n] WITHOUT distortion or aliasing IF:**

```
f_s > F_N (Nyquist rate)
```

Where **F_N = 2f_max**

### Key Terms

- **f_max**: Maximum frequency in analog signal
- **F_N = 2f_max**: Nyquist rate (minimum sampling rate)
- **F_s**: Actual sampling rate
- **Folding frequency**: f_s/2 (maximum reconstructible frequency)

### Rule

```
F_s ≥ 2f_max  (to avoid aliasing)
```

---

## 10. PRACTICAL SAMPLING RATES

| Signal Type          | f_max   | f_s      | Application             |
| -------------------- | ------- | -------- | ----------------------- |
| **Speech**           | 3.4 kHz | 8 kHz    | Wireless mic, telephone |
| **Teleconferencing** | 8 kHz   | 16 kHz   | VoIP                    |
| **Audio**            | 20 kHz  | 44.1 kHz | Audio CDs               |
| **Television**       | 5 MHz   | >10 MHz  | TV signals              |

**Anti-aliasing filter**: Ensures f_max doesn't exceed predetermined value

---

## 11. ALIASING PHENOMENON

### What is Aliasing?

When **F > f_s/2**, high frequencies appear as LOW frequencies!

**Example**:

- Analog: x_a(t) = cos(2π·100t) (F = 100 Hz)
- Sample at f_s = 75 Hz (violates Nyquist!)
- Appears as: cos(2π·25t) (F = 25 Hz)

### Why It Happens

Frequencies F and (f_s - F) produce SAME samples!

**General rule**: Frequency F appears as:

- F (if F ≤ f_s/2)
- f_s - F (if F > f_s/2) ← **Aliasing!**

---

## 12. FREQUENCY RELATIONSHIPS (SUPER IMPORTANT!)

### Analog vs Digital Frequency

**Analog frequency** (Hz): F
**Digital frequency** (cycles/sample): f

**Relationship**:

```
f = F/F_s
ω = 2πf = 2πF/F_s = ΩT_s
```

Where:

- Ω = 2πF (analog angular freq, rad/s)
- ω = 2πf (digital angular freq, rad/sample)
- T_s = 1/F_s (sampling period)

### Quick Conversions

```
F → f: f = F/F_s
f → F: F = f·F_s
Ω → ω: ω = ΩT_s = Ω/F_s
ω → Ω: Ω = ω·F_s
```

---

## 13. WORKED EXAMPLES

### Example 1: Basic Sampling

**Given**: x_a(t) = 3cos(100πt)

**a) Minimum sampling rate?**

```
Ω = 100π → F = 50 Hz
F_N = 2F = 100 Hz (Nyquist rate)
```

**b) Sample at f_s = 200 Hz?**

```
T_s = 1/200
x[n] = 3cos(100πn/200) = 3cos(πn/2)
```

**c) Sample at f_s = 75 Hz? (Violates Nyquist!)**

```
T_s = 1/75
x[n] = 3cos(100πn/75) = 3cos(4πn/3) = 3cos(2πn/3)
```

**d) Aliased frequency?**

```
ω = 2π/3 → f = 1/3
F = f·F_s = (1/3)·75 = 25 Hz
```

### Example 2: Multiple Frequencies

**Given**: x_a(t) = 3cos(50πt) + 10sin(300πt) - cos(100πt)

**Nyquist rate?**

```
F₁ = 25 Hz, F₂ = 150 Hz, F₃ = 50 Hz
f_max = 150 Hz
F_s ≥ 2·150 = 300 Hz
```

**Sampled at F_s = 300 Hz:**

```
x[n] = 3cos(πn/6) + 10sin(πn) - cos(πn/3)
     = 3cos(πn/6) - cos(πn/3)  (since sin(πn) = 0)
```

### Example 3: Undersampling

**Given**: x_a(t) = 2sin(200πt), F = 100 Hz

**Sample at F_s = 200 Hz:**

```
x[n] = 2sin(200πn/200) = 2sin(πn) = 0 for all n!
```

**Signal completely lost!** (Sampling at exactly Nyquist rate)

---

## 14. ALIASING EXAMPLE (DETAILED)

**Given**: x_a(t) = 3cos(2000πt) + 5sin(6000πt) + 10cos(12000πt)

**a) Nyquist rate?**

```
F₁ = 1000 Hz, F₂ = 3000 Hz, F₃ = 6000 Hz
f_max = 6000 Hz
F_N = 12000 Hz
```

**b) Sample at F_s = 5000 Hz (VIOLATES Nyquist!)**

```
x[n] = 3cos(2πn/5) + 5sin(6πn/5) + 10cos(12πn/5)
```

**Simplify using aliasing**:

```
12πn/5 = 12πn/5 - 2π·n = 2πn/5  (aliased!)
6πn/5 = 6πn/5 - 2π·n = -4πn/5  (aliased!)

x[n] = 3cos(2πn/5) + 5sin(-4πn/5) + 10cos(2πn/5)
     = 13cos(2πn/5) - 5sin(4πn/5)
```

**c) Reconstructed analog signal:**

```
f₁ = 1/5 → F₁ = f₁·F_s = 1000 Hz
f₂ = 2/5 → F₂ = f₂·F_s = 2000 Hz

y_a(t) = 13cos(2000πt) - 5sin(4000πt)
```

**Different from original!** (Aliasing distortion)

---

## 15. DIGITAL-TO-ANALOG CONVERSION (DAC)

### Interpolation Methods

**Purpose**: "Connect the dots" in digital signal

**Methods** (increasing quality):

1. **Zero-order hold**: Staircase (maintain value until next sample)
2. **Linear interpolation**: Straight lines between samples
3. **Quadratic interpolation**: Quadratic curves through 3 samples
4. **Optimal (sinc)**: Complicated (infinite sum), best quality

**Low-pass filter requirement**:

```
F_folding = F_s/2 < f_cutoff < F_s - F_max
```

---

## 16. QUANTIZATION

### Process

Convert continuous-valued signal to **finite set of discrete levels**

### Quantization Error (Noise)

```
e_q[n] = x_q[n] - x[n]
```

### Number of Levels

**B-bit quantizer**: 2^B levels

**Examples**:

- 8-bit: 256 levels
- 16-bit: 65,536 levels
- 24-bit: 16,777,216 levels

### Effects

- More bits → Less quantization error
- More bits → Higher storage/bandwidth requirements
- Trade-off: Quality vs Resources

---

## KEY FORMULAS SUMMARY

### Continuous-Time

```
x(t) = A cos(Ωt + φ) = A cos(2πFt + φ)
T = 1/F = 2π/Ω
```

### Discrete-Time

```
x[n] = A cos(ωn + φ) = A cos(2πfn + φ)
```

### Sampling

```
F_s = 1/T_s
F_N = 2f_max (Nyquist rate)
Requirement: F_s ≥ F_N
```

### Frequency Conversion

```
f = F/F_s
F = f·F_s
ω = 2πf = 2πF/F_s = ΩT_s
```

### Aliasing

```
cos(ωn) = cos((ω ± 2πk)n)
Folding frequency = F_s/2
```

---

## IMPORTANT CONCEPTS

### Nyquist Theorem Implications

1. Must sample at **F_s > 2f_max**
2. Maximum reconstructible frequency = **F_s/2**
3. Frequencies above F_s/2 → **Aliasing**
4. Use **anti-aliasing filter** before sampling

### Discrete-Time Uniqueness

1. Periodic only if f is **rational**
2. Frequencies differing by **2πk** are **identical**
3. Maximum oscillation at **ω = ±π**
4. Fundamental range: **-π to π**

### Practical Considerations

1. Always oversample: Use F_s > 2f_max (not equal!)
2. Anti-aliasing filter essential
3. Quantization introduces noise
4. More bits → Better quality but more resources

---

## EXAM ESSENTIALS

### Formulas to Memorize

1. **F_N = 2f_max** (Nyquist rate)
2. **f = F/F_s** (frequency conversion)
3. **ω = 2πf** (angular frequency)
4. **x[n] = A cos(ωn + φ)** (discrete sinusoid)
5. **Folding frequency = F_s/2**

### Key Concepts

1. Sampling theorem and Nyquist rate
2. Aliasing (what, why, how to avoid)
3. Three ADC steps (sampling, quantization, coding)
4. Frequency relationships (F, f, Ω, ω)
5. Discrete-time sinusoid properties (3 properties!)

### Common Exam Questions

1. Find minimum sampling rate (Nyquist)
2. Sample signal and write x[n]
3. Identify aliasing (F > F_s/2)
4. Find aliased frequency
5. Convert between analog and digital frequencies
6. Determine if discrete signal is periodic
7. Reconstruct analog signal from samples
8. Calculate quantization levels (2^B)

### Problem-Solving Steps

**Type 1: Find Nyquist rate**

1. Identify all frequencies in signal
2. Find f_max
3. F_N = 2f_max

**Type 2: Sample signal**

1. Write F_s = 1/T_s
2. Replace t with nT_s in x_a(t)
3. Simplify using trig identities

**Type 3: Check aliasing**

1. Compare each F with F_s/2
2. If F > F_s/2 → aliasing occurs
3. Aliased frequency = |F_s - F| or use ω ± 2πk

**Type 4: Frequency conversion**

1. Analog → Digital: f = F/F_s, ω = 2πf
2. Digital → Analog: F = f·F_s, Ω = ω·F_s

---

## COMMON MISTAKES TO AVOID

1. **F_s = 2f_max exactly**: Use F_s > 2f_max (strictly greater!)
2. **Forgetting multiple frequencies**: Check ALL frequency components
3. **Wrong frequency conversion**: Remember f = F/F_s (NOT F_s/F)
4. **Ignoring aliasing**: Always check if F > F_s/2
5. **Periodicity test**: Must be rational (k/N), not just any decimal
6. **Angular vs cyclic frequency**: ω = 2πf (don't forget 2π!)
7. **Units confusion**: F in Hz, f in cycles/sample, ω in rad/sample

---

**Pro Tip**: Master the FREQUENCY CONVERSIONS (f = F/F_s) and NYQUIST THEOREM (F_s > 2f_max) - they appear in EVERY problem! Also, practice identifying ALIASING - it's a favorite exam topic!
