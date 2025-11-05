# EENG537 Lecture 1 - Quick Study Guide

## Digital Radio / Digital Communication System

---

## CORE OBJECTIVE

**Digital Communication System**: Transmission of information in DIGITAL FORM from source to destination(s)

**Basic Structure**: Transmitter → Channel → Receiver

---

## 1. DIGITAL DATA

### Sources of Digital Data

**1. Inherently Digital**:

- Computer terminal output
- Already in binary form (0s and 1s)

**2. Analog-to-Digital Conversion**:

- Analog signal → Digital signal
- Process: **Sampling + Quantization**

### Types of Digital Content

- Digital Images
- Digital Video
- Digital Audio
- All represented as stream of binary data (0 & 1)

---

## 2. TRANSMISSION CHANNELS

**Analog (continuous) channels** between transmitter and receiver:

| Channel Type       | Example                                     |
| ------------------ | ------------------------------------------- |
| **Wireline**       | Telephone line, twisted-pair, coaxial cable |
| **Optical**        | Optical fiber                               |
| **Radio/Wireless** | Electromagnetic (antenna radiated)          |
| **Acoustic**       | Underwater acoustic                         |
| **Storage**        | Hard disk drive (HDD)                       |

---

## 3. TRANSMISSION SCHEMES (Evolution)

### Scheme 1: Simple Binary (PROBLEM!)

- Send pulse (+V) for '1'
- Send nothing for '0'

**Problem**: Can't distinguish string of zeros from no transmission!

### Scheme 2: Bipolar (BETTER)

- Send pulse (+V) for '1'
- Send pulse (-V) for '0'

**Solution**: Always transmitting something

### Scheme 3: Multi-level (4-PAM)

**Use 4 amplitude levels** to send 2 bits per symbol:

| Bits | Amplitude |
| ---- | --------- |
| 11   | +3        |
| 10   | +1        |
| 01   | -1        |
| 00   | -3        |

**Advantage**: More bits per symbol (higher data rate)

---

## 4. TEXT MESSAGE CODING

**Coder**: Converts original message (text) into symbol alphabet

Process: Message → Symbols → Transmission

---

## 5. TRANSMITTER & TRANSMITTED SIGNAL

### Baseband Transmitter Components

- **Input**: Symbol sequence s[k]
- **Process**: Scale and shift pulse shape
- **Output**: Transmitted signal

### Key Parameters

- **Symbol period (T)**: Time between successive symbols
- **Pulse shape p(t)**: Shape of transmitted pulse (often rectangular)
- **Start time (τ)**: When first pulse begins

### Transmitted Signal Structure

Each pulse:

- Same shape p(t)
- Offset in time (by multiples of T)
- Scaled by symbol value s[k]

---

## 6. PULSE AMPLITUDE MODULATION (PAM)

### Mathematical Description

**Complete transmitted signal**:

```
w(t) = Σ s[k] · p(t - τ - kT)
       k
```

Where:

- s[k] = symbol values
- p(t) = pulse shape
- τ = start time
- T = symbol period
- k = symbol index

### Pulse Sequence

- 1st pulse: s[0]p(t - τ) at time τ
- 2nd pulse: s[1]p(t - τ - T) at time τ + T
- 3rd pulse: s[2]p(t - τ - 2T) at time τ + 2T
- And so on...

### Terminology

- **PAM**: Pulse Amplitude Modulation (sending info by scaling pulse amplitude)
- **4-PAM**: Four symbol amplitudes (as in Scheme 3 above)

---

## 7. RECEIVED SIGNAL & RECEIVER

### Ideal Case

Received signal = Transmitted signal BUT:

- **Attenuated** (scaled by gain g)
- **Delayed** (by propagation delay δ)

### Baseband Receiver Components

1. **Sampler**:

   - Samples at time η
   - Typical choice: η = τ + δ + T/2 (middle of pulse)
   - Produces r[k] = samples

2. **Quantizer**:

   - Reproduces source symbols from samples
   - Must either:
     - Know g to scale by 1/g, OR
     - Separate ±g from ±3g to distinguish symbols

3. **Decoder**:
   - Reconstructs original message from symbols

---

## 8. SYNCHRONIZATION ISSUES (CRITICAL!)

### Problem 1: TIMING SYNCHRONIZATION

**Unknown quantities**:

- τ (transmit start time) - unknown
- δ (propagation delay) - unknown
- Receiver doesn't know when to sample!

**Additional issue**:

- T_trans ≠ T_rec (different oscillators)
- Receiver must adjust T_rec to align with T_trans

### Problem 2: CLOCK JITTER

- No clock perfectly regular
- May need to adjust η to keep sampling at pulse center

### Problem 3: FRAME SYNCHRONIZATION

- Symbols grouped into frames
- Need to find where frame starts for proper decoding

**Bottom line**: Timing synchronization is NEEDED!

---

## 9. DATA RATE & BANDWIDTH

### For 4-PAM Transmission

**Symbol duration**: T seconds

**Symbol rate**: 1/T symbols/second

**Bit rate**: 2/T bits/second (since 2 bits per symbol)

### Frequency Spectrum (Rectangular Pulse)

**Fourier Transform of p(t)**:

- NOT bandlimited (extends to infinity)
- Define B = 1/T
- Most energy within 5B Hz

### Key Relationship

**Decreasing T** (faster transmission):

- ↑ Data rate
- ↑ Occupied bandwidth

**Trade-off**: Bandwidth ∝ 1/T

**Rule**: Bandwidth occupied by pulse is **inversely related** to pulse width

- Narrower pulses (faster transmission) → More bandwidth needed

---

## 10. SPECTRUM SHARING

### Goal

Multiple users communicate simultaneously through same medium in same geographical region

### Method: FREQUENCY MULTIPLEXING

- Avoid interference by using different frequencies
- Users allocated different center frequencies
- Translate baseband spectrum to different RF frequencies
- No overlap between users

**Requirement**: Bandlimited signals translated by different amounts

---

## 11. RF COMMUNICATION SYSTEM

### Structure

Baseband signal → **Upconversion** → RF channel → **Downconversion** → Baseband signal

### Frequency Translation

- **Transmitter**: Baseband → RF (upconversion)
- **Receiver**: RF → Baseband (downconversion)
- Enables spectrum sharing

---

## 12. PRACTICAL OBSTACLES

### 1. CARRIER SYNCHRONIZATION

- Precise frequency translation required in receiver
- Receiver must match transmitter's carrier frequency

### 2. TIMING SYNCHRONIZATION

- Precise timing required (as discussed earlier)
- Must know when to sample

### 3. MULTI-USER INTERFERENCE

- Users not strictly bandlimited
- Signals can overlap in frequency
- Causes interference

### 4. NOISE CONTAMINATION

**Types**:

- In-band noise (within signal bandwidth)
- Out-of-band noise (outside bandwidth)
- Narrowband noise (specific frequencies)
- Broadband noise (wide frequency range)

### 5. CHANNEL DISTORTION

**Types**:

- **Fading**: Signal strength varies over time
- **Multipath**: Signal arrives via multiple paths
  - Direct path
  - Reflected paths
  - Creates interference with itself

**Multipath effects**:

- Time dispersion
- Frequency-selective fading
- Inter-symbol interference (ISI)

---

## KEY FORMULAS & RELATIONSHIPS

1. **Symbol Rate**: Rs = 1/T symbols/second

2. **Bit Rate** (for M-ary PAM): Rb = (log₂M)/T bits/second

   - For 4-PAM: Rb = 2/T bits/second

3. **Bandwidth**: B ≈ 1/T Hz (main lobe)

4. **Data Rate vs Bandwidth**: Rb ∝ B (directly proportional)

5. **Transmitted Signal**: w(t) = Σ s[k]·p(t - τ - kT)

6. **Received Signal** (ideal): r(t) = g·w(t - δ)

   - g = attenuation factor
   - δ = propagation delay

7. **Sampling Time**: η = τ + δ + T/2 (ideal)

---

## IMPORTANT CONCEPTS

### PAM Advantages

- Simple implementation
- Easy to scale (2-PAM, 4-PAM, 8-PAM, etc.)
- More symbols = more bits/symbol = higher data rate

### PAM Disadvantages

- Requires good SNR for higher order (more symbols = closer spacing)
- Susceptible to noise
- Requires synchronization

### Synchronization Types

1. **Timing Sync**: When to sample
2. **Carrier Sync**: Correct frequency translation
3. **Frame Sync**: Where frames start

### Bandwidth-Data Rate Trade-off

- Higher data rate → More bandwidth needed
- Fundamental limitation of digital communications
- Can't have both low bandwidth AND high data rate (for given modulation)

---

## EXAM ESSENTIALS

### Definitions to Memorize

1. **Digital Communication System**: Transmission of information in digital form
2. **PAM**: Pulse Amplitude Modulation - scaling pulse by symbol amplitude
3. **Symbol Period (T)**: Time between successive symbols
4. **Pulse Shape p(t)**: Shape of transmitted pulse
5. **Synchronization**: Aligning receiver timing with transmitter

### Key Concepts

1. Why bipolar better than unipolar (can distinguish from no transmission)
2. 4-PAM sends 2 bits per symbol (log₂4 = 2)
3. Three unknowns: τ, δ, T_trans
4. Bandwidth inversely related to pulse width (B ∝ 1/T)
5. Multipath causes self-interference

### Common Exam Questions

1. Calculate bit rate for M-PAM with symbol period T
2. Why is synchronization needed? (List 3 types)
3. Compare transmission schemes (simple, bipolar, 4-PAM)
4. How does decreasing T affect bandwidth?
5. What are practical obstacles in digital communications?
6. Explain spectrum sharing and frequency multiplexing
7. Draw block diagram of transmitter/receiver
8. What is multipath and why is it a problem?

---

## PRACTICAL INSIGHTS

### Why Digital?

- Robust to noise (regeneration possible)
- Easy encryption
- Easy compression
- Easy error correction
- Integration with computers

### Channel Types in Practice

- **Wireline**: High reliability, limited mobility
- **Wireless**: Mobility, but more obstacles (fading, multipath)
- **Optical**: Huge bandwidth, limited by distance
- **Acoustic**: Underwater, very slow propagation

### Synchronization Challenges

- Most difficult aspect of receiver design
- Requires feedback loops
- Can't be perfect (always some error)
- Trade-off: accuracy vs complexity

---

**Pro Tip**: Focus on the PROGRESSION of transmission schemes (why each is better than previous), understand bandwidth-data rate relationship, and know ALL synchronization issues - these are exam favorites!
