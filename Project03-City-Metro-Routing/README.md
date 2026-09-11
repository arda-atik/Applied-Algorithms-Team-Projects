# Project 03: City Metro Stops — Shortest Route with Graph Traversal

**Course:** Basics of Algorithmization / Algorithms (ČVUT / CTU)  
**Authors:** Arda Atik, Sviatoslav Bezhenar, Efe Hacıoğlu  
**Key Focus:** Graph Theory, BFS, DFS, Path Reconstruction, Backtracking  

---

## 1. Problem Statement
Model an unweighted, undirected city metro network where stations represent vertices ($V$) and direct rail tracks represent edges ($E$). Given a starting station and a target destination, compute the shortest path measured by the total number of intermediate transit moves.

### Graph Formulation
- **Vertices ($V$):** Metro stations.
- **Edges ($E$):** Direct bidirectional train connections.
- **Edge Weights:** Unweighted (uniform cost of 1 move per segment).
- **Goal:** Return the reconstructed sequence of stations (e.g., $A \rightarrow C \rightarrow D \rightarrow F$) and total distance.

---

## 2. Traversal Comparison

### Breadth-First Search (BFS) — Optimal Choice
- **Frontier Structure:** First-In-First-Out (FIFO) Queue (`collections.deque`).
- **Mechanics:** Explores the network level-by-level in expanding concentric layers.
- **Guarantee:** Guarantees finding the global shortest path in unweighted graphs because all nodes at distance $k$ are evaluated before any node at distance $k+1$.
- **Complexity:** $O(V + E)$ time, $O(V)$ space.

### Depth-First Search (DFS) — Suboptimal for Routing
- **Frontier Structure:** Last-In-First-Out (LIFO) Stack.
- **Mechanics:** Dives as deep as possible along a single branch before backtracking.
- **Drawback:** Frequently encounters long, inefficient detours and fails to guarantee shortest distance.
- **Complexity:** $O(V + E)$ time, $O(V)$ space.

---

## 3. Path Reconstruction via Parent Backtracking
During graph traversal, a parent pointer map (`parent[node] = predecessor`) records incoming traversal edges. Once the target vertex is reached, the route is reconstructed by tracing backwards from the destination to the origin:

$$\text{Target} \rightarrow \text{Parent}[\text{Target}] \rightarrow \dots \rightarrow \text{Start}$$

Reversing this reconstructed list yields the valid forward transit itinerary.

---

## 4. Test Scenarios Handled
- **Normal Path:** Multi-hop transit traversal across dense junction hubs.
- **Start Equals Target:** Immediate loop termination returning distance 0.
- **Disconnected Components:** Safely detects isolated stations where no valid path exists.
