# LLM Explainer

The `explainer` module operates at the boundary of semantic source code and human language.

## Role
Traditional reverse-engineering tools output massive dependency graphs that are impossible for humans to consume. `aut0arch` provides a higher-level abstraction. Once the `clustering` engine breaks the dependency graph into sub-systems, the `explainer` translates the code chunks back into human semantics.

It iterates through each generated cluster and collects the source code content of all encompassed files. It feeds this bulk text block directly to a Google Gemini model (`gemini-flash-latest`) alongside a finely-tuned system prompt asking for a high-level architectural overview. 

## LLM Outputs
The `explainer` forces the Gemini API to respond with a strictly typed JSON structure containing:
* A descriptive English **title** for the module
* An English paragraph describing the semantic **role** of the code files
* Deduced **inputs** and **outputs** to the module
* Specific **code evidence** identifying interesting patterns found within the provided context window

This information is integrated directly with the raw generated graph structures and output into a unified `clustering_data.json` for the frontend.
