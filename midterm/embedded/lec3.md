# CENG507 Lecture 3 - Quick Study Guide

## Embedded System Hardware - Part 1

---

## CORE STRUCTURE: HARDWARE IN THE LOOP

**Physical Environment → Sensors → ADC → Processing → DAC → Actuators → Physical Environment**

Components:

1. **Input**: Sensors, Sample-and-Hold, ADC
2. **Processing**: ASICs, Processors, FPGAs
3. **Output**: DAC, PWM, Actuators
4. **Energy**: Generation, Storage, Management
5. **Communication**: Between components

---

## 1. INPUT - CYPHY INTERFACE

### A. SENSORS (Physical to Electrical)

| Sensor Type      | Purpose              | Key Feature                                                                            |
| ---------------- | -------------------- | -------------------------------------------------------------------------------------- |
| **Acceleration** | Measure acceleration | Small mass displaced, changes resistance. Used in IMUs (6 DOF: x,y,z + roll,pitch,yaw) |
| **Image**        | Capture images       | CCD (better quality, complex) vs CMOS (faster, cheaper, smart sensors)                 |
| **Biometric**    | Authentication       | Iris, fingerprint, face. Issues: false accepts/rejects                                 |
| **RFID**         | Identification       | Tag + antenna, key for IoT                                                             |
| **Automotive**   | Car systems          | Rain, tire pressure, collision sensors                                                 |

**IMU** = Inertial Measurement Unit (gyros + accelerometers, 6 degrees of freedom)

---

### B. SAMPLE-AND-HOLD CIRCUITS (Discretization of Time)

**Purpose**: Convert continuous time signals to discrete time

**How it works**:

- Clocked transistor (switch) + capacitor
- Switch closes → capacitor charges to input voltage
- Switch opens → voltage held until next sample
- Result: Discrete sequence h(t) from continuous e(t)

---

### C. SAMPLING THEOREM (Critical!)

**Aliasing Problem**: Different signals can have same sampled representation

**Sampling Theorem**: To avoid aliasing:

- **Sampling frequency (fs) must be > 2 × maximum signal frequency**
- Or: **Signal frequency < fs/2**

**Anti-aliasing Filters**:

- Placed BEFORE sample-and-hold circuit
- Remove high-frequency components
- Ensure successful reconstruction

---

### D. ANALOG-TO-DIGITAL CONVERTERS (ADCs)

**Purpose**: Convert continuous values to discrete values

**Trade-off**: Fast ADCs = Low precision, High precision = Slow

**Flash ADC**:

- Uses many comparators
- Each comparator: if V+ > V- → output = 1, else 0
- Fast but requires many components

**Quantization Noise**:

- q(t) = h(t) - w(t) (difference between analog and quantized)
- Higher ADC resolution = less quantization noise
- Measured by SNR (Signal-to-Noise Ratio) in decibels (dB)

---

## 2. PROCESSING UNITS (Three Types)

### 1. ASICs (Application-Specific Integrated Circuits)

**Characteristics**:

- Hardwired, custom-designed chips
- Very energy-efficient
- High design/manufacturing cost
- No flexibility after production

**Use When**:

- Large market volumes
- Ultimate energy efficiency needed
- Special voltage/temperature ranges
- Mixed analog/digital signals
- Security-driven designs

---

### 2. PROCESSORS (Programmable)

**Key Advantage**: FLEXIBILITY - behavior changed by software

**COTS**: Commercial Off-The-Shelf processors (very popular)

**Three Efficiency Aspects**:

#### A. ENERGY EFFICIENCY

**Relationship**: E = ∫P(t)dt (Energy = Power integrated over time)

**Key Insight**: Reducing execution time usually reduces energy (if power doesn't increase too much)

**Techniques**:

1. **Parallel Execution**: Improves overall energy efficiency
2. **Dynamic Power Management (DPM)**: Multiple power-saving states
3. **Dynamic Voltage and Frequency Scaling (DVFS)**: Adjust voltage/frequency pairs
   - Example: Crusoe processor (1.1-1.6V, 200-700MHz, 20ms transition)

**Important**:

- Power affects: supply size, regulators, interconnect, cooling
- Energy affects: battery life, cost, reliability (high temp = shorter lifetime)

#### B. CODE SIZE EFFICIENCY

**Why Important**: Embedded systems have limited memory (no large HDD/SSD)

**Techniques**:

1. **CISC Machines**:

   - Complex Instruction Set Computer
   - Designed for code size efficiency (vs RISC for speed)
   - Example: ColdFire (Motorola 68000 family)

2. **Compression**:
   - Store instructions in compressed form
   - Decoder between processor and memory
   - Benefits: Less silicon, less energy, faster fetching

#### C. EXECUTION TIME EFFICIENCY

**DSP (Digital Signal Processing) Processors**:

**Key Features**:

1. **Saturating Arithmetic**:

   - Standard: wrap-around on overflow/underflow
   - Saturating: return max value (overflow) or min value (underflow)
   - Makes sense for audio/video (user won't notice difference)

2. **Fixed-Point Arithmetic**:

   - 80% of DSP processors don't include floating-point hardware
   - Specified by 3-tuple
   - Reduces cost and power

3. **Real-Time Capability**:
   - Avoid features that only improve average time (not worst-case)
   - Sometimes NO caches (hard to guarantee speed-up)
   - NO virtual addressing or demand paging

---

### VLIW PROCESSORS (Very Long Instruction Word)

**Concept**: EPIC (Explicit Parallelism Instruction Set Computer)

**How it works**:

- Multiple operations in one long instruction word (instruction packet)
- Each operation controls one hardware unit
- Compiler detects parallelism (NOT processor at run-time)
- Saves silicon and energy

**Branch Problem**:

- Large delay penalty in pipelines
- Solutions:
  1. Delayed branch (execute following instructions anyway)
  2. Stall pipeline until branch target fetched

---

### MULTI-CORE PROCESSORS

**Why**: Hit the "power wall" - can't increase single processor speed anymore

**Characteristics**:

- Multiple processors on same chip (die)
- Faster communication than multiprocessor systems
- Can share resources (caches)
- Today: **Heterogeneous** (mix of high-performance + energy-efficient cores)

---

### GPUs (Graphics Processing Units)

**Evolution**: Hardwired → Programmable (to support custom algorithms)

**Approach**: Run many fine-grained threads concurrently

**Structure** (Example: Matrix multiplication):

- **Grid**: Overall set of computations
- **Thread Blocks**: Allocated to cores
- **Threads**: Contains instructions
- **Fast switching**: Hide memory latencies by switching threads

**Key Feature**: Keep many processing units busy, hide memory latencies

---

### MPSoCs (Multiprocessor Systems on Chip)

**Definition**: Heterogeneous multi-core + GPU merged together

- Example: ARM cores + Mali GPU

---

### 3. RECONFIGURABLE LOGIC (FPGAs)

**Middle Ground**: Between ASICs (fast but fixed) and software (flexible but slow)

**FPGAs (Field Programmable Gate Arrays)**:

**Characteristics**:

- Programmable "in the field" (after fabrication)
- Almost as fast as ASICs
- Function changed by configuration data
- Arrays of processing elements

**Use Cases**:

- Fast prototyping
- Low-volume applications
- Real-time systems
- High parallel processing applications

**Structure**:

- **CLBs (Configurable Logic Blocks)**: Key components
- Each CLB contains blocks with:
  - **LUT (Look-Up Table)**: RAM implementing logic functions
  - 6 address inputs, 2 outputs
  - Can implement ANY function of 6 variables (2^64 functions!)
  - Or two functions of 5 variables
- Plus: DSP hardware, RAM, I/O interfaces, clocking, ADCs, debugging

**Configuration**: Generated from high-level description (e.g., VHDL)

---

## KEY COMPARISONS

### Image Sensors: CCD vs CMOS

| Feature     | CCD                         | CMOS                               |
| ----------- | --------------------------- | ---------------------------------- |
| Quality     | Better (historically)       | Now comparable                     |
| Technology  | Optimized for optics        | Standard CMOS                      |
| Readout     | Sequential (pixel to pixel) | Random access                      |
| Integration | Separate logic needed       | Logic on same chip (smart sensors) |
| Speed       | Slower                      | Faster (better for video)          |
| Cost        | Higher                      | Lower                              |
| Interface   | Complex                     | Easy (single voltage)              |
| Use         | Scientific imaging          | Live view, video, low-cost devices |

### Processing Options Comparison

| Type          | Speed   | Flexibility | Energy | Cost      | Use Case                     |
| ------------- | ------- | ----------- | ------ | --------- | ---------------------------- |
| **ASIC**      | Fastest | None        | Best   | Very High | Large volume, max efficiency |
| **Processor** | Medium  | High        | Medium | Low       | General embedded systems     |
| **FPGA**      | Fast    | High        | Good   | Medium    | Prototyping, low volume, RT  |

---

## IMPORTANT FORMULAS

**Energy**: E = ∫P(t)dt

**Sampling Theorem**: fs > 2 × fmax (or fmax < fs/2)

**Quantization Noise**: q(t) = h(t) - w(t)

**SNR**: Signal-to-Noise Ratio (measured in decibels)

---

## KEY TERMS TO MEMORIZE

- **Cyphy-interface**: Interface between physical and cyber world
- **Aliasing**: Multiple signals with same sampled representation
- **Quantization Noise**: Error from discretizing values
- **COTS**: Commercial Off-The-Shelf
- **DPM**: Dynamic Power Management
- **DVFS**: Dynamic Voltage and Frequency Scaling
- **CISC**: Complex Instruction Set Computer (code size efficient)
- **RISC**: Reduced Instruction Set Computer (speed efficient)
- **DSP**: Digital Signal Processing
- **VLIW**: Very Long Instruction Word
- **EPIC**: Explicit Parallelism Instruction Set Computer
- **GPU**: Graphics Processing Unit
- **MPSoC**: Multiprocessor System on Chip
- **FPGA**: Field Programmable Gate Array
- **CLB**: Configurable Logic Block
- **LUT**: Look-Up Table
- **IMU**: Inertial Measurement Unit

---

## EXAM TIPS

**Likely Questions**:

1. Explain sampling theorem and why fs > 2×fmax
2. Compare CCD vs CMOS sensors
3. Three types of processing units and when to use each
4. Explain DVFS and DPM
5. What is aliasing and how to prevent it?
6. ASIC vs FPGA vs Processor trade-offs
7. Why saturating arithmetic in DSP?
8. How do FPGAs achieve configurability? (Answer: LUTs)
9. GPU approach to parallelism (thread blocks)
10. Energy vs Power - why both matter?

**Focus Areas**:

- Sampling theorem (critical!)
- ADC/DAC conversion process
- Three efficiency types (energy, code size, execution time)
- FPGA structure (CLBs, LUTs)
- Trade-offs between processing options
- Energy efficiency techniques (parallel, DPM, DVFS)

---

**Pro Tip**: Understand the TRADE-OFFS - embedded systems are all about balancing speed, energy, cost, and flexibility!
