# Data Flow Pipeline

The system is currently operated sequentially under the `server` orchestrator. The typical execution data flow proceeds as follows:

```mermaid
graph TD;
    A[User Submits Repository URL via App] --> B(Orchestrator Clones Repo);
    B --> C(Parser);
    C -- "graph.json (Granular Tree)" --> D(Clustering Engine);
    D -- "clustered_structure.json" --> E(LLM Explainer);
    E -- "clustering_data.json (AI Enhanced)" --> F(React Frontend App);
    F --> G[User Explores Visual Graph];
```

## Input/Output Artifacts

1. **`graph.json`**
    * **Producer:** `parser.py`
    * **Consumer:** `run_clustering.py`
    * **Description:** A highly detailed collection of raw semantic references. Includes lists of nodes (classes, files, methods) and edges (containment, specific method calls).
2. **`clustered_structure.json`**
    * **Producer:** `clustering.py`
    * **Consumer:** `explainer.py`
    * **Description:** Represents grouped arrays of source code files (`cluster_0 = [file1, file2]`) and the aggregated weight of external links calling out between clusters.
3. **`clustering_data.json`**
    * **Producer:** `explainer.py`
    * **Consumer:** `app/public/`
    * **Description:** The ultimate human-readable end product. Consists of a global `system_story`, the pipeline array of individual AI-explained cluster definitions (title, role, inputs/outputs, interesting patterns), and a flow array denoting the mathematical connectivity.
