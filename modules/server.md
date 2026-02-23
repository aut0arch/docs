---
title: Server Orchestrator
parent: Modules
nav_order: 1
---

# Server Orchestrator

The `server` module is a lightweight Python Flask backend that acts as the traffic controller for the `aut0arch` system.

## Role
Its primary responsibility is to accept an analysis request from the frontend application, clone the target repository, and sequentially trigger the rest of the pipeline tools (`parser` -> `clustering` -> `explainer`) in the correct order.

## API Endpoints
The server currently exposes a single REST endpoint:
* `POST /analyze`
  * **Payload:** `{ "url": "https://github.com/user/repo.git", "model": "gemini" }` (Note: `model` is optional, defaults to `"gemini"`, but can be set to `"ollama"`)
  * **Response:** `{ "status": "success", "message": "Analysis complete" }`

## Error Handling & Logging
The orchestrator captures standard output and standard error from the subprocesses it triggers. In the event of a failure, it returns a 500 error to the frontend and writes a detailed traceback to `orchestrator_errors.log` (and via standard Python logging) to aid in debugging.
