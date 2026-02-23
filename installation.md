---
title: Installation (Docker)
layout: default
nav_order: 2
---

# Installation & Setup via Docker

Aut0arch is deeply distributed and requires multiple native dependencies, including C compilers for AST parsing. Consequently, we highly recommend running the platform via **Docker Compose**.

## Prerequisites

1.  **Docker Desktop / Engine:** Ensure [Docker](https://docs.docker.com/get-docker/) is installed on your machine.
2.  **Docker Compose:** Ensure [Docker Compose](https://docs.docker.com/compose/install/) is installed.

## Step 1: Clone the Repository Configuration

Clone the root repository that contains all the sub-modules (`app`, `server`, `parser`, `clustering`, `explainer`).

```bash
git clone https://github.com/aut0arch/aut0arch.git
cd aut0arch
```

*(Ensure all submodules pull correctly if configured as git submodules).*

## Step 2: Configure Environment Variables

The Explainer module uses Generative AI to understand the code. If you are using Google Gemini, you need to provide an API key.

1.  Navigate to `explainer/`
2.  Create a `.env` file (`cp .env.example .env` if it exists)
3.  Add your API key: `GOOGLE_API_KEY="your-api-key"`

*If you are using the local Ollama option, you must ensure the Ollama API is accessible from the Docker network.*

## Step 3: Launch Docker Compose

From the **root directory** of the repository (where `docker-compose.yml` is located), simply run:

```bash
docker-compose up --build -d
```

This will build two containers:
1.  **Backend:** A Python 3.10 environment that installs all dependencies for the server, parser, clustering, and explainer.
2.  **Frontend:** A Node.js 20 environment that runs the Vite development server.

## Step 4: Access the Application

Once the containers are successfully running, the application will be exposed on your local network:

*   **Frontend UI:** Navigate to [http://localhost:5173](http://localhost:5173) in your web browser.
*   **Backend API:** The core orchestrator API is running at `http://localhost:5000`.

## Stopping the Application

To shut down the environments and free up ports, run:

```bash
docker-compose down
```
