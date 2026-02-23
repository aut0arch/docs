---
title: LLM Explainer
parent: Modules
nav_order: 4
---

# LLM Explainer

The `explainer` module operates at the boundary of semantic source code and human language.

## Role
Traditional reverse-engineering tools output massive dependency graphs that are impossible for humans to consume. `aut0arch` provides a higher-level abstraction. Once the `clustering` engine breaks the dependency graph into sub-systems, the `explainer` translates the code chunks back into human semantics.

It iterates through each generated cluster and collects the source code content of all encompassed files. It feeds this bulk text block directly to an AI language model alongside a finely-tuned system prompt asking for a high-level architectural overview. 

By default, the explainer uses Google Gemini (`gemini-flash-latest`), but it also natively supports running locally via Ollama (`gemma3:270m`) for users who need enhanced privacy or offline capabilities.

### Supported Models
- **Gemini (Cloud):** Fast, large context window. Requires \`GOOGLE_API_KEY\` in the \`.env\` file.
- **Ollama (Local):** Privacy-focused, runs entirely on your machine. Ensure an Ollama server is running locally (defaulting to \`http://localhost:11434\` via \`OLLAMA_HOST\`).

You can select the active backend during execution:
\`\`\`bash
# Run with Gemini (Default)
python explainer.py --model gemini

# Run with local Ollama
python explainer.py --model ollama
\`\`\`

## LLM Outputs
The \`explainer\` forces the selected AI model to respond with a strictly typed JSON structure containing:
* A descriptive English **title** for the module
* An English paragraph describing the semantic **role** of the code files
* Deduced **inputs** and **outputs** to the module
* Specific **code evidence** identifying interesting patterns found within the provided context window

This information is integrated directly with the raw generated graph structures and output into a unified \`clustering_data.json\` for the frontend.
