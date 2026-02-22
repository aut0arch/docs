---
title: Overview
parent: Architecture
nav_order: 1
---

# System Architecture Overview

`aut0arch` uses a modular pipeline system designed to break down the monolithic task of full-stack reverse-engineering into manageable, focused steps. Each part of the pipeline executes iteratively and feeds data forward into the next component.

## Core Modules Strategy

The `aut0arch` system adopts a pipelined Microservices Strategy, ensuring no single application is overly complicated. Instead, each standalone process implements a single step in the overarching reverse-engineering logic:

1. **Information Extraction (`parser`)**
    Extracting syntactic data strictly from the code using an Abstract Syntax Tree (AST).
2. **Structural Inference (`clustering`)**
    Grouping the extracted data mathematically based on connection strength to infer higher-level structures.
3. **Semantic Analysis (`explainer`)**
    Examining the groups structurally created and determining their purpose using an AI language model context window.
4. **Information Synthesis (`app`)**
    Rendering a comprehensive interactive visual artifact representing the combined extracted structure and AI analysis.
5. **Coordination (`server`)**
    Ensuring the entire pipeline triggers safely, coordinates errors, and correctly connects inputs to outputs without human intervention.

Each module lives in its own repository and is decoupled. As long as `parser` produces a standardized `graph.json` interface, `clustering` will execute flawlessly without needing to understand the underlying language. This modularity enables future tools (e.g., a Python parser or a Go parser) to be hooked directly into `aut0arch` with zero changes to the clustering, explainer, or frontend logic.
