# ENGG515 - Quick Study Guide

## Graph Theory

---

## 1. BASIC TERMINOLOGY

### Graph Definition

**Graph G = (V, E)**:

- V = Set of vertices (nodes/points)
- E = Set of edges (arcs/lines connecting vertices)

### Edge Types

- **Directed**: Has arrowhead (one direction)
- **Undirected**: No direction
- **Parallel edges**: Same end vertices
- **Loop**: Edge from vertex to itself (v, v)

### Graph Types

- **Simple graph**: No parallel edges or loops
- **Empty graph**: No edges (E is empty)
- **Null graph**: No vertices (V and E empty)
- **Trivial graph**: Only one vertex

---

## 2. VERTEX & EDGE PROPERTIES

### Degree of Vertex d(v)

**Number of edges with v as endpoint**

- Count loops TWICE
- Parallel edges count separately

**Special vertices**:

- **Pendant vertex**: d(v) = 1
- **Isolated vertex**: d(v) = 0

### Graph Degree

- **δ(G)**: Minimum degree among all vertices
- **Δ(G)**: Maximum degree among all vertices

### Adjacency

- **Adjacent edges**: Share common end vertex
- **Adjacent vertices**: Connected by edge

---

## 3. COMPLETE GRAPH

**Definition**: Simple graph with EVERY possible edge between all vertices

**Notation**: Kₙ (n vertices)

**Number of edges**: C(n,2) = n(n-1)/2

**Examples**:

- K₃: Triangle (3 edges)
- K₄: 6 edges
- K₅: 10 edges

---

## 4. WALKS, TRAILS, PATHS

### Walk

**Finite sequence**: v₀, e₁, v₁, e₂, ..., eₖ, vₖ

- Can repeat vertices and edges
- **Length**: k (number of edges)
- **Open**: v₀ ≠ vₖ
- **Closed**: v₀ = vₖ

### Trail

**Walk where each EDGE traversed AT MOST ONCE**

- Can repeat vertices
- Cannot repeat edges

### Path

**Trail where each VERTEX visited AT MOST ONCE**

- Exception: Initial and terminal vertices (for circuit)
- No repeated vertices (except possibly start/end)

### Circuit

**Closed path** (starts and ends at same vertex)

---

## 5. SUBGRAPH & COMPONENT

### Subgraph

G₁ = (V₁, E₁) is subgraph of G = (V, E) if:

- V₁ ⊆ V
- Every edge in E₁ is also in E

**Induced subgraph**: <E₁> includes all end vertices of edges in E₁

### Component

**Connected subgraph** (or isolated vertex) where:

- Cannot be extended by adding edges from G
- Different components have NO common vertices

---

## 6. MATRICES

### Incidence Matrix A (n×m)

- n rows (vertices) × m columns (edges)
- aᵢⱼ = 1 if edge j incident on vertex i, else 0

**Properties**:

- Each column has exactly TWO 1's
- Row sum = degree of vertex
- All-zero row = isolated vertex

### Adjacency Matrix A (n×n)

- n rows × n columns (square)
- aᵢⱼ = number of edges from vertex i to j
- **Simple graph**: Binary (0 or 1)
- **Undirected**: Symmetric about diagonal

---

## 7. GRAPH OPERATIONS

### Complement G'

- Same vertices as G
- Edges in G' are exactly those NOT in G
- (G')' = G

### Difference G - G₁

- Same vertices
- Edges in G but NOT in G₁

### Union G₁ ∪ G₂

- V = V₁ ∪ V₂
- E = E₁ ∪ E₂

### Intersection G₁ ∩ G₂

- V = V₁ ∩ V₂
- E = E₁ ∩ E₂

### Ring Sum G₁ ⊕ G₂

- Symmetric difference: (E₁ - E₂) ∪ (E₂ - E₁)

### Removal

- **G - v**: Remove vertex v and all incident edges
- **G - e**: Remove edge e only

---

## 8. CUTS

### Cut Vertex (Articulation Vertex)

**Removing v increases number of components**

- G is **separable** if cut vertex exists
- G is **non-separable** if no cut vertex

**Condition**: v is cut vertex IF ∃ vertices u, w where:

- v ≠ u, v ≠ w, u ≠ w
- v is on EVERY path from u to w

### Cut Set

**Edge set F ⊆ E** where:

- G - F is NOT connected
- G - H is connected for any H ⊂ F (F is minimal)
- G - F has exactly TWO components

### Cut (Partition)

**Pair <V₁, V₂>** where:

- V = V₁ ∪ V₂
- V₁ ∩ V₂ = ∅
- V₁ ≠ ∅, V₂ ≠ ∅
- Cut = {edges with one end in V₁, other in V₂}

---

## 9. BIPARTITE GRAPHS

### Definition

**Graph with cut <V₁, V₂> where E = <V₁, V₂>**

- ALL edges connect V₁ to V₂
- NO edges within V₁ or within V₂

### Complete Bipartite Kₙ,ₘ

- n vertices in V₁, m vertices in V₂
- EVERY vertex in V₁ connected to EVERY vertex in V₂
- Total edges = n × m

### Graph Coloring Test

**2-colorable ↔ Bipartite**

**Algorithm**:

1. Choose vertex s, color red
2. Color neighbors blue
3. Color neighbors of blue vertices red
4. Continue until all colored
5. Valid coloring? → Bipartite ✓
6. Conflict? → Not bipartite ✗

---

## 10. TREES & FORESTS

### Tree

**Connected graph with NO circuits**

### Forest

**Graph with NO circuits** (can be disconnected)

- Each component is a tree

### Spanning Tree

**Tree that includes ALL vertices** of connected graph

**Properties**:

- n vertices → n-1 edges
- Unique path between any two vertices
- Adding any edge creates circuit

---

## 11. MINIMUM SPANNING TREE (MST)

### Definition

**Spanning tree with MINIMUM total edge weight**

### Applications

- Telecommunication networks
- Transportation networks (minimize cost)
- Electrical power transmission
- Computer system wiring
- Pipeline networks

### Prim's Algorithm

**Process**:

1. S = ∅, A = ∅ (S = vertices, A = edges)
2. Select starting vertex r, add to S
3. Find lightest edge with one end in S, other in V-S
4. Add edge to A, add new vertex to S
5. If V-S empty, STOP; else goto 3

**Result**: MST in set A

---

## 12. MST FORMULATION (Excel Solver)

### Decision Variables

```
xᵢⱼ = 1 if edge (i,j) in tree T
xᵢⱼ = 0 otherwise
```

### Objective Function

```
Minimize: Z = Σ xᵢⱼ · Cᵢⱼ
```

### Constraints

1. **Exactly N-1 edges**: Σxᵢⱼ = N-1
2. **No cycles**: For any subset S of K nodes, at most K-1 edges
3. **Connectivity**: At least 1 edge connected to each node

**Cut Formulation**: Iterative process adding constraints to eliminate cycles

---

## 13. DIRECTED GRAPHS (DIGRAPHS)

### Adjacency Matrix (Directed)

- **Leaving arc** (i→j): aᵢⱼ = 1
- **Entering arc** (j→i): aᵢⱼ = -1
- **No arc**: aᵢⱼ = 0

### Acyclic Directed Graph

**No directed circuits**

---

## 14. SHORTEST PATH (Dijkstra)

### Problem

Find **lightest path** from vertex u to vertex v

### Dijkstra's Algorithm

**Initialization**:

```
N' = {u}
For all v adjacent to u: D(v) = c(u,v)
For all other v: D(v) = ∞
```

**Loop**:

1. Find w not in N' with minimum D(w)
2. Add w to N'
3. Update D(v) for all v adjacent to w and not in N':
   ```
   D(v) = min(D(v), D(w) + c(w,v))
   ```
4. Repeat until all nodes in N'

**Two Tables**:

- **Distance table**: Path weights
- **Predecessor table**: Reconstruct path

---

## 15. SHORTEST PATH (Excel Formulation)

### Decision Variables

```
xᵢⱼ = 1 if arc (i,j) in path
xᵢⱼ = 0 otherwise
```

### Objective

```
Minimize: Z = Σ xᵢⱼ · dᵢⱼ
```

### Constraints

For each node:

- **Source**: One leaving arc
- **Destination**: One entering arc
- **Other nodes**: #leaving arcs = #entering arcs

**Formula**: Σ(leaving) - Σ(entering) = {1 for source, -1 for sink, 0 for others}

---

## 16. FLOW NETWORKS

### Definition

**Directed graph** with:

- One source s
- One sink t
- Edge capacities c(u,v)

### Flow f(u,v)

Real-valued function satisfying:

**1. Capacity constraint**:

```
f(u,v) ≤ c(u,v) for all u,v
```

**2. Flow conservation** (except s and t):

```
Σ f(i,v) = Σ f(v,j) for all v ≠ s,t
```

(Flow in = Flow out)

---

## 17. MAXIMUM FLOW

### Residual Network

From G(V,E), create residual:

- Each edge (u,v) → two directed edges:
  - (u,v): weight = capacity - flow (residual capacity)
  - (v,u): weight = flow (can push back)

### Augmenting Path P

**Path from s to t in residual network**

**Residual capacity**:

```
d(P) = min{r(i,j) : (i,j) ∈ P}
```

**Augmentation**:

- Add d(P) to each edge along P in flow network
- Update residual capacities

---

## 18. FORD-FULKERSON ALGORITHM

### Process

```
1. x = 0 (initial flow)
2. Create residual network G(x)
3. While ∃ path from s to t in G(x):
   a. Find path P from s to t
   b. F = d(P) (bottleneck capacity)
   c. Send F units along P
   d. Update residual network
4. Output: x is maximum flow
```

### Max-Flow Min-Cut Theorem

```
Maximum flow value = Minimum cut capacity
```

**Cut capacity**: CAP(S,T) = Σ c(u,v) for u∈S, v∈T

---

## 19. MAX FLOW (Excel Formulation)

### Decision Variables

```
fᵢⱼ = flow on arc (i,j)
```

### Objective

```
Maximize: Flow leaving source
```

### Constraints

1. **Flow conservation**: Flow in = Flow out (except s, t)
2. **Capacity**: fᵢⱼ ≤ cᵢⱼ for all arcs
3. **Non-negativity**: fᵢⱼ ≥ 0

---

## KEY FORMULAS SUMMARY

### Complete Graph

```
Edges in Kₙ = n(n-1)/2 = C(n,2)
```

### Tree Properties

```
n vertices → n-1 edges (for tree)
```

### Dijkstra Update

```
D(v) = min(D(v), D(w) + c(w,v))
```

### Flow Conservation

```
Σ f(i,v) - Σ f(v,j) = 0 (for v ≠ s,t)
```

### Residual Capacity

```
Forward: capacity - flow
Backward: flow
```

---

## IMPORTANT CONCEPTS

### Graph Problems & Solutions

| Problem              | Algorithm/Method      |
| -------------------- | --------------------- |
| Minimum cost network | MST (Prim's)          |
| Shortest path        | Dijkstra              |
| Maximum flow         | Ford-Fulkerson        |
| Scheduling/Coloring  | Graph coloring        |
| Network reliability  | Max flow (capacity=1) |
| Resource allocation  | Bipartite matching    |

### Key Properties

1. **Tree**: Connected + no circuits → n-1 edges
2. **Bipartite**: 2-colorable
3. **Planar**: Can draw without edge crossings
4. **Connected**: Path exists between any two vertices
5. **Simple**: No loops or parallel edges

---

## EXAM ESSENTIALS

### Definitions to Know

1. Walk, Trail, Path, Circuit
2. Cut vertex, Cut set, Cut
3. Spanning tree, MST
4. Bipartite graph
5. Component
6. Degree (δ and Δ)
7. Flow network
8. Residual network
9. Augmenting path

### Key Algorithms

1. **Prim's** (MST)
2. **Dijkstra's** (Shortest path)
3. **Ford-Fulkerson** (Max flow)
4. **Graph coloring** (Bipartite test)

### Common Exam Questions

1. Draw graph from adjacency/incidence matrix
2. Find MST using Prim's
3. Calculate shortest path with Dijkstra
4. Determine if graph is bipartite (coloring)
5. Find maximum flow (Ford-Fulkerson)
6. Identify cut vertices and cut sets
7. Formulate as Excel Solver problem
8. Check if graph is planar
9. Count edges in complete graph
10. Find components of disconnected graph

---

## PROBLEM-SOLVING STRATEGIES

### MST (Prim's Algorithm)

1. Start with any vertex
2. Always choose lightest edge connecting known to unknown vertices
3. Avoid creating cycles
4. Stop when all vertices included

### Shortest Path (Dijkstra)

1. Maintain distance table and predecessor table
2. Always pick unvisited node with smallest distance
3. Update neighbors' distances
4. Reconstruct path using predecessor table

### Max Flow (Ford-Fulkerson)

1. Start with zero flow
2. Find augmenting path in residual network
3. Push maximum possible flow
4. Update residual capacities
5. Repeat until no augmenting path exists

### Bipartite Test

1. Color starting vertex (e.g., red)
2. Color neighbors opposite color (blue)
3. Continue alternating
4. Conflict? → Not bipartite
5. Success? → Bipartite

---

## APPLICATIONS SUMMARY

### Real-World Uses

1. **Telecommunication**: MST for network design
2. **Transportation**: Shortest path for routing
3. **Supply chain**: Max flow for throughput
4. **Scheduling**: Graph coloring for conflicts
5. **Circuit design**: Planar graphs
6. **Social networks**: Community detection (components)
7. **Computer networks**: Routing protocols
8. **GPS**: Shortest path algorithms

---

## COMMON MISTAKES TO AVOID

1. **Forgetting loops count twice** in degree calculation
2. **Confusing walk, trail, path** (different restrictions!)
3. **Wrong edge count** in complete graph: n(n-1)/2 not n²
4. **Not checking flow conservation** at ALL nodes (except s, t)
5. **Forgetting residual backward edges** in max flow
6. **Not updating** predecessors in Dijkstra
7. **Creating cycles** in MST
8. **Assuming all graphs connected** (check components!)

---

**Pro Tip**: Master the THREE major algorithms (Prim's, Dijkstra, Ford-Fulkerson) - they're the workhorses of graph problems! Also, practice Excel Solver formulations - exams love testing both manual and computational approaches!
