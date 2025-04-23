# Equivalent Resistance Using Graph Theory

## 1. Motivation

Calculating equivalent resistance is essential in understanding electrical networks. Traditional methods using series-parallel simplifications become tedious for complex circuits. **Graph theory** offers a powerful algorithmic alternative by modeling circuits as weighted graphs:
- **Nodes** represent junctions.
- **Edges** represent resistors with weights equal to resistance values.

This enables programmatic simplification and analysis of even complex resistor networks.

---

## 2. Algorithm Overview

We simplify the circuit using two rules:
1. **Series combination**: If a node has exactly two neighbors, the resistors can be merged.
2. **Parallel combination**: If multiple edges exist between two nodes, combine them via:
$$
\frac{1}{R_{\text{eq}}} = \sum_i \frac{1}{R_i}
$$

The process is repeated until a single edge (equivalent resistance) remains between the source and target.

---

## 3. Pseudocode

```
function simplify_graph(graph, source, target):
    repeat:
        for each node N in graph:
            if N is not source or target and degree(N) == 2:
                merge series resistors at N
        for each pair of nodes (A, B) with multiple edges:
            merge them in parallel
    until no further simplifications
    return weight of edge between source and target
```

---

## 4. Python Implementation

```python
import networkx as nx

def parallel_merge(G, u, v):
    resistances = [1 / G[u][v][key]['resistance'] for key in G[u][v]]
    R_eq = 1 / sum(resistances)
    G.remove_edges_from([(u, v, key) for key in list(G[u][v].keys())])
    G.add_edge(u, v, resistance=R_eq)

def series_merge(G, node):
    neighbors = list(G.neighbors(node))
    u, v = neighbors[0], neighbors[1]
    R1 = list(G[u][node].values())[0]['resistance']
    R2 = list(G[v][node].values())[0]['resistance']
    R_eq = R1 + R2
    G.remove_node(node)
    G.add_edge(u, v, resistance=R_eq)

def simplify_circuit(G, source, target):
    changed = True
    while changed:
        changed = False
        for node in list(G.nodes):
            if node not in (source, target) and G.degree[node] == 2:
                series_merge(G, node)
                changed = True
                break
        for u, v in list(G.edges()):
            if G.number_of_edges(u, v) > 1:
                parallel_merge(G, u, v)
                changed = True
                break
    return G[source][target]['resistance']

# Example usage
G = nx.MultiGraph()
G.add_edge('A', 'B', resistance=2)
G.add_edge('B', 'C', resistance=3)
G.add_edge('C', 'D', resistance=4)
G.add_edge('A', 'D', resistance=6)  # Parallel with path A-B-C-D

result = simplify_circuit(G, 'A', 'D')
print(f"Equivalent Resistance between A and D: {result} ohms")
```

---

## 5. Example Circuit

1. **Series**: A–B–C with resistors 2Ω, 3Ω
   $$ R_{\text{eq}} = 2 + 3 = 5\ \Omega $$
2. **Parallel**: Two 6Ω resistors
   $$ R_{\text{eq}} = \left(\frac{1}{6} + \frac{1}{6}\right)^{-1} = 3\ \Omega $$

---

## 6. Efficiency & Improvements

- Current implementation uses **repeated iteration**, which can be optimized.
- **DFS** or **Union-Find** structures can speed up recognition of connected components and cycles.
- Can extend to support **voltage source modeling**, **current flow**, and **nonlinear elements**.

---

## 7. Extensions

- Add **visualization** using `matplotlib` and `networkx.draw()`.
- Export to SPICE-compatible netlists.
- Integrate with Falstad or simulation environments.

