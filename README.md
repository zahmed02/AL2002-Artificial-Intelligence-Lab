I’ll show the **alpha‑beta pruning process step by step** using ASCII trees. Each step includes the current **α** and **β** values for the node being evaluated, and pruned branches are marked with `✗`.

---

## Tree Legend
- **MAX** nodes (root) – try to maximize.
- **MIN** nodes (A, B, C) – try to minimize.
- Leaves are MAX nodes with numeric values.
- Annotated as `(α, β)` at each node when visited.

---

### Step 0 – Initial tree (before search)
```
            MAX ( -∞, +∞ )
           /      |      \
          /       |       \
    MIN A       MIN B     MIN C
   /  |  \     /  |  \   /  |  \
  2   3   4   1   5   6  7   8   9
```

---

### Step 1 – Visit A (MIN), evaluate first leaf (2)
- Node A gets `α = -∞, β = +∞` from root.
- Leaf returns 2 → A updates β = min(+∞, 2) = 2.

```
            MAX ( -∞, +∞ )
           /      |      \
          /       |       \
    MIN A       MIN B     MIN C
   ( -∞,2 )    ( -∞,+∞ ) ( -∞,+∞ )
   /  |  \     /  |  \   /  |  \
  2   3   4   1   5   6  7   8   9
```

---

### Step 2 – A evaluates second leaf (3)
- Leaf returns 3 → A updates β = min(2, 3) = 2 (unchanged).

```
            MAX ( -∞, +∞ )
           /      |      \
          /       |       \
    MIN A       MIN B     MIN C
   ( -∞,2 )    ( -∞,+∞ ) ( -∞,+∞ )
   /  |  \     /  |  \   /  |  \
  2   3   4   1   5   6  7   8   9
```

---

### Step 3 – A evaluates third leaf (4)
- Leaf returns 4 → A updates β = min(2, 4) = 2.
- A returns 2 to root.

```
            MAX ( -∞, +∞ )
           /      |      \
          /       |       \
       2 (A)     MIN B     MIN C
                ( -∞,+∞ ) ( -∞,+∞ )
                /  |  \   /  |  \
               1   5   6  7   8   9
```

---

### Step 4 – Root updates α = max(-∞, 2) = 2
- Now root: `α = 2, β = +∞`.

```
            MAX ( 2, +∞ )
           /      |      \
          /       |       \
       2 (A)     MIN B     MIN C
                ( 2,+∞ )  ( 2,+∞ )
                /  |  \   /  |  \
               1   5   6  7   8   9
```

---

### Step 5 – Visit B (MIN) with (α=2, β=+∞)
- Evaluate B’s first leaf (1).
- B sets β = min(+∞, 1) = 1.
- Now at B: `α = 2, β = 1` → **β ≤ α**, so **prune** the remaining children (5, 6).

```
            MAX ( 2, +∞ )
           /      |      \
          /       |       \
       2 (A)     MIN B     MIN C
                ( 2,1 )   ( 2,+∞ )
                /  |  \   /  |  \
               1   ✗   ✗  7   8   9
```

---

### Step 6 – B returns 1 to root
- Root updates α = max(2, 1) = 2 (unchanged).

```
            MAX ( 2, +∞ )
           /      |      \
          /       |       \
       2 (A)     1 (B)    MIN C
                         ( 2,+∞ )
                         /  |  \
                        7   8   9
```

---

### Step 7 – Visit C (MIN) with (α=2, β=+∞)
- Evaluate C’s first leaf (7).
- C sets β = min(+∞, 7) = 7.

```
            MAX ( 2, +∞ )
           /      |      \
          /       |       \
       2 (A)     1 (B)    MIN C
                         ( 2,7 )
                         /  |  \
                        7   8   9
```

---

### Step 8 – C evaluates second leaf (8)
- Returns 8 → β = min(7, 8) = 7 (unchanged).

```
            MAX ( 2, +∞ )
           /      |      \
          /       |       \
       2 (A)     1 (B)    MIN C
                         ( 2,7 )
                         /  |  \
                        7   8   9
```

---

### Step 9 – C evaluates third leaf (9)
- Returns 9 → β = min(7, 9) = 7.
- C returns 7 to root.

```
            MAX ( 2, +∞ )
           /      |      \
          /       |       \
       2 (A)     1 (B)     7 (C)
```

---

### Step 10 – Root updates α = max(2, 7) = 7
- Final root value = 7. Pruned branches (✗) were never explored.

```
            MAX ( 7, +∞ )
           /      |      \
          /       |       \
       2 (A)     1 (B)     7 (C)
```

---

### Summary of Alpha Cut
- The **alpha cut** happened at node **B** after seeing the first child (value 1), because the current β (1) became ≤ the α value from the root (2). This made exploring the remaining children (5 and 6) unnecessary.
