# Greedy Navigator

Interactive demos of the **Greedy Spatial Navigation (GSN)** algorithm, its layout optimization, navigable city plan construction, and maze navigation, based on three papers:

> Lee, S. H. & Holme, P. (2012). Exploring Maps with Greedy Navigators. *Physical Review Letters*, 108, 128701.
> https://link.aps.org/doi/10.1103/PhysRevLett.108.128701

> Lee, S. H. & Holme, P. (2012). Geometric properties of graph layouts optimized for greedy navigation. *Physical Review E*, 86, 067103.
> https://link.aps.org/doi/10.1103/PhysRevE.86.067103

> Lee, S. H. & Holme, P. (2013). A greedy-navigator approach to navigable city plans. *The European Physical Journal Special Topics*, 215, 135–144.
> https://doi.org/10.1140/epjst/e2013-01720-8

**Live demo (navigation):** https://lshlj82.github.io/greedy-navigator/

**Live demo (optimizer):** https://lshlj82.github.io/greedy-navigator/GSN_optimizer.html

**Live demo (city plans):** https://lshlj82.github.io/greedy-navigator/GSN_shortcut.html

**Live demo (maze):** https://lshlj82.github.io/greedy-navigator/maze_navigator.html

*Demos created by Claude Sonnet 4.6.*

---

## Authors

**Sang Hoon Lee**
- IceLab, Department of Physics, Umeå University, 901 87 Umeå, Sweden
- Oxford Centre for Industrial and Applied Mathematics, Mathematical Institute, University of Oxford, Oxford OX1 3LB, UK *(PRE 2012, EPJST 2013)*
- Department of Energy Science, Sungkyunkwan University, Suwon 440-746, Korea *(PRL 2012)*

**Petter Holme**
- IceLab, Department of Physics, Umeå University, 901 87 Umeå, Sweden
- Department of Energy Science, Sungkyunkwan University, Suwon 440-746, Korea
- Department of Sociology, Stockholm University, 106 91 Stockholm, Sweden

---

## What is Greedy Spatial Navigation?

Real-world navigation happens with incomplete information — we rarely have a perfect map, but we do have a sense of direction. The GSN model captures this: an agent moves toward whichever neighboring intersection most closely points in the direction of the target. When completely stuck (all neighbors already visited), it backtracks.

Three routing strategies are compared:

| Strategy | Description |
|---|---|
| **GSN** | Greedy Spatial Navigation — always moves to the unvisited neighbor with the smallest angle toward the target |
| **SPN** | Shortest Path Navigation — BFS optimal path, requires complete map knowledge |
| **Random DFS** | Self-avoiding walk — moves to a random unvisited neighbor; backtracks only when all neighbors have been visited |

The **navigability** of a network is measured as:

$$\nu = \frac{d}{d_g}$$

where $d$ is the shortest path length (SPN) and $d_g$ is the total steps taken by the greedy navigator. $\nu \to 1$ means the city layout allows near-optimal greedy routing.

---

## Files

### `index.html` — GSN Navigation Demo (PRL 2012)

Explores navigability on real and synthetic road networks.

**Features:**
- **100 real city road networks** from the original paper's dataset — 20 cities each from the US, Europe, Asia, Latin America, and Africa (2 km × 2 km excerpts via OpenStreetMap)
- **Random graph mode** — procedurally generated perturbed grid networks for quick experimentation
- Step-by-step animation with adjustable speed
- Live comparison of $d_g$, $d$, $d_r$, $\nu$, and backtrack count
- Angle visualization showing how GSN selects its next move at each intersection

**Usage:**
1. Select a continent and city (or switch to **Random Graph** mode)
2. Click any intersection to set the **source (S)**
3. Click another to set the **target (T)**
4. Choose an algorithm and press **Play**

---

### `GSN_optimizer.html` — Layout Optimization via Simulated Annealing (PRE 2012)

Demonstrates the reverse problem: instead of navigating a given map, find the vertex layout that *minimizes* $d_g$ for a fixed graph topology, using Simulated Annealing (SA).

At each trial step, a randomly chosen vertex is perturbed. The new layout is accepted if $d_g$ improves, or with probability $p_\text{high}$ during heating (allowing escape from local minima). Quenching then freezes the layout until convergence, and the process cycles for multiple sessions, always recording the best layout found.

**Features:**
- Five graph models: Barabási-Albert, Watts-Strogatz, 1D Ring, 2D Grid, Zachary Karate Club
- Split-screen view: SA-optimized layout (top) vs. initial random layout (bottom)
- $d_g$ time series chart with heating/quenching phase bands
- **Test GSN before/after**: pick any source–target pair and animate the GSN path simultaneously on both layouts, with angle spokes showing the navigator's decision at each step
- Live stats: $d_g$ initial → best, $\nu$, improvement %

**Usage:**
1. Select a graph model and press **▶ Run SA** to start optimization
2. Once SA finishes, click **⊕ Pick S/T** and select two nodes
3. Press **▶ Run GSN** to animate the navigator on both layouts simultaneously

---

### `GSN_shortcut.html` — Navigable City Plan Construction (EPJST 2013)

Grows a road network from scratch by greedily adding shortcuts to a Minimum Spanning Tree (MST) skeleton, optimizing for one of four navigability metrics under a total edge-length budget.

At each step, every candidate edge is evaluated and the one that most improves the chosen metric is added — subject to a length budget and, optionally, a no-crossing rule that prevents intersections from creating unintended junctions.

**Four strategies:**

| Strategy | Routing | Metric |
|---|---|---|
| **GSNH** | GSN | Hop count |
| **GSNE** | GSN | Euclidean path length |
| **SPNH** | Shortest path | Hop count |
| **SPNE** | Shortest path | Euclidean path length |

**Key findings reproduced:** hopping-distance strategies produce hub nodes (fat-tailed degree distributions); Euclidean-distance strategies produce triangular block structures. Disabling the no-crossing rule with SPNH leads to star-graph condensation — a collapse avoided naturally by GSNH.

**Features:**
- Animated shortcut construction step by step
- Shortcut edges color-coded by addition order (blue → violet)
- Live degree distribution and metric history charts
- No-crossing rule toggle

**Usage:**
1. Choose a strategy and length budget, then press **⊕ Generate New Graph**
2. Press **▶ Run** to watch the network grow, or **▸ Step** to advance one shortcut at a time
3. Compare GSNH vs GSNE to see hub vs. triangular-block emergence

---

### `maze_navigator.html` — Maze Navigation (PRL 2012)

Generates perfect mazes (connected spanning trees of a grid) and animates all three routing strategies on them. Since a perfect maze is a tree — exactly one path between any two cells — the SPN solution is always unique, making the cost of incomplete information (captured by $\nu = d/d_g$) especially vivid.

**Three maze generation algorithms:**

| Algorithm | Character | Reference |
|---|---|---|
| **DFS (Backtracker)** | Long winding corridors, river-like passages, fewest dead ends | Tarjan (1972) |
| **Prim's** | Many short dead ends branching off a central structure, bushy topology | Prim (1957) |
| **Kruskal's** | Uniformly random spanning tree, balanced between the two | Kruskal (1956) |

All three algorithms generate a **perfect maze** — a spanning tree of the grid graph in which every cell is reachable and there are no loops. Applied to maze generation, each algorithm produces a uniformly or near-uniformly random spanning tree with a characteristic structural bias determined by the order in which edges are selected.

**Features:**
- Adjustable maze size (width 8–30, height 6–22)
- Click any cell to set source S; click S again to cancel and re-select
- Click another cell to set target T
- **⚄ Random S/T** button for instant random placement
- **▶ Navigate Yourself** — use arrow keys or click highlighted adjacent cells to find your own path; Backspace to undo; your step count and backtrack count appear in the results panel and are compared against the algorithms when you reach T
- Step-by-step or continuous animation for GSN, SPN, and Random DFS
- Angle-spoke visualization showing the direction decision at each GSN step
- Live $d_g$, $d$, $d_r$, $\nu$, and backtrack count

**Usage:**
1. Choose a generation algorithm and size, then press **⊕ Generate Maze**
2. Click any cell for S, another for T (or use **⚄ Random S/T**)
3. Press **▶ Navigate Yourself** to explore the maze manually with arrow keys, then run **GSN**, **SPN**, or **RND** on the same pair to compare

---

## Dataset

The road network data used in `index.html` is the same dataset as Fig. 4 of the PRL paper. City excerpts were extracted using [Merkaartor](http://merkaartor.be/) from [OpenStreetMap](https://www.openstreetmap.org/) data. Original data by Lee & Holme, distributed under the OpenStreetMap license (ODbL).

---

## References

```bibtex
@article{lee2012exploring,
  title   = {Exploring Maps with Greedy Navigators},
  author  = {Lee, Sang Hoon and Holme, Petter},
  journal = {Physical Review Letters},
  volume  = {108},
  pages   = {128701},
  year    = {2012},
  doi     = {10.1103/PhysRevLett.108.128701}
}

@article{lee2012geometric,
  title   = {Geometric properties of graph layouts optimized for greedy navigation},
  author  = {Lee, Sang Hoon and Holme, Petter},
  journal = {Physical Review E},
  volume  = {86},
  pages   = {067103},
  year    = {2012},
  doi     = {10.1103/PhysRevE.86.067103}
}

@article{lee2013greedy,
  title   = {A greedy-navigator approach to navigable city plans},
  author  = {Lee, Sang Hoon and Holme, Petter},
  journal = {The European Physical Journal Special Topics},
  volume  = {215},
  pages   = {135--144},
  year    = {2013},
  doi     = {10.1140/epjst/e2013-01720-8}
}

@article{tarjan1972depth,
  title   = {Depth-first search and linear graph algorithms},
  author  = {Tarjan, Robert},
  journal = {SIAM Journal on Computing},
  volume  = {1},
  number  = {2},
  pages   = {146--160},
  year    = {1972},
  doi     = {10.1137/0201010}
}

@article{prim1957shortest,
  title   = {Shortest connection networks and some generalizations},
  author  = {Prim, Robert C.},
  journal = {Bell System Technical Journal},
  volume  = {36},
  number  = {6},
  pages   = {1389--1401},
  year    = {1957},
  doi     = {10.1002/j.1538-7305.1957.tb01515.x}
}

@article{kruskal1956shortest,
  title   = {On the shortest spanning subtree of a graph and the traveling salesman problem},
  author  = {Kruskal, Joseph B.},
  journal = {Proceedings of the American Mathematical Society},
  volume  = {7},
  number  = {1},
  pages   = {48--50},
  year    = {1956},
  doi     = {10.1090/S0002-9939-1956-0078686-7}
}
```
