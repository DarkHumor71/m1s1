# CENG507 Lecture 4 - Quick Study Guide

## Embedded System Hardware - Part 2

---

## TOPICS COVERED

1. Memories
2. Communication
3. Output (DAC, PWM, Actuators)
4. Electrical Energy
5. Secure Hardware

---

## 1. MEMORIES

### KEY PROBLEM: MEMORY WALL

**The Gap**:

- Memory speed: increases 1.07× per year
- Processor performance: increases 1.5-2× per year
- Result: HUGE gap between processor and memory speeds
- Further performance limited by memory access times

**Memory Wall**: Gap that existed when clock speeds saturated (~2003), still remains with multi-cores

---

### MEMORY TYPES

#### RAM (Random Access Memory)

| Type               | Speed  | Size                     | Retention             | Cost |
| ------------------ | ------ | ------------------------ | --------------------- | ---- |
| **SRAM** (Static)  | Fast   | Large (more silicon/bit) | As long as power on   | High |
| **DRAM** (Dynamic) | Slower | Small (less silicon/bit) | Short (needs refresh) | Low  |

**Usage**: Most embedded systems have SRAM. Many also include DRAM for capacity.

---

### MEMORY HIERARCHY (Top to Bottom = Fast to Slow)

1. **Processor Registers**

   - Fastest
   - Limited capacity (few hundred words max)
   - Most expensive per bit

2. **Buffer Memories** (Between registers and main memory)

   - **Caches**: Automatic, hardware-managed
   - **Translation Look-aside Buffers (TLBs)**: For virtual memory
   - **Scratchpad Memory (SPM)**: Software-managed, very energy-efficient

3. **Main Memory (Working Memory)**

   - Implements processor memory addresses
   - Capacity: few MB to GB
   - **Volatile** (lost when power off)

4. **Non-Volatile Storage**
   - **ROM (Read-Only Memory)**: Fixed at factory (mask ROM)
   - **EEPROM**: Electrically erasable
   - **Flash Memory**: Best for embedded systems (firmware + user data)
   - **HDD/SSD**: For larger systems
   - **Cloud**: Internet-based storage

---

### CACHES vs SCRATCHPAD MEMORY

#### CACHES

- **Hardware-managed**: Automatic updates
- **Tag checking**: Hardware compares tag fields
- **Name**: From French "cacher" (to hide) - invisible to programmer
- **Energy**: N-way set associative reads N entries in parallel (energy-hungry)
- **Goal**: Good run-time efficiency initially, but also improves energy efficiency

#### SCRATCHPAD MEMORY (SPM) / Tightly Coupled Memory (TCM)

- **Software-managed**: Mapped into address space
- **No tag checking**: Simple address decoder
- **Access**: By proper memory address selection
- **Energy**: Very efficient (no parallel reads like caches)
- **Integration**: On-chip with processor (on-chip memory)

**KEY DIFFERENCE**: SPM is more energy-efficient than caches (avoids parallel tag reads)

---

### REGISTER FILES

- Small memories with high access frequency
- Can get very hot due to frequent accesses
- Power consumption is critical concern

---

## 2. COMMUNICATION

### CHANNELS vs MEDIA

- **Channels**: Abstract entities (max capacity, noise parameters)
- **Communication Media**: Physical entities
  - Wireless (RF, infrared)
  - Optical (fibers)
  - Wires

---

### COMMUNICATION REQUIREMENTS

1. **Real-time behavior**: Standard Ethernet fails this
2. **Efficiency**: Hardware connections can be expensive
3. **Appropriate bandwidth/delay**: Balance performance vs cost
4. **Event-driven support**: Not just polling (too slow for emergencies)
5. **Security/Privacy**: Encryption for confidentiality
6. **Safety/Robustness**: Reliable operation
7. **Fault tolerance**: Operational even after faults
8. **Maintainability/Diagnosability**: Repairable in reasonable time

---

### ELECTRICAL ROBUSTNESS: SIGNALING TYPES

#### SINGLE-ENDED SIGNALING

- **Method**: Signal on single wire, voltage vs common ground
- **Advantage**: One ground wire for multiple signals
- **Disadvantage**: Very susceptible to external noise
- **Use**: Digital communication within chips

#### DIFFERENTIAL SIGNALING

- **Method**: Two wires (twisted pairs), value = sign of voltage difference
- **Encoding**: V1 > V2 → '1', V1 < V2 → '0'
- **Advantages**:
  - Noise added equally to both wires → comparator removes it
  - No common ground needed
  - Higher throughput than single-ended
- **Disadvantages**:
  - Requires two wires per signal
  - Needs negative voltages (unless complementary logic)
- **Examples**: Ethernet, USB

---

### COMMUNICATION STANDARDS (By Category)

#### Sensor/Actuator Buses

- Simple devices (switches, lamps)
- Cost of wiring is critical
- Many devices

#### Field Buses (Higher data rates than sensor/actuator)

- **CAN** (Controller Area Network)
- **TTP** (Time-Triggered Protocol)
- **FlexRay**
- **LIN** (Local Interconnect Network)
- **MAP**
- **EIB** (European Installation Bus)

#### I2C (Inter-Integrated Circuit) Bus

- **Range**: Short distances (meters)
- **Wires**: Only 4 (ground, clock SCL, data SDA, voltage supply)
- **Speed**: Standard 100 kb/s (variants: 10 kb/s to 3.4 Mb/s)
- **Cost**: Low

#### Wired Multimedia

- **MOST** (Media Oriented Systems Transport): Automotive infotainment
- **IEEE 1394 (FireWire)**: Multimedia

#### Wireless Communication

- **Mobile**: HSPA (7 Mbit/s), LTE (10× HSPA), 5G (50 Mbit/s to >1 Gbit/s, low latency)
- **Bluetooth**: Short distances (phones, headsets)
- **WLAN**: IEEE 802.11 and supplements
- **ZigBee**: Low-power, personal area networks (home automation, IoT)
- **DECT** (Digital European Cordless Telecommunications): Wireless phones (worldwide except different frequencies in North America)

---

## 3. OUTPUT (Cyber to Physical Interface)

### OUTPUT DEVICES

**Displays**:

- **Organic displays**: Emit light, high density, no backlight/polarizing filters needed (vs LCDs)

**Electro-mechanical devices**: Motors, actuators

**Conversion needed**: Digital → Analog (via DAC or PWM)

---

### DIGITAL-TO-ANALOG CONVERTERS (DACs)

#### Weighted-Resistor DAC

**How it works**:

1. Binary input bits (MSB to LSB)
2. Resistor network (binary-weighted resistances)
3. Bits control switches connecting resistors
4. Current summation at common node (MSB contributes most)
5. Operational amplifier converts current to voltage
6. Output: Vout = (Total Current × Rref)

**Formula**: Current proportional to digital value → Voltage output

**Shannon-Whittaker Interpolation**: Smart interpolation computes values between sampling instances

**Zero-order hold**: DAC keeps previous value until next conversion (creates step functions)

#### Disadvantages of DACs

- Difficult to build (resistors need excellent precision)
- Need power amplifiers for motors/lamps (class A amplifiers very inefficient)
- Hard to integrate analog circuitry on digital chips
- External analog components increase cost

---

### PULSE-WIDTH MODULATION (PWM)

**Alternative to DAC**: Very popular!

**Method**:

- Digital output signal
- Duty cycle corresponds to value to be converted
- Compare counter against programmable register
- High voltage when counter > register, else ~0V
- Eliminate higher-frequency components effects

**Advantages**:

- Avoids DAC disadvantages
- Digital output (easier integration)
- Programmable clock for basic frequency

---

### ACTUATORS

**Range**: Large (move tons) to tiny (μm dimensions - microsystem technology)

**Applications**:

- Medical: Tiny actuators in human body for drug delivery (better than needles)
- Internet of Things
- General automation

---

## 4. ELECTRICAL ENERGY

### ENERGY SOURCES

#### Plugged Devices

- Connected to power grid
- Energy easily available

#### Others (Especially IoT Sensor Networks)

**Batteries**:

- Store chemical energy
- Limitation: Must be carried to location

**Energy Harvesting (Energy Scavenging)**:
Avoid battery limitations by converting other energy forms:

1. **Photovoltaics**: Light → Electrical (photovoltaic effect in semiconductors, solar panels)
2. **Piezoelectric effect**: Mechanical strain → Electrical
3. **Thermoelectric Generators (TEGs)**: Temperature gradients → Electrical (can use human body heat)
4. **Kinetic energy**: Motion → Electrical (watches, wind energy)
5. **Ambient EM radiation**: Electromagnetic → Electrical
6. Many other physical effects

**Trade-off**: Harvesting provides less energy than batteries/grid

---

### ENERGY EFFICIENCY: HARDWARE COMPARISON

**Figure of Merit**: GOP/J (Giga-Operations per Joule)

| Technology                | Efficiency          | Flexibility | Use Case                      |
| ------------------------- | ------------------- | ----------- | ----------------------------- |
| **ASIC** (Hardwired)      | Highest             | None        | Large volumes, max efficiency |
| **FPGA** (Reconfigurable) | ~10× less than ASIC | Medium      | Moderate flexibility needed   |
| **DSP Processors**        | ~10× less than FPGA | High        | Domain-specific               |
| **General Processors**    | Lowest              | Highest     | Maximum flexibility           |

**Key Trade-off**: Efficiency vs Flexibility (inverse relationship)

**Trend**: As technology advances (smaller feature sizes), operations/joule increases for ALL technologies

---

## 5. SECURE HARDWARE

### WHY IMPORTANT

- General embedded system requirement
- Critical for Internet of Things
- Needed for communication AND storage

---

### ATTACK TYPES

#### 1. SOFTWARE ATTACKS

**Methods**:

- Software Trojans
- Exploit software defects (buffer overflows common)
- **Side-channel attacks**: Exploit additional information sources
  - Execution time information (time-based)
  - Solution: Algorithms with data-independent execution times

**Requirements**:

- Security algorithms: Execution time independent of data
- Computer arithmetic: No data-dependent instruction times

#### 2. PHYSICAL ATTACKS

**Methods**:

- **Chip analysis**: De-packaging → micro-probing or optical analysis
- **Power analysis**:
  - SPA (Simple Power Analysis): May compute encryption keys
  - DPA (Differential Power Analysis): Statistical methods on current fluctuations
- **Electromagnetic radiation analysis**: Side-channel attack

---

### COUNTERMEASURES

#### 1. Software Level

- Security-aware development process
- Prevent buffer overflows and defects

#### 2. Physical Protection

- **Tamper-resistant devices**: Shielding, tampering detection sensors
- **Power-independent design**: Data patterns have little impact on power (special devices needed)

#### 3. Logical Security (Cryptography)

**Symmetric Ciphers** (Same key for encrypt/decrypt):

- DES, 3DES, AES

**Asymmetric Ciphers** (Public key encrypt, private key decrypt):

- RSA, Diffie-Hellman

**Hash Codes** (Detect modifications):

- MD5, SHA

---

### CHALLENGES FOR COUNTERMEASURES

1. **Performance gap**: Advanced encryption too slow for limited embedded systems
2. **Battery gap**: Encryption energy-intensive (problem for portables, smart cards critical)
3. **Lack of flexibility**: Many protocols needed, must update → hinders hardware accelerators
4. **Tamper resistance**: Hard to guarantee power consumption independent of crypto keys
5. **Assurance gap**: Security verification requires extra design effort
6. **Cost**: Higher security = higher cost

---

## KEY COMPARISONS

### Cache vs Scratchpad Memory

| Feature        | Cache                   | Scratchpad (SPM)          |
| -------------- | ----------------------- | ------------------------- |
| Management     | Hardware (automatic)    | Software (manual)         |
| Tag checking   | Yes (parallel reads)    | No (address decoder)      |
| Visibility     | Hidden from programmer  | Mapped to address space   |
| Energy         | Higher (N-way parallel) | Lower (no parallel reads) |
| Predictability | Less (for real-time)    | More (for real-time)      |

### Single-ended vs Differential Signaling

| Feature        | Single-ended                 | Differential                      |
| -------------- | ---------------------------- | --------------------------------- |
| Wires          | 1 per signal + common ground | 2 per signal (twisted pair)       |
| Noise immunity | Poor                         | Excellent                         |
| Throughput     | Lower                        | Higher                            |
| Voltage        | Positive only                | Needs negative (or complementary) |
| Cost           | Lower                        | Higher                            |
| Use            | Within chips                 | Ethernet, USB, external           |

### DAC vs PWM

| Feature     | DAC                              | PWM     |
| ----------- | -------------------------------- | ------- |
| Type        | Analog                           | Digital |
| Precision   | High (resistor precision needed) | Medium  |
| Integration | Difficult on digital chips       | Easy    |
| Efficiency  | Poor (power amplifiers)          | Good    |
| Cost        | High (external components)       | Low     |
| Popularity  | Lower                            | Higher  |

---

## IMPORTANT TERMS

- **Memory Wall**: Gap between processor and memory speeds
- **SRAM**: Static RAM (fast, large, no refresh)
- **DRAM**: Dynamic RAM (slow, small, needs refresh)
- **SPM/TCM**: Scratchpad/Tightly Coupled Memory (software-managed, energy-efficient)
- **Volatile**: Lost when power off (RAM)
- **Non-volatile**: Persistent storage (ROM, Flash)
- **Flash Memory**: Best for embedded systems
- **Single-ended**: One wire + ground
- **Differential**: Two wires, twisted pairs
- **I2C**: Inter-Integrated Circuit bus (4 wires, short distance)
- **CAN**: Controller Area Network (field bus)
- **ZigBee**: Low-power IoT protocol
- **DAC**: Digital-to-Analog Converter
- **PWM**: Pulse-Width Modulation
- **Energy Harvesting**: Converting other energy forms to electrical
- **TEG**: Thermoelectric Generator
- **SPA**: Simple Power Analysis
- **DPA**: Differential Power Analysis
- **Symmetric cipher**: Same key (DES, AES)
- **Asymmetric cipher**: Public/private keys (RSA)

---

## EXAM FOCUS AREAS

**Likely Questions**:

1. Explain memory wall and its impact
2. Compare SRAM vs DRAM
3. Cache vs Scratchpad - advantages/disadvantages
4. Single-ended vs Differential signaling
5. Why is differential better for noise?
6. What is PWM and why preferred over DAC?
7. Energy harvesting techniques (list 3-5)
8. Security attacks types (software vs physical)
9. Countermeasures for secure hardware
10. Trade-off: Efficiency vs Flexibility in processors
11. Communication requirements for embedded systems
12. I2C bus characteristics

**Key Concepts**:

- Memory hierarchy (registers → buffers → main → non-volatile)
- SPM is more energy-efficient than caches (WHY?)
- Differential signaling removes noise (HOW?)
- PWM avoids DAC problems (WHAT problems?)
- Energy efficiency: ASIC > FPGA > DSP > General processor
- Security challenges: 6 gaps/challenges

---

**Pro Tip**: Focus on COMPARISONS and TRADE-OFFS - that's what this lecture is all about! Know WHY one choice is better than another for specific applications.
