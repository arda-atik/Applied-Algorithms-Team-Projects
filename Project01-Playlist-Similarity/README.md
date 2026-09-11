# Project 01: Playlist Similarity (List Scanning vs. Set Operations)

**Course:** Basics of Algorithmization / Algorithms (ČVUT / CTU)  
**Authors:** Arda Atik, Sviatoslav Bezhenar, Efe Hacıoğlu  
**Key Focus:** Set Theory, Jaccard Index, Empirical Benchmarking, Hash-based Optimization  

---

## 1. Problem Statement
Given two users' playlists containing song IDs ($A$ and $B$), compute:
1. Number of common songs (Intersection: $|A \cap B|$)
2. Total unique songs (Union: $|A \cup B|$)
3. Similarity Score using the **Jaccard Index**:
$$\text{Jaccard}(A, B) = \frac{|A \cap B|}{|A \cup B|}$$

### Edge Cases Handled
- **Both playlists empty:** Jaccard $= 0$
- **One playlist empty:** Intersection $= 0$
- **Identical playlists:** Jaccard $= 1.0$
- **No overlap:** Jaccard $= 0$
- **Duplicate song IDs:** Deduplicated under set semantics

---

## 2. Algorithmic Approaches

### Approach A — Baseline (List Scanning)
- Scans list $B$ for every song in $A$, using a small `seen` tracking set to prevent double counting.
- **Time Complexity:** $O(n \cdot m)$
- **Space Complexity:** Low auxiliary memory
- **Drawback:** Quadratic scaling makes it unusable for large user libraries.

### Approach B — Hash-Set Operations (Standard)
- Converts both lists into hash sets and leverages built-in set intersection ($A \cap B$) and union ($A \cup B$).
- **Time Complexity:** $O(n + m)$
- **Space Complexity:** Moderate (hash tables in memory)
- **Takeaway:** Optimal general-purpose approach for batch and one-off queries.

### Approach C — Cached Sets (System Design Extension)
- Wraps playlists in a class holding both the raw list and the precomputed set.
- Reuses the set for repeated pairwise comparisons against large user bases.

---

## 3. Empirical Benchmarks

Execution time comparisons across varying playlist sizes and overlap ratios:

### Benchmark: 0% Overlap
| Playlist Size | Approach A (List Scan) | Approach B (Set Method) | Speedup Factor |
| :--- | :--- | :--- | :--- |
| **1,000** | 18 ms | 1.2 ms | ~15x |
| **5,000** | 420 ms | 5.8 ms | ~72x |
| **10,000** | 1,650 ms | 11.4 ms | ~144x |
| **50,000** | 41,000 ms | 58 ms | ~706x |
| **100,000** | 165,000 ms (2.75 min) | 120 ms (0.12 s) | **~1,375x** |

### Benchmark: 50% Overlap
| Playlist Size | Approach A (List Scan) | Approach B (Set Method) |
| :--- | :--- | :--- |
| **1,000** | 15 ms | 1.3 ms |
| **5,000** | 350 ms | 6.1 ms |
| **10,000** | 1,500 ms | 12.0 ms |
| **50,000** | 37,000 ms | 60.0 ms |
| **100,000** | 150,000 ms | 125.0 ms |

---

## 4. Key Takeaways
- For realistic dataset sizes (100k items), hash-set intersection drops runtime from nearly 3 minutes down to 120 ms.
- Duplicates in list inputs must be carefully handled in list-scanning algorithms to prevent skewed union metrics, while set semantics resolve this naturally.
