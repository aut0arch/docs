# Clustering Engine

The `clustering` module takes the highly-granular semantic graph produced by the `parser` and abstracts it into cohesive architectural modules.

## Role
A typical dependency graph contains thousands of nodes. The `clustering` engine condenses these into logical boundaries (like a "Database Layer" or "Auth Service") that represent high-level subsystems.

It accomplishes this via community detection algorithms:
1. **Flattening:** It collapses all individual `methods` and `classes` up to their root host file. Connections between files act as weighted edges.
2. **Louvain Heuristics:** The `networkx` and `community-louvain` libraries are run against the weighted network graph to detect strong communities of interconnected files.

## Outputs
The subsystem boundaries and their interconnection weights are grouped and output as `clustered_structure.json`.
