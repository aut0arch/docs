---
title: Home
layout: home
nav_order: 1
---

# aut0arch Documentation

Welcome to the official documentation for **aut0arch**!

`aut0arch` is an automated architecture discovery and explanation pipeline. It is designed to take a repository of source code (currently focusing on Java) and automatically reverse-engineer its high-level architecture. It doesn't just draw diagrams; it leverages Large Language Models (LLMs) to intelligently explain *what* each component does, *why* it exists, and *how* it interacts with the rest of the system.

## Why aut0arch?

Understanding a new or legacy codebase is one of the most time-consuming tasks for software engineers. Traditional documentation is quickly outdated, and raw dependency graphs are often too granular to provide meaningful insight.

`aut0arch` solves this by:
1. **Parsing** the actual source code to build a ground-truth semantic graph.
2. **Clustering** those fine-grained details into high-level, cohesive modules.
3. **Explaining** those modules using AI, giving them human-readable roles and context.
4. **Visualizing** the result in an interactive web application.

## Repository Structure

The `aut0arch` ecosystem consists of 5 specialized microservices:

* **server**: The central orchestrator that manages the pipeline.
* **parser**: The engine that reads code and builds the raw dependency graph.
* **clustering**: The tool that abstracts the raw graph into architectural components.
* **explainer**: The AI-integration layer that writes human-readable descriptions.
* **app**: The React frontend that visualizes the final architecture.

To get started, please refer to the [Installation via Docker](./installation.md) guide.

To learn more about how they work together, head over to the [Architecture Overview](architecture/overview.md).
