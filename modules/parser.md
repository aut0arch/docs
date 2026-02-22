---
title: Semantic Parser
parent: Modules
nav_order: 2
---

# Semantic Parser

The `parser` module is responsible for analyzing raw source code and constructing a highly granular semantic relationship graph.

## Role
Traditional line-based tools cannot accurately capture the nuances of an Object-Oriented codebase. By leveraging **Tree-sitter**, the `parser` builds an Abstract Syntax Tree (AST) that natively understands the target language (currently Java).

It operates in two passes:
1. **Pass 1 (Discovery):** Scans all files and recursively identifies `classes`, `interfaces`, and `methods`.
2. **Pass 2 (Linking):** Scans the AST for method invocations. Whenever `Method A` is seen calling `Method B`, it searches the index and creates a `CALLS` edge between them.

## Outputs
The tool generates a raw `graph.json` artifact containing thousands of `Nodes` and `Edges`. Optionally, it can export this structured data into a Graphviz `.dot` file for low-level visual validation.
