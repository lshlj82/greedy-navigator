# Greedy Navigator

An interactive demo of the **Greedy Spatial Navigation (GSN)** algorithm introduced in:

> Lee, S. H. & Holme, P. (2012). Exploring Maps with Greedy Navigators. *Physical Review Letters*, 108, 128701.  
> https://link.aps.org/doi/10.1103/PhysRevLett.108.128701

**Live demo:** https://lshlj82.github.io/greedy-navigator/

*Demo created by Claude Sonnet 4.6.*

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

## Features

- **100 real city road networks** from the original paper's dataset — 20 cities each from the US, Europe, Asia, Latin America, and Africa (2 km × 2 km excerpts via OpenStreetMap)
- **Random graph mode** — procedurally generated perturbed grid networks for quick experimentation
- Step-by-step animation with adjustable speed
- Live comparison of $d_g$, $d$, $d_r$, $\nu$, and backtrack count
- Angle visualization showing how GSN selects its next move

---

## Dataset

The road network data is the same dataset used in Fig. 4 of the paper. City excerpts were extracted using [Merkaartor](http://merkaartor.be/) from [OpenStreetMap](https://www.openstreetmap.org/) data. Original data by Lee & Holme, distributed under the OpenStreetMap license (ODbL).

---

## Usage

Open `index.html` in any modern browser — no server or build step required. All city data is embedded in the file.

1. Select a continent and city (or switch to **Random Graph** mode)
2. Click any intersection to set the **source (S)**
3. Click another to set the **target (T)**
4. Choose an algorithm and press **Play**

---

## Reference

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
```
