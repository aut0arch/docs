---
layout: default
title: Test Cases
nav_order: 3
---

# Test Cases for `aut0arch` system

This document outlines the test cases for the 5 core components (repositories) of the `aut0arch` system: `server`, `parser`, `clustering`, `explainer`, and `app`.

---

## 1. `server` (Flask Orchestrator)

### Unit Tests
- **TC-SERVER-01:** **Valid URL Analysis Request.** Send a POST request to `/analyze` with a valid `url` payload (`{"url": "https://github.com/user/valid-repo.git"}`). **Expected:** Returns HTTP 200 with `{ "status": "success", "message": "Analysis complete" }` if pipeline succeeds.
- **TC-SERVER-02:** **Missing URL Payload.** Send a POST request to `/analyze` without the `url` field. **Expected:** Returns HTTP 400 with `{ "error": "No URL provided" }`.
- **TC-SERVER-03:** **Invalid URL Format.** Send a POST request to `/analyze` with a malformed URL (e.g., `invalid_url`). **Expected:** Returns HTTP 400 with `{ "error": "Invalid URL" }`.
- **TC-SERVER-04:** **Pipeline Failure Handling.** Mock the `run_pipeline` function to fail. Send a valid POST request to `/analyze`. **Expected:** Returns HTTP 500 with `{ "status": "error", "message": "Analysis failed" }`.

### Integration Tests
- **TC-SERVER-05:** **End-to-End Analysis Workflow.** Send a valid URL for a small, known Java repository. **Expected:** Server correctly triggers parser, clustering, and explainer sequentially. Expected output files (`graph.json`, `clustered_structure.json`, `clustering_data.json`) are generated in their respective locations, and temporary clone directories are properly cleaned up.

---

## 2. `parser` (Java AST Analyzer)

### Unit Tests
- **TC-PARSER-01:** **File Discovery.** Run the parser on a directory of mock Java files. **Expected:** Identifies all `.java` files while ignoring non-Java files.
- **TC-PARSER-02:** **Class Definition Parsing.** Feed a single Java file with multiple class definitions to the tree-sitter logic. **Expected:** Extracts the correct class names, interfaces, and methods structure.
- **TC-PARSER-03:** **Method Call Linking (Intra-file).** Feed a Java file where `MethodA` calls `MethodB`. **Expected:** Identifies the edge representing the call from `MethodA` to `MethodB`.
- **TC-PARSER-04:** **Method Call Linking (Inter-file).** Feed two Java files where a class in File 1 calls a method in File 2. **Expected:** Cross-file edge is correctly established in the dependency graph.

### Integration Tests
- **TC-PARSER-05:** **Graph Output Validation.** Run parser on a test directory. **Expected:** Exports a valid `graph.json` file including correct `nodes` and `edges` lists.

---

## 3. `clustering` (Architecture Abstractor)

### Unit Tests
- **TC-CLUSTER-01:** **Valid Graph Loading.** Run clustering on a completely well-formed `graph.json`. **Expected:** Successfully reads the JSON payload and builds internal graph representation.
- **TC-CLUSTER-02:** **Missing or Substandard Graph File.** Run the clustering logic against a non-existent `graph.json` or an empty JSON structure. **Expected:** Exits gracefully with a proper log/error message.
- **TC-CLUSTER-03:** **Modularity Grouping Algorithm.** Test the clustering algorithm against a mock `graph.json` containing 2 distinctly isolated connected components. **Expected:** Accurately produces exactly 2 distinct clusters mapping to each component.

### Integration Tests
- **TC-CLUSTER-04:** **Clustered Output Verification.** Feed a known baseline `graph.json`. Run clustering. **Expected:** Outputs `clustered_structure.json` featuring correctly populated lists of `clusters` (with `id` and `files` keys) and `edges` between those clusters.

---

## 4. `explainer` (Ollama/LLM Integration)

### Unit Tests
- **TC-EXPLAIN-01:** **File Content Loading.** Invoke `read_file_content` using a valid relative file path defined in a cluster. **Expected:** Retrieves the exact underlying file string contents correctly.
- **TC-EXPLAIN-02:** **Missing File Handling.** Invoke `read_file_content` with a file path that does not exist. **Expected:** Returns an error string indication smoothly (e.g., `[File not found: ...]`) without crashing the explainer app.
- **TC-EXPLAIN-03:** **LLM API Response parsing.** Mock an Ollama local API response matching the required exact JSON structure. **Expected:** The `generate_cluster_info` parses and deserializes the JSON efficiently into Python dictionary format.
- **TC-EXPLAIN-04:** **Fallback Behavior on LLM Failure.** Mock the API to throw an HTTP timeout or invalid response structure. **Expected:** Gracefully utilizes the fallback response (`"role": "Functionality could not be determined."`) without interrupting the rest of the flow.

### Integration Tests
- **TC-EXPLAIN-05:** **Full Explanation Write-Cycle.** Supply a valid `clustered_structure.json` alongside valid mock code files. **Expected:** The final expected output file `app/public/clustering_data.json` is generated successfully, containing the `system_story`, `pipeline`, and `flow` structures expected by the frontend.

---

## 5. `app` (React Frontend Visualizer)

### Unit Tests
- **TC-APP-01:** **Missing Data State.** Load the application without `clustering_data.json` available or when the data inside is empty. **Expected:** The UI displays an appropriate loading state, "Upload URL" prompt, or a friendly graphic rather than crashing.
- **TC-APP-02:** **Node Rendering.** Provide mock `clustering_data.json` containing 3 valid modular pipeline items. **Expected:** Checks that the Cytoscape canvas correctly mounts exactly 3 graph nodes.
- **TC-APP-03:** **Side Panel Populating.** Simulate a click metric on a visualized node. **Expected:** Exposes and slides in the side panel populating accurate `title`, `role`, `inputs`, `outputs`, and `code_evidence` mapped to the interacted node ID.

### Integration Tests
- **TC-APP-04:** **End-to-End Orchestration API Call.** Provide an interface interaction invoking "Analyze" with a repo URL. **Expected:** Sends correct POST payload to `:5000/analyze`, displaying loading UI loops, then properly rehydrates mapping visually upon polling successful response statuses.
