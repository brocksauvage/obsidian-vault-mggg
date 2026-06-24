---
tags: [gerrychain, class, core, graph]
---

# Graph

**Module:** `gerrychain/graph/graph.py`
**Part of:** [[GerryChain Architecture]]

`Graph` is the **dual graph** of a geography: each node is a geographic unit (precinct, census block, county, …) carrying data attributes (population, votes, area), and each edge connects two **adjacent** units. Everything else in GerryChain is built on top of it — a [[Partition]] is a `Graph` plus an [[Assignment]].

## A wrapper over NetworkX *and* RustworkX

`Graph` encapsulates an underlying graph that may be either a [NetworkX](https://networkx.org/) graph (`_nx_graph`) or a [RustworkX](https://www.rustworkx.org/) graph (`_rx_graph`). It exposes a NetworkX-style interface so legacy code keeps working, while allowing the faster Rust-backed implementation for the heavy lifting.

- When a graph is added to a [[Partition]], the NetworkX graph is typically **converted to RustworkX** (`convert_from_nx_to_rx`) for speed, after which the NX graph becomes inaccessible.
- ID-translation helpers (`internal_node_id_for_original_nx_node_id`, `original_nx_node_id_for_internal_node_id`, `get_nx_to_rx_node_id_map`, …) keep the user-facing node IDs stable across the conversion.
- `is_nx_graph()` / `is_rx_graph()` report which backend is active.

> [!note] The class has no `__init__`
> Instances are built through `from_*` classmethods, not the constructor, because some external code relies on default-constructing the class. Always create a `Graph` via one of the loaders below.

## Loading data (classmethods)

| Method | Source |
|---|---|
| `from_file(path, ...)` | Shapefile / GeoJSON / GeoPackage — computes rook or queen adjacency. |
| `from_geodataframe(df, ...)` | An existing `geopandas` GeoDataFrame. |
| `from_json(file)` / `to_json(file)` | Node-link JSON (the common interchange format for dual graphs). |
| `from_networkx(nx_graph)` / `to_networkx_graph()` | Convert to/from NetworkX. |
| `from_rustworkx(rx_graph)` | Wrap an existing RustworkX graph. |

Adjacency (rook vs. queen) is computed in `gerrychain/graph/adjacency.py`; geometry reprojection/validation lives in `gerrychain/graph/geo.py` (raising `GeometryError` on bad geometries).

## Working with the graph

- **Structure:** `nodes`, `edges`, `node_indices`, `edge_indices`, `neighbors(n)`, `degree(n)`, `add_edge`, `subgraph(nodes)`.
- **Data:** `node_data(n)`, `edge_data(e)`, `add_data(df, columns)` (join a DataFrame of attributes), `join(...)`.
- **Traversal:** BFS helpers (`generic_bfs_edges`, `successors`, `predecessors`) used by tree algorithms.
- **Spectral / connectivity:** `laplacian_matrix()`, `normalized_laplacian_matrix()` (used by [[Spectral Proposals]]), `num_connected_components()`, `subgraphs_for_connected_components()`, `is_a_tree()`.
- **Diagnostics:** `islands` (disconnected single nodes), `warn_for_islands()`, `verify_graph_is_valid()`.

## See also
- [[FrozenGraph]] — the immutable, cached wrapper used inside a running chain
- [[Partition]] — a `Graph` + an [[Assignment]]
- [[Spectral Proposals]] — consumes the Laplacian matrices
- [[Tree Partitioning]] — spanning-tree algorithms over the graph
