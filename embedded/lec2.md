# CENG507 Lecture 2 - Quick Study Guide

## Specifications and Modeling

---

## CORE CONCEPT

**Model** = Simplified representation containing ONLY relevant characteristics for a task

- Minimal: no unnecessary characteristics
- Described in languages

---

## MODELS OF COMPUTATION (MoC) - TWO CLASSIFICATIONS

### 1. COMMUNICATION (How components talk)

**A. Shared Memory**

- Components access same memory
- Fast but hard to distribute
- Requires protection (semaphores, mutexes, monitors)
- Critical sections need exclusive access

**B. Message Passing - Three Types:**

- **Asynchronous (Non-blocking)**
  - Sender doesn't wait
  - Uses buffered channels
  - Risk of overflow
- **Synchronous (Blocking/Rendezvous)**
  - Atomic instantaneous actions
  - Both processes wait for each other
  - No overflow risk, may impact performance
- **Extended Rendezvous (Remote Invocation)**
  - Sender waits for acknowledgment
  - Recipient can do preliminary checking

### 2. COMPUTATION (What components do)

- **Differential Equations**: For analog circuits and physical systems
- **FSMs (Finite State Machines)**: Finite states, inputs, outputs, transitions
- **Data Flow**: Data availability triggers operations
- **Discrete Event**: Events with time stamps, global event queue
- **Von Neumann**: Sequential execution of primitive computations

---

## KEY LANGUAGES COMPARISON TABLE

| Language          | Type            | Communication | Use Case            |
| ----------------- | --------------- | ------------- | ------------------- |
| **StateCharts**   | FSM + Hierarchy | Shared Memory | Reactive systems    |
| **SDL**           | FSM             | Async Message | Distributed systems |
| **VHDL/Verilog**  | Discrete Event  | Signals       | Hardware design     |
| **SystemC/SpecC** | Discrete Event  | Channels      | HW/SW co-design     |
| **Ada/CSP**       | Von Neumann     | Sync Message  | Real-time systems   |
| **Petri Nets**    | Control Flow    | -             | Causal dependencies |

---

## FSM EXTENSIONS (Important!)

**1. Classical FSMs**

- Deterministic: only one state active
- Edges = transitions
- Edge labels = events
- Synchronous FSMs: changes only at clock transitions

**2. Timed Automata**

- FSM + real-valued variables (clocks)
- Clock constraints guard transitions
- Clocks can be reset to zero
- Example: answering machine with timing conditions

**3. StateCharts (David Harel, 1987)**

- FSM + hierarchy
- Superstates contain substates
- Implicit shared memory communication
- Limited timing specification
- Included in UML

**4. Communicating FSMs (CFSMs)**

- Multiple FSMs + communication
- Used in SDL

---

## DATA FLOW MODELS

**Kahn Process Networks (KPN)**

- Nodes = computations (programs/tasks)
- Edges = infinite FIFOs
- Shows dependencies, NOT execution order
- Communication guaranteed within finite time

**Simulink**

- MATLAB toolbox
- Popular in control engineering
- Graphical computational structures
- Model-based design approach

---

## PETRI NETS

- Model control and control dependencies (Carl Adam Petri, 1962)
- Focus on causal dependencies
- No global synchronization
- Suited for distributed systems
- Extended in UML as activity diagrams

---

## HARDWARE DESCRIPTION LANGUAGES (HDLs)

**Discrete Event-Based Model:**

- Queue of future events sorted by time
- Fetch event → perform actions → enter new events
- Time advances when no current actions

**SpecC**

- Clear separation: communication vs computation
- Behaviors + channels + interfaces
- Hierarchical networks

**SystemC**

- Based on C/C++
- Channels, ports, interfaces
- Concurrent processes with wait primitives
- Dynamic sensitivity lists

**VHDL**

- VHSIC Hardware Description Language
- DoD project, widespread use
- Processes model concurrent hardware
- Signals for communication (like wires)
- More popular in Europe

**Verilog**

- IEEE standard 1364
- Similar to VHDL
- More popular in USA
- Less flexible type system

---

## VON NEUMANN LANGUAGES

**CSP (Communicating Sequential Processes)**

- Channel-based communication
- Rendezvous-based (synchronous) message passing
- Both processes wait for each other

**Ada**

- Designed by US DoD in 1980s
- Based on PASCAL
- Named after Ada Lovelace (first programmer)
- Tasks (processes) with nested declarations
- Real-time language for military software

---

## COMBINED MODELS (MEMORIZE THESE!)

- **StateCharts** = FSMs + Shared Memory
- **SDL** = FSMs + Async Message Passing
- **Ada/CSP** = Von Neumann + Sync Message Passing

---

## EARLY DESIGN PHASE TOOLS

**Use Cases**

- Describe possible applications of System Under Design (SUD)
- Standardized in UML
- No precisely specified computation or communication model

**Sequence Charts / Message Sequence Charts**

- Show message sequences between components
- Vertical dimension = sequence (or real time in Time/Distance Diagrams)
- Horizontal dimension = different components (or real distance)
- Display partial orders and possible behaviors

**Modelica**

- For cyber-physical systems
- Graphical and textual forms
- Interconnected blocks with equations
- Bidirectional meaning (like mathematics)
- Flattening creates global equation set

---

## KEY PEOPLE & DATES TO REMEMBER

- **David Harel (1987)**: Created StateCharts
- **Carl Adam Petri (1962)**: Invented Petri Nets
- **Ada Lovelace**: First programmer (Ada language named after her)

---

## REQUIREMENTS FOR MODELING LANGUAGES

**Essential Features:**

- Component-based design
- Concurrency
- Synchronization and communication
- Timing behavior
- State-oriented behavior
- Event-handling
- Exception-oriented behavior
- Executability
- Support for large systems
- Readability, portability, flexibility
- Appropriate Model of Computation (MoC)

**Two Types of Hierarchies:**

- **Behavioral**: Objects describing system behavior (states, events, outputs)
- **Structural**: Physical composition (processors, memories, actuators, sensors)

---

## CRITICAL POINTS FOR EXAM

1. **No single MoC meets all requirements** - must use compromises/mixed models

2. **Hierarchies:**

   - Behavioral = behavior objects (states, events)
   - Structural = physical components (processors, memories)

3. **Real-time systems** need explicit timing (classical CS abstracts time away with O-notation)

4. **SpecC & SystemC** meet most listed requirements

5. **Shared Memory vs Message Passing**: trade-off between speed and distribution

6. **Component-based design**: behavior of system should be predictable from component behaviors

7. **Humans struggle with concurrent systems** - many real system problems result from incomplete understanding

---

## COMMON EXAM TOPICS

- Compare communication paradigms (shared memory vs message passing types)
- Match language to application domain
- FSM variants and when to use each
- Identify MoC from description
- Combined model examples
- When to use which model (application-dependent)
- Differences between VHDL and Verilog
- StateCharts vs SDL vs classical FSMs

---

## TASK GRAPHS

- Nodes (τ) = computations/vertices
- Edges (E) = relations between computations
- Capture causal dependencies
- Extensions may include:
  - Timing information
  - Different types of relations
  - Exclusive resource access
  - Periodic schedules
  - Hierarchical graph nodes

---

## PRO STUDY TIPS

**Focus on:**

- DIFFERENCES between models
- WHEN TO USE WHAT (application context)
- Combined models (how they merge computation + communication)
- Trade-offs (speed vs distribution, safety vs performance)
- Language-to-domain matching

**Common Question Patterns:**

- "Which MoC is best for X application?"
- "Compare communication paradigm A vs B"
- "What are the advantages/disadvantages of language X?"
- "Identify the MoC from this description"

---

**FINAL NOTE**: No perfect language exists. Real design = compromises + mixed models based on application domain!
