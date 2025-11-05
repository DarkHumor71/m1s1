# EENG537 Lecture 2 - Quick Study Guide

## A Telecommunication System

---

## 1. ELECTROMAGNETIC TRANSMISSION FUNDAMENTALS

### Antenna Requirements

**Basic Rule**: Antenna needs to be **longer than 1/10 of wavelength** for efficient transmission

**Wavelength-Frequency Relationship**:

```
λ = c/f
where c ≈ 3×10⁸ m/s (speed of light)
```

**Example**:

- f = 10 kHz
- λ = (3×10⁸)/(10×10³) = 30,000 m = 30 km
- Required antenna: > 0.1λ = 3 km (impractical!)

**Solution**: Use HIGHER carrier frequencies

- Higher f → Smaller λ → Shorter antenna
- Makes transmission practical

---

## 2. ANALOG UP CONVERSION (MODULATION)

### Baseband Signal

**Definition**: Signal that includes frequencies very near zero (lowpass signal)

- Denoted: w(t) in time domain
- Fourier Transform: W(f) in frequency domain

**Properties of W(f)**:

- Usually complex
- **Magnitude** |W(f)| is **symmetric**
- **Phase** ∠W(f) is **anti-symmetric**

### Frequency Up-Conversion (Modulation)

**Process**: Multiply baseband signal by sinusoid

```
s(t) = w(t) × cos(2πf₀t)
```

Where:

- f₀ = carrier frequency
- Usually f₀ >> f* (f* = signal bandwidth)

### Frequency Domain Result

```
S(f) = ½W(f - f₀) + ½W(f + f₀)
```

**Effect**:

- Baseband signal W(f) centered at 0
- Creates two copies at ±f₀
- Each scaled by ½

**S(f)** is called **passband signal**

---

## 3. BANDWIDTH DEFINITIONS

### Four Common Definitions

1. **Absolute Bandwidth**

   - Total frequency range

2. **3-dB Bandwidth (Half-Power)**

   - Range where power drops to ½ (-3 dB)
   - Most common definition

3. **Null-to-Null (Zero-Crossing)**

   - Between first nulls on either side

4. **Power Bandwidth**
   - Range containing 99% of signal power

---

## 4. ANALOG DOWN CONVERSION (DEMODULATION)

### Ideal Case

**Assumption**: r(t) = s(t) (signal arrives unimpaired)

### Down-Conversion Process

**Step 1: Mixing**

```
d(t) = r(t) × cos(2πf₀t)
     = w(t)cos(2πf₀t) × cos(2πf₀t)
     = ½w(t) + ½w(t)cos(4πf₀t)
```

**Frequency Domain**:

```
D(f) = ½W(f) + ½[½W(f-2f₀) + ½W(f+2f₀)]
```

**Step 2: Lowpass Filtering**

- Apply ideal LPF
- Extracts ½W(f) portion near DC
- Output: ½w(t) (recovered baseband signal)

**Block Diagram**: r(t) → [×cos(2πf₀t)] → [LPF] → ½w(t)

---

## 5. FREQUENCY DIVISION MULTIPLEXING (FDM)

### Concept

**Definition**: Combining different signals using different carrier frequencies

### FDM Transmitted Signal

```
s(t) = w₁(t)cos(2πf₁t) + w₂(t)cos(2πf₂t) + w₃(t)cos(2πf₃t)
```

**Requirements**:

- f\* << f₁ < f₂ < f₃
- Carriers separated enough: no overlap
- Each signal occupies band: [fᵢ - f*, fᵢ + f*]

### Spectrum Structure

```
Signal 1: [f₁-f*, f₁+f*]
Signal 2: [f₂-f*, f₂+f*]
Signal 3: [f₃-f*, f₃+f*]
(no overlap between adjacent bands)
```

### Recovering Specific Signal (e.g., w₃(t))

**Method 1: Direct Down-Conversion**

1. Mix received signal with cos(2πf₃t)
2. Apply LPF with cutoff f\*
3. Recovers ½w₃(t)

**Method 2: Intermediate Frequency (IF)**

1. Mix with cos(2πfᵢt) where fᵢ < f₁
2. Apply BPF: passes [f₁-fᵢ-f*, f₃-fᵢ+f*]
3. Further digital processing after ADC

---

## 6. ANALOG CORE

### Complete AM Communication System

**Transmitter**:

- Baseband w(t) → [×cos(2πf₀t)] → s(t) → Channel

**Receiver**:

- Channel → r(t) → [×cos(2πf₀t)] → [LPF] → ½w(t)

### Non-Ideal Effects (Real World Issues)

#### 1. Filter Imperfections

- Bandwidth deviations from spec
- Shoulders don't drop fast enough
- May pass adjacent signals

#### 2. Oscillator Inaccuracies

- Frequency not exact
- Modulation/demodulation errors
- Always some jitter

#### 3. Phase Uncertainty

- Carrier phase unknown at receiver
- Depends on travel time between TX and RX

#### 4. Fundamental Limitations

- Perfect filters impossible (even in principle)
- No perfectly regular oscillator

---

## 7. CARRIER SYNCHRONIZATION PROBLEM

### Frequency and Phase Errors

**Down-conversion with errors**:

```
d(t) = w(t)cos(2πf₀t) × cos(2π(f₀+α)t + β)
     = ½w(t)[cos(2παt+β) + cos(2π(2f₀+α)t+β)]
```

**After LPF** (assuming α << f₀):

```
output = ½w(t)cos(2παt + β)
```

### Critical Case

**When α = 0 and β ≈ π/2**: Output vanishes!

**Solution**: Receiver must include **carrier synchronization**

- Adjust frequency (α → 0)
- Adjust phase (β → 0)

---

## 8. SAMPLING & ADC

### Purpose

Transform analog signal → digital for processing

### ADC Process

1. **Sampling**: Measure amplitude at regular intervals
2. **Quantization**: Store measurements at quantization levels

### Nyquist Criterion

**Requirement**: Sample at **≥ 2× highest frequency** to avoid aliasing

### Three Sampling Locations

#### Case 1: At Receiver Input (Direct)

- **Rate**: Proportional to carrier frequency (≥ 2(f₃+f\*))
- **Pros**: Fully software-based receiver (desirable)
- **Cons**: Not feasible for high-freq carriers (ADC too expensive)

#### Case 2: After Down-Conversion (Baseband)

- **Rate**: ≥ 2f\* (lowest rate)
- **Pros**: Affordable ADC
- **Cons**: Requires accurate (expensive) analog down-conversion

#### Case 3: Intermediate Frequency (IF) - BEST!

- **Two-step process**:
  1. Analog down-conversion to IF
  2. Sample at IF, then digital down-conversion to baseband
- **Pros**:
  - Minimal precision analog (inexpensive)
  - Reasonable sampling rate (inexpensive)
- **Balance**: Best compromise

---

## 9. DIGITAL COMMUNICATION ADVANTAGES/DISADVANTAGES

### ADVANTAGES

1. ✓ Digital circuits relatively inexpensive
2. ✓ Data encryption enhances privacy
3. ✓ Greater dynamic range
4. ✓ Merge voice/video/data sources
5. ✓ Easy signal compression
6. ✓ Noise doesn't accumulate over repeaters
7. ✓ Low error rates possible (even with noise)
8. ✓ Error correction via coding

### DISADVANTAGES

1. ✗ Bandwidth required
2. ✗ Synchronization needed

**Key Trade-off**: Digital provides robustness and flexibility at cost of bandwidth and complexity

---

## 10. PULSE SHAPING

### Why Important?

Convert digital data stream → analog signal for transmission

### Signal Structure

```
w(t) = Σ s[k]·p(t - kT)
```

Where p(t) is the pulse shape

### Rectangular Pulse Analysis

**Time Domain**: p(t) = rect(t/T)

**Fourier Transform**:

```
P(f) = T·sinc(fT)
```

### Problem with Rectangular Pulse

**Characteristics**:

- ✓ Strictly time-limited
- ✗ Broad frequency content (side lobes)
- ✗ Requires wide band allocation in FDM
- **Conclusion**: Need alternatives!

### Pulse Shape Comparison

| Pulse Type        | Time Domain      | Frequency Domain            | Issues                                         |
| ----------------- | ---------------- | --------------------------- | ---------------------------------------------- |
| **Rectangular**   | Time-limited     | Broad (sinc)                | Wide bandwidth                                 |
| **Sinc**          | Not time-limited | Strictly bandlimited (rect) | Many terms needed, high time sync requirements |
| **Raised Cosine** | Compromise       | Compromise                  | Good balance                                   |

### Zero-ISI Condition

**Requirement**: Pulse must be **zero at all integer multiples of T** (except at origin)

- Rectangular satisfies this
- Sinc satisfies this: sinc(t/T) = 0 at t = ±T, ±2T, ±3T, ...
- Raised cosine satisfies this

**Why Important**: Sampling at proper times gives clean symbol values (no inter-symbol interference)

---

## 11. TIME DIVISION MULTIPLEXING (TDM)

### Concept

**Alternative to FDM**: Multiple messages use SAME carrier frequency but at ALTERNATING time instants

### Comparison: FDM vs TDM

| Feature        | FDM                | TDM             |
| -------------- | ------------------ | --------------- |
| **Separation** | Frequency          | Time            |
| **Carrier**    | Different for each | Same for all    |
| **Timing**     | Simultaneous       | Alternating     |
| **Bandwidth**  | Each has own band  | Share same band |

---

## KEY FORMULAS & RELATIONSHIPS

### 1. Wavelength-Frequency

```
λ = c/f  (c ≈ 3×10⁸ m/s)
```

### 2. Up-Conversion (Modulation)

```
Time: s(t) = w(t)·cos(2πf₀t)
Freq: S(f) = ½W(f-f₀) + ½W(f+f₀)
```

### 3. Down-Conversion (Demodulation)

```
Mixer output: d(t) = r(t)·cos(2πf₀t)
After LPF: ½w(t)
```

### 4. Rectangular Pulse Transform

```
p(t) = rect(t/T) ↔ P(f) = T·sinc(fT)
```

### 5. Sinc Pulse Transform

```
p(t) = sinc(t/T) ↔ P(f) = T·rect(fT)
```

### 6. Nyquist Sampling

```
fs ≥ 2fmax
```

---

## IMPORTANT CONCEPTS

### Modulation Purpose

- Shift signal to higher frequencies
- Enable practical antenna sizes
- Enable frequency multiplexing

### Symmetry Properties

- **Magnitude** |W(f)| = |W(-f)| (even symmetry)
- **Phase** ∠W(f) = -∠W(-f) (odd symmetry)

### FDM Requirements

- Adequate frequency separation
- Guard bands to prevent overlap
- Precise carrier frequencies

### Carrier Synchronization

- **Critical** for proper demodulation
- Must match frequency AND phase
- Phase error of 90° → complete signal loss!

### Pulse Shape Trade-offs

- **Time-limited** ↔ **Frequency-limited** (can't have both!)
- Rectangular: simple but wide bandwidth
- Sinc: ideal bandwidth but impractical (infinite time)
- Raised cosine: practical compromise

---

## EXAM ESSENTIALS

### Definitions to Know

1. **Baseband signal**: Frequencies near zero
2. **Passband signal**: Modulated signal around carrier
3. **FDM**: Frequency Division Multiplexing
4. **TDM**: Time Division Multiplexing
5. **Carrier synchronization**: Matching freq and phase
6. **Zero-ISI**: No inter-symbol interference

### Key Concepts

1. Why higher frequencies needed (antenna size)
2. Up-conversion creates copies at ±f₀
3. Down-conversion needs matching carrier
4. Phase error can kill signal completely
5. Rectangular pulse → wide bandwidth problem
6. Sinc pulse → ideal bandwidth but impractical
7. Three sampling locations and trade-offs

### Common Exam Questions

1. Calculate wavelength for given frequency
2. Draw spectrum after modulation
3. Show down-conversion process with LPF
4. Explain FDM signal separation
5. What happens with phase error β = π/2?
6. Compare rectangular vs sinc pulse shapes
7. Which sampling location is best and why? (Case 3!)
8. Draw FDM spectrum for 3 signals
9. Calculate antenna length requirement
10. Advantages of digital vs analog communication

---

## PRACTICAL INSIGHTS

### Why Modulation?

- **Physical**: Practical antenna sizes
- **Spectrum**: Multiple users (FDM)
- **Propagation**: Better channel characteristics at higher frequencies

### Real-World Challenges

- No perfect filters
- Oscillators drift
- Phase unknown and varying
- Carrier synchronization hardest part

### Design Choices

- **Pulse shape**: Balance between time and frequency localization
- **Sampling location**: IF compromise best for most applications
- **Multiplexing**: FDM for continuous signals, TDM for bursty data

### Modern Systems

- Use intermediate frequency (IF) approach
- Digital processing after IF sampling
- Software-defined radio (SDR) trend

---

**Pro Tip**: Focus on MODULATION/DEMODULATION process (time and frequency domain), understand why carrier sync is critical (phase error kills signal!), and know pulse shape trade-offs - these concepts are fundamental to entire course!
