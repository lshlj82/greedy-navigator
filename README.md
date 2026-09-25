# Greedy Navigator

Interactive demos of the **Greedy Spatial Navigation (GSN)** algorithm and its layout optimization, based on:

> Lee, S. H. & Holme, P. (2012). Exploring Maps with Greedy Navigators. *Physical Review Letters*, 108, 128701.
> https://link.aps.org/doi/10.1103/PhysRevLett.108.128701

> Lee, S. H. & Holme, P. (2012). Geometric properties of graph layouts optimized for greedy navigation. *Physical Review E*, 86, 067103.
> https://link.aps.org/doi/10.1103/PhysRevE.86.067103

**Live demo (navigation):** https://lshlj82.github.io/greedy-navigator/

**Live demo (optimizer):** https://lshlj82.github.io/greedy-navigator/GSN_optimizer.html

*Demos created by Claude Sonnet 4.6.*

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

Demonstrates the reverse problem: instead of navigating a given map, find the vertex layout that *minimizes* $d_g$ for a fixed graph topology.

The optimizer uses **Simulated Annealing (SA)**: at each step, a randomly chosen vertex is perturbed. The new layout is accepted if $d_g$ improves, or with probability $p_\text{high}$ during heating (allowing escape from local minima). Quenching freezes the layout until convergence, and the process cycles for multiple sessions.

**Features:**
- Five graph models: Barabási-Albert, Watts-Strogatz, 1D Ring, 2D Grid, Zachary Karate Club
- Split-screen view: top canvas shows the SA-optimized layout evolving in real time; bottom canvas keeps the initial random layout fixed for direct comparison
- $d_g$ time series chart with heating/quenching phase bands
- **Test GSN before/after**: pick any source–target pair and animate the GSN path simultaneously on both layouts, with angle spokes showing the navigator's decision at each step
- Live stats: $d_g$ initial → best, $\nu$, improvement %, step-by-step backtrack count

**Usage:**
1. Select a graph model and press **▶ Run SA** to start optimization
2. Watch the layout evolve as $d_g$ drops in the chart
3. Once SA finishes (or at any point), click **⊕ Pick S/T** and select two nodes
4. Press **▶ Run GSN** to animate the navigator on both layouts simultaneously

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
```
