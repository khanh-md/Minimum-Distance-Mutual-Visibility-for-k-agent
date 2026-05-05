# Minimum Distance Mutual Visibility for k-agents

This repository contains the experimental study and algorithmic implementation for the **$k$-agent Distance-to-Sight Mutual Visibility Problem**. 

The primary objective of this project is to coordinate an unanchored group of mobile agents within a bounded, non-convex environment (such as a complex floor plan or maze). The goal is to find the absolute minimum distance the agents must travel to reach a state of **Mutual Visibility**, a configuration where every single agent has a straight, unobstructed line of sight to every other agent simultaneously.

## Key Features

* **Complex Environment Generation:** Dynamically generates random non-convex, chaotic polygons and star-shaped domains to test algorithmic limits.
* **Topological Space Partitioning:** Utilizes "essential cuts" to fracture complex mazes into convex sub-cells where visibility is mathematically guaranteed.
* **Geodesic Routing:** Calculates shortest paths constrained entirely within the polygon boundaries using topological graphs.
* **Automated Benchmarking:** Includes headless execution scripts to benchmark algorithmic time complexity, scaling, and success rates across randomized parameters.

## Algorithms Implemented

### 1. Greedy Centroid Heuristic (Baseline)
An iterative, vector-based approach where agents calculate the geometric mean of the group and step toward it.
* **Success:** Works flawlessly in convex and star-shaped environments by naturally guiding agents into the central visibility kernel.
* **Failure State:** Frequently fails in non-convex polygons. Because it lacks awareness of reflex vertices, agents become trapped against walls in "local minima" (geographically close, but visually separated).
* **Complexity:** Time $O(T \cdot k)$, Space $O(k + n)$.

### 2. Geodesic Topological Decomposition 
A robust, Fixed-Parameter Tractable (FPT) algorithm designed to guarantee success in highly complex, non-convex geometry. 
* **Mechanism:**
    1. Generates a **Geodesic Convex Hull** to prune the search space.
    2. Projects **Essential Cuts** from reflex vertices to partition the maze into convex sub-cells.
    3. Identifies the "Mutual Visibility Kernel" (the optimal meeting room) and routes agents via Dijkstra's shortest path.
    4. Performs **Ray-Casting Backtracking** based on the *Tangency Lemma*, stopping agents at the exact threshold where their line of sight grazes a reflex corner to ensure the absolute minimum travel distance.
* **Complexity:** Time $O(r^{k-1}n)$, Space $O(n + r^2 + k)$, where $r$ is the number of reflex vertices.

