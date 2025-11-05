# EENG537 Lecture 3 - Quick Study Guide

## Bits to Symbols to Signals

---

## SIGNAL FLOW (Complete Chain)

**Transmitter**: Message → Bits → Symbols → Signals (analog)

**Receiver**: Signals (analog) → Symbols → Bits → Message

---

## 1. BITS TO SYMBOLS

### Source Data Forms

- Pressure wave (air)
- Electron flow (wire)
- Digitized image/sound
- Text

**Process**: Analog → ADC → Binary digits (0,1) → Symbols

### M-Level Signaling

| Signaling Type | Bits per Symbol (b) | Alphabet Size (M) | Symbol Values    |
| -------------- | ------------------- | ----------------- | ---------------- |
| **2-level**    | 1                   | 2                 | {+1, -1}         |
| **4-level**    | 2                   | 4                 | {-3, -1, +1, +3} |
| **8-level**    | 3                   | 8                 | {±1, ±3, ±5, ±7} |

**General Formula**: M = 2^b or b = log₂(M)

---

## 2. GRAY CODING (Important!)

### Definition

Mapping where **adjacent symbols differ by only ONE bit**

### Why Important?

- Most likely errors (due to noise) → adjacent amplitude selection
- With Gray coding: Single bit error (not multiple)
- Improves error resilience

### Gray Coding Examples

**2-PAM**:

```
-1 → 0
+1 → 1
```

**4-PAM**:

```
-3 → 00
-1 → 01
+1 → 11  (note: differs from -1 by 1 bit)
+3 → 10
```

**Note**: Gray coding is NOT unique for given M

---

## 3. KEY PARAMETERS & FORMULAS

### Time Parameters

- **Symbol period (T)**: Time between successive symbols
- **Symbol rate**: Rs = 1/T symbols/second
- **Bit interval**: Tb = T/b = T/log₂(M) seconds

### Data Rates

```
Bit rate: Rb = b/T = log₂(M)/T = b·Rs bits/second
```

**Example (4-PAM)**:

- b = 2 bits/symbol
- T = 0.1 seconds
- Rs = 1/0.1 = 10 symbols/sec
- Rb = 2/0.1 = 20 bits/sec

---

## 4. SYMBOLS TO SIGNALS (PAM)

### Pulse Amplitude Modulation (PAM)

**Mathematical Expression**:

```
x(t) = Σ s[k]·p(t - kT)
     k≥0
```

Where:

- s[k] = kth symbol value
- p(t) = pulse shape
- T = symbol period

**Alternative Form** (Convolution):

```
x(t) = [Σ s[k]·δ(t - kT)] ⊛ p(t)
```

### Implementation

**Pulse Shaping Filter**: Pass scaled Dirac pulses through filter with impulse response p(t)

---

## 5. PULSE SHAPES (Critical Comparison!)

### Three Main Pulse Shapes

#### 1. RECTANGULAR PULSE

**Time Domain**:

- Duration: T seconds
- Compact (time-limited)

**Frequency Domain**:

- P(f) = T·sinc(fT)
- Broad spectrum (wide side lobes)
- NOT bandlimited

**Characteristics**:

- ✓ Simple
- ✓ Time-limited
- ✗ Bandwidth inefficient
- ✗ Limits simultaneous users in FDM

#### 2. SINC PULSE

**Time Domain**:

- p(t) = sinc(t/T)
- NOT time-limited (infinite duration)
- Zero at t = ±T, ±2T, ±3T, ... (zero ISI!)

**Frequency Domain**:

- P(f) = T·rect(fT)
- Strictly bandlimited
- Ideal bandwidth

**Characteristics**:

- ✓ Strictly bandlimited
- ✓ Zero ISI
- ✗ Not time-limited
- ✗ Many terms needed for approximation
- ✗ High timing synchronization requirements

#### 3. RAISED COSINE / HAMMING

**Compromise** between rectangular and sinc

**Characteristics**:

- Neither strictly time nor frequency limited
- Practical implementation
- Good balance

### Pulse Shape Design Goals (Can't Optimize All!)

1. **No ISI**: Value at time k doesn't interfere with other sample times
2. **Bandwidth efficient**: Make good use of spectrum
3. **Noise resilient**: Robust against noise

**Trade-off**: Must carefully balance these requirements

---

## 6. CORRELATION (Pattern Matching)

### Definition

**Cross-Correlation**: Quantifies similarity of two signals

**Discrete Time Formula**:

```
Rw,v[j] = (1/T)Σ w[k]·v[k+j]
```

**For finite records** (Matlab xcorr):

```
Rw,v[j] = Σ w[k]·v[k+j] = Σ w[k-j]·v[k]
```

### Applications in Communications

1. **Symbol Level**: Find appropriate sampling times
2. **Frame Level**: Find start of message/frame

### How It Works

- Shift one sequence relative to other
- Multiply point-by-point and sum
- **Small sum**: Not similar
- **Large sum**: Very similar

### Simple Example

```
u = [+1, -1, -1, +1]
w = [+1, +1, -1, +1]
v = [+1, +1, +1, -1]

u·w = (+1)(+1) + (-1)(+1) + (-1)(-1) + (+1)(+1) = 2
v·w = (+1)(+1) + (+1)(+1) + (+1)(-1) + (-1)(+1) = 0

→ u is most similar to w
```

---

## 7. CORRELATION vs CONVOLUTION

### Key Differences

| Feature          | Correlation             | Convolution        |
| ---------------- | ----------------------- | ------------------ |
| **Operation**    | R[j] = Σw[k]v[k+j]      | y[n] = Σx[k]h[n-k] |
| **Relationship** | R[j] = x[j] ⊛ fliplr(h) | Direct convolution |
| **Purpose**      | Measure similarity      | System response    |
| **Meaning**      | Pattern matching        | Filter output      |
| **Padding**      | Extra zeros added       | No extra padding   |

### Matlab Functions

```matlab
ycorr = xcorr(h, fliplr(x))  % Correlation
yconv = conv(x, h)            % Convolution
```

**Relationship**: Convolution ≈ Correlation of time-reversed signal (with padding difference)

---

## 8. RECEIVE FILTERING (Signals → Symbols)

### Process

1. **Correlate** received signal with pulse shape
2. **Scale** appropriately
3. **Downsample** to symbol rate (choose 1 of M samples)
4. **Quantize** to nearest alphabet element

### Mathematical Steps

**Step 1: Correlation**

```
y = xcorr(received_signal, pulse_shape)
```

**Step 2: Downsample**

```
z = y[N*M : M : 2*N*M-1] / (pow(ps)*M)
```

Choose every Mth sample, normalize

**Step 3: Quantization**

```
mprime = quantalph(z, alphabet)
```

Quantize to nearest element in {-3, -1, +1, +3}

### Matlab Implementation (recfilt.m)

```matlab
ps = hamming(M);              % Pulse shape
y = xcorr(x, ps);             % Correlate
z = y(N*M:M:2*N*M-1)/(pow(ps)*M);  % Downsample & normalize
mprime = quantalph(z, [-3,-1,1,3])'; % Quantize
```

---

## 9. FRAME SYNCHRONIZATION

### Purpose

- Data separated into **frames** (chunks)
- Must locate frame boundary (start)
- Use **marker sequence** (predefined symbols)

### Good Marker Properties

**Example Comparison**:

- **Marker A**: [1, 1, 1, 1, 1, 1, 1] (all ones)
- **Marker B**: [1, 1, 1, -1, -1, 1, -1] (varied pattern)

### Correlation Results

**Marker A**: xcorr = [-1, -1, 1, 1, 1, 3, 5, 7, 7, 7, 5, 5]

- Multiple peaks
- Gradual rise
- **Less robust**

**Marker B**: xcorr = [1, 1, 3, -1, -5, -1, -1, 1, 7, -1, 1, -3]

- Sharp single peak at position 9
- Clear maximum
- **More robust** (especially with noise)

### Why Marker B Better?

- **Sharp peak**: Easier to detect
- **Lower side lobes**: Less confusion
- **Noise resilient**: Peak still distinguishable

### Design Principle

Good marker has:

- High autocorrelation at zero lag
- Low autocorrelation at other lags
- Low cross-correlation with random data

---

## 10. PULSE SHAPE BANDWIDTH COMPARISON

### Experimental Results (T = 0.1 sec)

| Pulse Shape     | Main Lobe Bandwidth | Side Lobes  | Notes                                 |
| --------------- | ------------------- | ----------- | ------------------------------------- |
| **Hamming**     | ~2 Hz               | Small       | Power mainly in main lobe             |
| **Rectangular** | Narrower            | Large       | Wastes bandwidth on side lobes        |
| **Sinc**        | Narrowest (~1.5 Hz) | Almost none | Best bandwidth, 25% less than Hamming |

**Conclusion**: Sinc is best compromise with minimal power outside main band

---

## 11. EFFECTS OF MISMATCH

### Pulse Shape Mismatch (TX vs RX)

**Problem**: TX uses one pulse shape, RX uses different one

**Results** (Example):

- TX: Hamming, RX: sin pulse → Percentage errors
- TX: Hamming, RX: cos pulse → Percentage errors

**Lesson**: Pulse shapes must match for proper demodulation!

### Noise Effects

**Simulation**: Add noise to received signal

```matlab
x = x + σ·randn(size(x))
```

**As σ increases**: Error rate increases

**Critical threshold**: Beyond certain noise level, correlation fails to recover symbols

---

## KEY MATLAB FUNCTIONS

### Communication Functions

```matlab
letters2pam(str)           % Encode ASCII to 4-PAM
pam2letters(seq)           % Decode 4-PAM to ASCII
quantalph(x, alphabet)     % Quantize to nearest symbol
```

### Signal Processing

```matlab
xcorr(x, y)               % Cross-correlation
conv(x, h)                % Convolution
filter(b, a, x)           % Filter implementation
hamming(N)                % Hamming window
```

### Utility

```matlab
fft(x)                    % Fast Fourier Transform
fftshift(X)               % Shift zero-frequency to center
fliplr(x)                 % Flip left-right (time reversal)
```

---

## IMPORTANT CONCEPTS

### Zero ISI Condition

Pulse p(t) must satisfy:

- p(0) = 1
- p(kT) = 0 for all k ≠ 0

**Ensures**: Sampling at correct times gives clean symbols (no interference)

### Oversampling

- Factor M: Sample M times per symbol period
- Required for proper pulse shaping
- Downsampling after correlation recovers symbol rate

### Quantization

- Map continuous values to discrete alphabet
- Nearest neighbor method
- Errors occur when noise pushes value closer to wrong symbol

---

## EXAM ESSENTIALS

### Formulas to Memorize

1. **M = 2^b** (alphabet size from bits per symbol)
2. **Rs = 1/T** (symbol rate)
3. **Rb = b·Rs = log₂(M)/T** (bit rate)
4. **x(t) = Σ s[k]·p(t-kT)** (PAM signal)
5. **Rw,v[j] = Σ w[k]·v[k+j]** (correlation)

### Key Concepts

1. Gray coding reduces bit errors (adjacent symbols differ by 1 bit)
2. Pulse shape trade-off: time-limited ↔ frequency-limited
3. Rectangular: simple but bandwidth inefficient
4. Sinc: ideal bandwidth but impractical
5. Correlation used for: timing sync and frame sync
6. Good marker: sharp autocorrelation peak, low side lobes
7. Receive filtering: correlate → downsample → quantize

### Common Exam Questions

1. Calculate bit rate for M-PAM with given T
2. Design Gray code for M-PAM
3. Compare pulse shapes (bandwidth, ISI properties)
4. Perform correlation calculation by hand
5. Why is marker B better than marker A?
6. Draw correlation output for given sequences
7. Explain receive filtering process (3 steps)
8. What happens with pulse shape mismatch?
9. Calculate number of symbol errors
10. Design good frame marker (properties)

---

## PRACTICAL INSIGHTS

### System Design Choices

**Pulse Shape Selection**:

- Rectangular: Simple hardware, but bandwidth inefficient
- Raised cosine: Industry standard (good compromise)
- Root raised cosine: Split between TX and RX (matched filter)

**Marker Design**:

- Longer marker: More robust but wastes bandwidth
- Varied pattern: Better autocorrelation properties
- Consider using pseudo-random sequences (PN codes)

**Error Sources**:

1. Noise (AWGN)
2. Pulse shape mismatch
3. Timing errors
4. Quantization errors
5. Channel distortion

### Matlab Simulation Tips

- Use oversampling (M=10 typical)
- Normalize correlation output
- Account for correlation delay
- Test with different noise levels
- Verify with known test sequences

---

**Pro Tip**: Master CORRELATION - it's the heart of receiver design! Understand how it's used for both symbol recovery and frame synchronization. Also, know pulse shape trade-offs cold - exam loves comparing rectangular vs sinc vs raised cosine!
