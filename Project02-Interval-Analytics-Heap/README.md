# Project 02: Interval Analytics & Resource Allocation

**Course:** Basics of Algorithmization / Algorithms (ČVUT / CTU)  
**Authors:** Arda Atik, Sviatoslav Bezhenar, Efe Hacıoğlu  
**Key Focus:** Interval Scheduling, Sweep-Line Algorithms, Min-Heap, Concurrency Analysis  

---

## 1. Problem Statement
Given a set of half-open intervals $[start, end)$ representing room or machine reservations, compute the following metrics from a single dataset:
1. **Merged Busy Intervals:** Total non-overlapping active windows.
2. **Total Busy Time:** Total duration the facilities are in use.
3. **Maximum Overlap:** Peak concurrency level (maximum simultaneous meetings).
4. **Capacity Breach Time:** Earliest timestamp where concurrent usage strictly exceeds capacity $C$.
5. **Optimal Resource Allocation:** Assign intervals to the minimum number of rooms/machines.

### Edge Cases Handled
- **Touching intervals:** $[1, 3)$ and $[3, 5)$ (should not count as overlap).
- **Nested intervals:** $[1, 10)$ and $[3, 4)$.
- **Identical endpoints & zero-length intervals.**
- **Duplicate intervals:** $[2, 5), [2, 5), [2, 5)$ (overlap $= 3$).

---

## 2. Algorithmic Approaches

### Approach A — Baseline (Naive Pairwise Check)
- Scans all pairs to compute peak overlap.
- **Complexity:** $O(n^2)$ time, $O(1)$ auxiliary space.
- **Drawback:** Unusable on large datasets due to quadratic scaling.

### Approach B — Sort by Start Time & Iterative Merge
- Sorts intervals once by `start_time` and maintains an active end-time list.
- Computes merged busy intervals and peak overlap.
- **Complexity:** $O(n \log n)$ time, $O(n)$ space.

### Approach C — Sweep-Line with Min-Heap (Optimal)
- Sorts intervals by `start_time` and uses a **Min-Heap** to track active finish times.
- Frees rooms when `heap.min <= start` and pushes new finish times in $O(\log k)$ time.
- Resolves room allocation, peak concurrency, and capacity breach tracking in a single pass.
- **Complexity:** $O(n \log n)$ time, $O(n)$ space.

---

## 3. Empirical Benchmarks

Execution time comparisons across varying interval counts:

| Number of Intervals | Baseline $O(n^2)$ | Sort & Merge $O(n \log n)$ | Sweep-line with Min-Heap $O(n \log n)$ |
| :--- | :--- | :--- | :--- |
| **1,000** | 18 ms | 2 ms | 3 ms |
| **5,000** | 420 ms | 12 ms | 16 ms |
| **10,000** | 1,650 ms | 25 ms | 35 ms |
| **50,000** | 41,000 ms | 145 ms | 190 ms |
| **100,000** | 165,000 ms (2.75 min) | 320 ms | 450 ms |

---

## 4. Weighted Extension
- Implemented a greedy strategy for priority-weighted meetings under a fixed capacity constraint $C$.
- When concurrency exceeds $C$, the lowest-weight meeting in the active pool is discarded to maximize total schedule value.
