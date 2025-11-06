# ENGG515 - Quick Study Guide

## Nonlinear Optimization

---

## 1. FUNDAMENTAL DEFINITIONS

### General Problem Form

```
Maximize: f(x)
Subject to: gᵢ(x) ≤ bᵢ for i = 1,2,...,m
            x ≥ 0
```

Where:

- x = [x₁, x₂, ..., xₙ]ᵀ ∈ Rⁿ (decision variables)
- W = feasible solution set
- f(x) = objective/cost function

### Key Concepts

**Neighborhood N(x)**:

```
N(x) = {y ∈ W : ||x - y|| < ε}
```

- ε > 0 (very small)
- Euclidean distance: ||x - y|| = √[(x₀-y₀)² + (x₁-y₁)² + ... + (xₙ-yₙ)²]

**Local Maximizer x\***:

- f(x*) ≥ f(x) for any x ∈ N(x*)
- **Strict**: f(x*) > f(x) for any x ∈ N(x*)

**Global Maximizer x\***:

- f(x\*) ≥ f(x) for any x ∈ W
- **Strict**: f(x\*) > f(x) for any x ∈ W

**Major Difficulty**: Algorithms can't easily differentiate local vs global maximizers!

---

## 2. LEVEL SETS & BOUNDARIES

### Level Set at Level c

```
S = {x ∈ W : f(x) = c}
```

Set of all points where function equals c

### Interior Point

- Point x ∈ W where neighborhood N(x) completely in W

### Boundary Point

- Every neighborhood has points in W AND outside W
- Set of all boundary points = boundary of W
- Note: Some sets have NO boundary (e.g., ]-1, 1[)

---

## 3. SEARCH DIRECTION & DERIVATIVES

### Feasible Direction d

Vector d ∈ Rⁿ with d ≠ 0 is feasible at x ∈ W if:

- ∃ α₀ > 0 such that x + αd ∈ W for all α ∈ [0, α₀]

### Directional Derivative

```
∂f/∂d = lim[α→0] [f(x+αd) - f(x)]/α = dᵀ∇f(x)
```

**For unit vector d** (||d|| = 1):

- ∂f/∂d = rate of increase of f at x in direction d

**Key Insights**:

- If ∂f/∂d > 0 → f increases in direction d
- **Maximum increase**: Direction of gradient ∇f(x)
- Maximum rate: ||∇f(x)||

---

## 4. CONVEX/CONCAVE FUNCTIONS

### Convex Set W ⊂ Rⁿ

For any x', x'' ∈ W and t ∈ [0,1]:

```
(1-t)x' + tx'' ∈ W
```

(Line segment joining any two points stays in W)

### Concave Function

On convex set W:

```
(1-t)f(x') + tf(x'') ≤ f((1-t)x' + tx'')
```

- Line segment NEVER above graph
- "Curved down"

### Convex Function

On convex set W:

```
(1-t)f(x') + tf(x'') ≥ f((1-t)x' + tx'')
```

- Line segment NEVER below graph
- "Curved up"

**Important Properties**:

- Sum of concave functions = concave
- Sum of convex functions = convex
- **Concave + maximize → local max = global max** (if ∂²f/∂x² ≤ 0)
- **Convex + minimize → local min = global min** (if ∂²f/∂x² ≥ 0)

---

## 5. UNCONSTRAINED OPTIMIZATION

### Necessary Condition (Differentiable f)

```
∂f/∂xⱼ = 0 for j = 1,2,...,n
```

**Sufficient Condition**: If f is concave (for maximization) or convex (for minimization)

---

## 6. ONE-VARIABLE UNCONSTRAINED

### Conditions for Global Optimum

**Necessary & Sufficient** (if f concave):

```
df/dx = 0
```

### Newton Method

**Approximation** (Taylor series):

```
p(x) = f(xᵢ) + f'(xᵢ)(x - xᵢ) + f''(xᵢ)(x - xᵢ)²/2
```

**Update Rule**:

```
xᵢ₊₁ = xᵢ - f'(xᵢ)/f''(xᵢ)
```

**Algorithm**:

1. Find initial x₀, set i = 0
2. Compute f' and f'' at xᵢ
3. Set xᵢ₊₁ = xᵢ - f'(xᵢ)/f''(xᵢ)
4. If |xᵢ₊₁ - xᵢ| < ε then STOP, else i = i+1, goto 2

---

## 7. MULTIVARIABLE UNCONSTRAINED

### Method 1: GRADIENT SEARCH

**Key Idea**: Move in direction of gradient (steepest ascent)

**Algorithm**:

1. Find initial X₀, set i = 0
2. Express F(α) by replacing X with Xᵢ₊₁ = Xᵢ + α∇f(Xᵢ)
3. Maximize F(α) (one-variable problem!)
4. If stopping criterion met, STOP; else i = i+1, goto 2

**Stopping Criterion**: |∂F/∂xⱼ| ≤ ε for all j

**Geometric Interpretation**:

- Start at X⁽⁰⁾
- Move in ∇f direction
- Stop when gradient tangent to level set
- New gradient perpendicular to level set
- Repeat

### Method 2: NEWTON'S METHOD

**Update Rule**:

```
X⁽ⁱ⁺¹⁾ = X⁽ⁱ⁾ - [F(X⁽ⁱ⁾)]⁻¹ ∇f(X⁽ⁱ⁾)
```

Where F(X) = Hessian matrix:

```
F = [∂²f/∂xᵢ∂xⱼ]
```

**Algorithm**:

1. Select starting point X⁽⁰⁾, compute f(X⁽⁰⁾)
2. Compute gradient ∇f and Hessian F
3. Compute X⁽ⁱ⁺¹⁾ = X⁽ⁱ⁾ - [F(X⁽ⁱ⁾)]⁻¹ ∇f(X⁽ⁱ⁾)
4. If stopping criterion met, STOP; else goto 2

---

## 8. CONSTRAINED OPTIMIZATION

### Lagrangian Method

**Original Problem**:

```
Maximize: f(X)
Subject to: gᵢ(X) ≤ bᵢ for i = 1,...,m
```

**Lagrangian Function**:

```
F(X, λ) = f(X) - Σᵢ₌₁ᵐ λᵢ[gᵢ(X) - bᵢ]
```

Where λ = [λ₁, λ₂, ..., λₘ] = Lagrange multipliers

**Process**:

1. Form Lagrangian (n+m variables, unconstrained)
2. Set all partial derivatives = 0
3. Solve system of equations
4. Solution (X*, λ*) gives critical points

**Key Insight**: Converts constrained → unconstrained problem!

---

## 9. DUALITY

### Primal Problem

```
Minimize: f(x)
Subject to: hᵢ(x) = 0 for i = 1,...,m
            gⱼ(x) ≤ 0 for j = 1,...,r
```

### Lagrangian

```
L(x,λ,μ) = f(x) + Σᵢλᵢhᵢ(x) + Σⱼμⱼgⱼ(x)
```

### Dual Function

```
q(λ,μ) = inf[x∈Rⁿ] L(x,λ,μ)
```

### Dual Problem

```
Maximize: q(λ,μ)
Subject to: μ ≥ 0
```

**Properties**:

- q is **always concave** (even if f not convex!)
- λ, μ = dual variables

---

## 10. DUALITY THEOREMS

### Weak Duality

```
d* ≤ f*
```

Where:

- d\* = optimal dual value
- f\* = optimal primal value
- **Duality gap** = f* - d*

**Interpretation**: Dual gives lower bound for primal (minimization)

### Strong Duality

```
d* = f*
```

- Duality gap = 0
- Dual optimal = Primal optimal
- Doesn't always hold!

---

## KEY FORMULAS SUMMARY

### Distances & Derivatives

```
||x - y|| = √[Σ(xᵢ - yᵢ)²]
∂f/∂d = dᵀ∇f(x)
```

### Gradient & Hessian

```
∇f(x) = [∂f/∂x₁, ∂f/∂x₂, ..., ∂f/∂xₙ]ᵀ
F(x) = [∂²f/∂xᵢ∂xⱼ]  (Hessian)
```

### Newton Update

```
One variable: xᵢ₊₁ = xᵢ - f'(xᵢ)/f''(xᵢ)
Multi-variable: X⁽ⁱ⁺¹⁾ = X⁽ⁱ⁾ - [F(X⁽ⁱ⁾)]⁻¹∇f(X⁽ⁱ⁾)
```

### Gradient Search

```
Xᵢ₊₁ = Xᵢ + α∇f(Xᵢ)
```

### Lagrangian

```
F(X,λ) = f(X) - Σᵢλᵢ[gᵢ(X) - bᵢ]
```

### 2×2 Matrix Tests

For A = [a b; c d]:

- **Positive definite**: a > 0 AND det(A) > 0
- **Negative definite**: a < 0 AND det(A) > 0
- **Indefinite**: det(A) < 0

### 2×2 Matrix Inverse

```
A⁻¹ = (1/det(A)) [d -b; -c a]
where det(A) = ad - bc
```

---

## IMPORTANT CONCEPTS

### Optimization Strategy Selection

| Problem Type            | Best Method          | Why                             |
| ----------------------- | -------------------- | ------------------------------- |
| 1-var unconstrained     | Newton               | Fast convergence                |
| Multi-var unconstrained | Gradient or Newton   | Gradient simpler, Newton faster |
| Constrained             | Lagrangian           | Converts to unconstrained       |
| Dual needed             | Lagrangian + Duality | Get bounds, dual problem        |

### Concave/Convex Importance

1. **Concave + maximize**: Local max = Global max
2. **Convex + minimize**: Local min = Global min
3. **Otherwise**: Only find local optima!

### When to Use What

- **Gradient Search**: Simple, reliable, slower
- **Newton's Method**: Fast, needs Hessian (expensive)
- **Lagrangian**: When have constraints
- **Duality**: Get bounds, sometimes easier to solve

---

## EXAM ESSENTIALS

### Definitions to Know

1. Local vs Global maximizer
2. Convex set, Concave/Convex function
3. Feasible direction
4. Directional derivative
5. Level set
6. Interior vs Boundary points
7. Lagrange multipliers
8. Dual function
9. Weak vs Strong duality

### Key Concepts

1. Gradient points in direction of maximum increase
2. Newton uses second-order information (Hessian)
3. Concave function + maximize → local = global
4. Lagrangian converts constrained to unconstrained
5. Dual provides lower bounds (weak duality)
6. Strong duality: dual optimal = primal optimal

### Common Exam Questions

1. Find local/global maximizer for given function
2. Apply Newton method (1 or 2 iterations)
3. Apply Gradient search (1 or 2 iterations)
4. Form Lagrangian for constrained problem
5. Find critical points using Lagrangian
6. Sketch level sets
7. Determine if function convex/concave
8. Find directional derivative
9. Set up dual problem
10. Calculate duality gap

---

## PRACTICAL TIPS

### Problem Solving Steps

**Unconstrained**:

1. Check if concave/convex
2. Choose method (Gradient or Newton)
3. Set stopping criterion
4. Iterate until convergence

**Constrained**:

1. Form Lagrangian
2. Take partial derivatives
3. Set all = 0
4. Solve system
5. Check which critical point is optimal

### Common Mistakes to Avoid

1. Forgetting Lagrange multipliers for each constraint
2. Not checking second derivatives (concave/convex test)
3. Confusing local vs global optima
4. Wrong signs in Lagrangian (should be minus!)
5. Not normalizing direction vectors when needed

### Variable Changes

- **x restricted negative**: Use x = -x' with x' ≥ 0
- **x unrestricted**: Use x = x' - x'' with x', x'' ≥ 0
- **Constraint g(x) ≤ -b**: Multiply by -1 to get -g(x) ≤ b

---

**Pro Tip**: Master NEWTON'S METHOD and LAGRANGIAN - they're the workhorses of nonlinear optimization! Also, always check if function is concave/convex first - it tells you if local optimum is global!
