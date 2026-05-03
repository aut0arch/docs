---
title: Token Cost Analysis
layout: default
nav_order: 5
---

# Token Cost Analysis

This page estimates the API cost of running the `aut0arch` explainer module across popular LLM providers and models.

## Baseline: Observed Token Usage

In a benchmark run using `JacMattisen/board-java` (15 clusters), the explainer consumed approximately **17,000 tokens** per run (based on empirical observation with `openai/gpt-oss-20b`).

The token count is split roughly as:
- **Input**: ~12,000 tokens (system prompt + code context per cluster × 15 calls)
- **Output**: ~5,000 tokens (structured JSON per cluster × 15 calls)

> **Note:** Token counts scale approximately linearly with the number of clusters. The `TheAlgorithms/Java` repository produced 180 clusters, meaning costs would be ~12× higher than the 15-cluster estimate shown here.

---

## Cost Estimate per Run (17k tokens, 15 clusters)

Prices are sourced from official documentation as of **May 2026** and represent standard on-demand API rates. All prices in USD.

### 🟢 OpenAI

| Model | Type | Input ($/1M) | Output ($/1M) | Est. Cost (17k tokens) |
| :--- | :--- | :--- | :--- | :--- |
| GPT-4.1 | Balanced | $2.00 | $8.00 | ~$0.064 |
| GPT-4.1 mini | Fast | $0.40 | $1.60 | ~$0.013 |
| GPT-4o mini | Fast | $0.15 | $0.60 | ~$0.005 |
| o3 | Reasoning | $2.00 | $8.00 | ~$0.064 |
| o4-mini | Fast Reasoning | $1.10 | $4.40 | ~$0.035 |

### 🔵 Google Gemini

| Model | Type | Input ($/1M) | Output ($/1M) | Est. Cost (17k tokens) |
| :--- | :--- | :--- | :--- | :--- |
| Gemini 2.5 Pro | Flagship | $1.25 | $10.00 | ~$0.065 |
| Gemini 2.5 Flash | Balanced | $0.30 | $2.50 | ~$0.016 |
| Gemini 2.5 Flash-Lite | Fast/Cheap | $0.10 | $0.40 | ~$0.003 |
| Gemini Flash 1.5 | Legacy Fast | $0.075 | $0.30 | ~$0.002 |

> Gemini 2.5 Flash-Lite is among the cheapest capable models available.

### 🟠 Anthropic Claude

| Model | Type | Input ($/1M) | Output ($/1M) | Est. Cost (17k tokens) |
| :--- | :--- | :--- | :--- | :--- |
| Claude Opus 4.7 | Flagship | $5.00 | $25.00 | ~$0.185 |
| Claude Sonnet 4.6 | Balanced | $3.00 | $15.00 | ~$0.111 |
| Claude Haiku 4.5 | Fast | $1.00 | $5.00 | ~$0.037 |

### ⚫ xAI Grok

| Model | Type | Input ($/1M) | Output ($/1M) | Est. Cost (17k tokens) |
| :--- | :--- | :--- | :--- | :--- |
| Grok 3 | Flagship | $3.00 | $15.00 | ~$0.111 |
| Grok 3 Mini | Fast Reasoning | $0.30 | $0.50 | ~$0.006 |

### 🔴 DeepSeek

| Model | Type | Input ($/1M) | Output ($/1M) | Est. Cost (17k tokens) |
| :--- | :--- | :--- | :--- | :--- |
| DeepSeek V3 | Balanced | $0.28 | $0.42 | ~$0.005 |
| DeepSeek R1 | Reasoning | $0.55 | $2.19 | ~$0.017 |

---

## Combined Comparison (Sorted by Estimated Cost)

| Model | Provider | Est. Cost (15 clusters) | Est. Cost (180 clusters) |
| :--- | :--- | :--- | :--- |
| `Gemini Flash 1.5` | Google | ~$0.002 | ~$0.024 |
| `Gemini 2.5 Flash-Lite` | Google | ~$0.003 | ~$0.036 |
| `GPT-4o mini` | OpenAI | ~$0.005 | ~$0.060 |
| `DeepSeek V3` | DeepSeek | ~$0.005 | ~$0.060 |
| `Grok 3 Mini` | xAI | ~$0.006 | ~$0.084 |
| `GPT-4.1 mini` | OpenAI | ~$0.013 | ~$0.156 |
| `Gemini 2.5 Flash` | Google | ~$0.016 | ~$0.192 |
| `DeepSeek R1` | DeepSeek | ~$0.017 | ~$0.204 |
| `o4-mini` | OpenAI | ~$0.035 | ~$0.420 |
| `Claude Haiku 4.5` | Anthropic | ~$0.037 | ~$0.444 |
| `GPT-4.1` | OpenAI | ~$0.064 | ~$0.768 |
| `o3` | OpenAI | ~$0.064 | ~$0.768 |
| `Gemini 2.5 Pro` | Google | ~$0.065 | ~$0.780 |
| `Grok 3` | xAI | ~$0.111 | ~$1.33 |
| `Claude Sonnet 4.6` | Anthropic | ~$0.111 | ~$1.33 |
| `Claude Opus 4.7` | Anthropic | ~$0.185 | ~$2.22 |

---

## Methodology & Notes

- **Token split assumption**: 70% input (~12k tokens), 30% output (~5k tokens).
- **Scaling**: Costs scale linearly with the number of clusters. Small repos (15 clusters) cost very little; large monorepos (180+ clusters) can cost ~12× more.
- **Reasoning tokens**: Models like `o3`, `o4-mini`, and `DeepSeek R1` use internal "thinking" tokens which are billed as output — actual costs could be higher than estimated.
- **Caching**: All providers offer 50–90% discounts for cached/repeated context. Since aut0arch uses an on-disk LLM cache, repeated calls for the same code are free locally — API costs are only incurred on the first analysis of a new repo.
- **Free tiers**: OpenRouter free models (`step-3.5-flash`, `gpt-oss-20b`) were used in our benchmarks at $0 cost, making them ideal for testing.

> Prices sourced from official provider documentation (OpenAI, Google AI Dev, Anthropic, xAI, DeepSeek). Always verify at each provider's official pricing page before production use.
