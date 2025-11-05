# CENG507 Lecture 1 - Quick Study Guide

## Introduction to Embedded System Design

---

## KEY TERMS & DEFINITIONS

### 1. EMBEDDED SYSTEMS

**Definition**: "Information processing systems embedded into enclosing products"

**Examples**: Cars, trains, planes, telecommunication equipment, fabrication equipment

### 2. CYBER-PHYSICAL SYSTEMS (CPS)

**Definition**: "Integrations of computation and physical processes"

**Formula**: CPS = ES + (Dynamic) Physical Environment

**Key point**: Emphasizes link to physical processes and environment

### 3. INTERNET OF THINGS (IoT)

**Definition**: Pervasive presence of devices (sensors, actuators, mobile phones) with unique addressing, able to interact and cooperate

**Key features**:

- Links sensors (data to Internet)
- Links actuators (control from Internet)
- Expected to connect TRILLIONS of devices

### RELATIONSHIP

- **CPS/IoT** = Most future IT applications
- Choice between terms is preference-based
- Both require fundamental embedded system design techniques

---

## HISTORICAL EVOLUTION

| Era         | Technology                       | Focus                                 |
| ----------- | -------------------------------- | ------------------------------------- |
| Until 1980s | Mainframe computers, tape drives | Large-scale computing                 |
| Later       | Personal computers (PCs)         | Office applications, some control     |
| Modern      | Embedded systems                 | Integration with physical environment |

**Related Terms**:

- **Ubiquitous computing**: Information anytime, anywhere (long-term goal)
- **Pervasive computing**: Practical aspects, existing technology
- **Ambient intelligence**: Communication in smart homes/buildings

---

## OPPORTUNITIES (Application Areas)

### Major Sectors

1. **Automotive Electronics**

   - Airbag control, engine control, navigation, ABS, ESP
   - Trend: Autonomous driving
   - Benefits: Comfort, accident avoidance, environmental impact

2. **Avionics**

   - Flight control, anticollision, pilot info, autopilots
   - Dependability is CRITICAL
   - Autonomous flying emerging

3. **Railways**

   - Safety features, advanced signaling
   - High speed, short intervals
   - Autonomous trains (limited contexts)

4. **Maritime Engineering**

   - Navigation, safety, operation optimization

5. **New Mobility Concepts**

   - E-bikes, e-scooters (CPS example)
   - Collective taxis, taxi-calling services

6. **Mechanical Engineering**

   - Machinery, fabrication equipment
   - CPS/IoT for production optimization

7. **Robotics**

   - Traditional ES/CPS area
   - Important mechanical aspects

8. **Power Engineering & Smart Grid**

   - Decentralized energy production
   - ICT for stability

9. **Civil Engineering**

   - Structural health monitoring
   - Advanced warnings (avalanches, dam collapses)

10. **Disaster Recovery**

    - Flexible communication infrastructure
    - Life-saving, relief coordination

11. **Smart Buildings**

    - Comfort, energy reduction, safety/security
    - Integration: air-conditioning, lighting, access control, etc.

12. **Agricultural Engineering**

    - Animal traceability, disease detection

13. **Health Sector & Medical Engineering**

    - New sensors, data analysis (ML)
    - Disease detection, risk assessment

14. **Scientific Experiments**

    - Physics experiments requiring IT observation

15. **Public Safety**

    - Authentication (fingerprint, face recognition)
    - Pandemic response

16. **Military Applications**

    - Radar signal analysis (historical)

17. **Telecommunication**

    - Mobile phones (fast-growing market)
    - RF design, DSP, low-power design

18. **Consumer Electronics**
    - Video/audio equipment
    - High-definition TV, multimedia phones, game consoles

**Conclusion**: Almost ALL engineering disciplines affected

---

## CHALLENGES

### 1. DEPENDABILITY (Critical!)

**Definition**: System provides intended service with high probability, causes no harm

**Components of Dependability**:

#### A. Security

- **Confidentiality**: No unauthorized access to information
- **Integrity**: Information accuracy/completeness
- **Availability**: Information accessible when needed

#### B. Safety

- **Definition**: Absence of unacceptable risk of physical injury or damage
- Critical for systems affecting human lives

#### C. Reliability

- **Definition**: Probability system won't fail within certain time
- Must be high

#### D. Repairability (Reparability)

- **Definition**: Probability failing system can be repaired within certain time
- Must be high

#### E. Availability

- **Formula**: High reliability + High repairability + No security hazards = High availability

---

### 2. REAL-TIME CONSTRAINTS

**Hard Real-Time Constraint**: Missing deadline could result in CATASTROPHE

**Soft Real-Time Constraint**: All other time constraints (less critical)

**Importance**: Many CPS must meet real-time constraints

---

### 3. HYBRID SYSTEMS

**Definition**: Include both analog and digital parts

**Analog**:

- Continuous signal values
- Continuous time
- Real numbers (uncountable set)

**Digital**:

- Discrete signal values
- Discrete time
- Finite representable values

**Challenge**: Physical quantities can only be APPROXIMATED in digital computers

---

### 4. RESOURCE EFFICIENCY (Critical!)

Must be resource-aware. Key metrics:

#### 1. ENERGY

- Electronic ICT uses electrical energy
- Power AND energy (integral of power over time) are deciding factors
- Critical for portable/embedded systems

#### 2. RUN-TIME

- Exploit hardware architecture maximally
- Avoid wasted processor cycles
- Optimize execution times (algorithms → hardware)

#### 3. CODE SIZE

- Code stored on system itself
- Tight storage capacity constraints
- Critical for SoCs (System on Chip)
- Example: Implanted medical devices (size + communication constraints)

#### 4. WEIGHT

- All portable systems must be lightweight
- Important buying factor

#### 5. COST

- High-volume mass markets (consumer electronics)
- Competitiveness crucial
- Minimum resources for required functionality

**KEY INSIGHT**: Software design CANNOT be done independently of hardware!

- Integrated hardware/software approach needed
- Cooperation between electrical engineering and CS required

---

## COMMON CHARACTERISTICS

### 1. Sensors & Actuators

- **Sensors**: Detect physical environment
- **Actuators**: Convert numbers into physical effects
- Connected to Internet (for IoT)

### 2. Reactive Systems

**Definition**: "Continual interaction with environment, executes at pace determined by environment"

**Model**:

- In a state, waiting for input
- For each input: compute, generate output, new state
- **Best model**: Automata (NOT mathematical functions)

### 3. Under-representation

- Not enough in teaching/public discussions
- Complex → requires comprehensive equipment
- But: Visible physical impact makes teaching appealing

### 4. Dedicated Purpose

- Dedicated to specific application
- Example: Car control software won't run games/spreadsheets
- Always runs same software

### 5. Unique User Interface

- NOT keyboards, mice, large monitors
- Instead: Push buttons, steering wheels, pedals, etc.

---

## DESIGN FLOWS

### OVERALL PROCESS

**Start**: Ideas in people's heads (with application domain knowledge)
→ **Captured in**: Design specification
→ **Reuse**: Standard hardware and system software components
→ **Stored in**: Design repository

### KEY COMPONENTS

#### 1. DESIGN REPOSITORY

**Purpose**: Track design models

**Features**:

- Version management (CVS, SVN, git)
- Design management interface
- Track tool applicability and sequences
- Comfortable GUI

**Extensions**:

- Can become IDE (Integrated Development Environment) / Design Framework
- Tracks dependencies between tools and design info
- Enables iterative design decisions

#### 2. APPLICATION MAPPING

**Process**: Map applications to execution platforms

**Includes**:

- Mapping operations to concurrent tasks
- Hardware/Software partitioning
- Compilation
- Scheduling

#### 3. EVALUATION & VALIDATION

**Evaluation objectives**:

- Performance
- Dependability
- Energy consumption
- Thermal behavior

**Validation**: Check descriptions against each other

- No design step guaranteed correct
- Each decision should be evaluated AND validated

#### 4. OPTIMIZATION

**Why important**: Efficiency is critical for embedded systems

**Types**:

- High-level transformations (loop transformations)
- Energy-oriented optimizations

#### 5. TEST

- Test generation
- Testability evaluation
- Can be included in iterations OR done after design completion
- Update repository after each step

---

## DESIGN PARADIGMS

### 1. SYNTHESIS (Automatic Generation)

**Definition**: "Process of generating system description in terms of lower-level components from high-level description of expected behavior"

**Approach**: "Describe-and-Synthesize"

- Describe properties
- Let tool do the rest
- Avoids manual design steps

### 2. TRADITIONAL (Manual)

**Approach**: "Specify-Explore-Refine" or "Design-and-Simulate"

- Manual design + simulation
- Simulation for catching errors
- More simulation than synthesis approach

---

## EXAM ESSENTIALS

### Key Definitions (Memorize!)

1. Embedded System
2. Cyber-Physical System (CPS = ES + Dynamic Physical Environment)
3. Internet of Things (IoT)
4. Dependability (5 components)
5. Hard vs Soft real-time constraints
6. Reactive system
7. Synthesis

### Critical Concepts

- **Dependability** = Security + Safety + Reliability + Repairability → Availability
- **Hard real-time**: Missing deadline = catastrophe
- **Hybrid systems**: Analog + Digital (physical quantities only approximated)
- **Resource efficiency**: 5 metrics (Energy, Run-time, Code size, Weight, Cost)
- **Reactive systems**: Best modeled as automata (not functions)
- **Hardware/Software integration**: Cannot design separately!

### Design Flow Components

1. Design Repository (with version control)
2. Application Mapping (tasks, HW/SW partitioning, compilation, scheduling)
3. Evaluation (performance, dependability, energy, thermal)
4. Validation (check correctness)
5. Optimization (efficiency critical)
6. Test (optional in iterations, mandatory overall)

### Application Domains

- Know at least 5-7 major application areas
- Understand why dependability critical for some (avionics, railways)
- Know examples of CPS/IoT in each domain

---

## LIKELY EXAM QUESTIONS

1. Define: Embedded System, CPS, IoT - explain relationships
2. What is dependability? List and explain all components
3. Hard vs Soft real-time constraints - give examples
4. Why must embedded systems be resource-aware? List 5 efficiency metrics
5. What are reactive systems? Why are automata better models than functions?
6. Why can't software design be independent of hardware in embedded systems?
7. Explain design repository role and features
8. What is synthesis? Compare with traditional design approach
9. List 5 application areas for embedded/CPS/IoT systems
10. What makes hybrid systems challenging for digital computers?

---

## COURSE INFO

**Topics to Cover**:

- RTOS, RISC-V
- Design Flows (Evaluation, Validation, Mapping, Optimization, Test)
- Hardware & Software
- Resource Access Protocols
- FreeRTOS
- RISC-V Assembly & Applications

**Assessment**:

- Attendance: 10%
- Project: 20%
- Midterm: 30%
- Final: 40%

---

**Pro Tip**: This lecture is FOUNDATION for everything else. Know the definitions cold, understand why efficiency and dependability matter, and grasp the hardware/software integration requirement!
