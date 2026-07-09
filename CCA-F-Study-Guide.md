# Claude Certified Architect - Foundations (CCA-F) — Free Study Guide

A curated collection of free resources to prepare for the CCA-F certification exam.

---

## Overview

CCA-F validates foundational proficiency in:

| Domain | Topics |
|---|---|
| **API Fundamentals** | Messages API, authentication, error handling, rate limits, models, SDKs |
| **Tool Use** | Tool definition schema, tool choice, tool runner vs manual loop, handling results |
| **Prompt Engineering** | System prompts, multi-turn conversations, structured outputs, prompt caching |
| **Agents & Workflows** | Agent design patterns, tool surface design, context management, subagents |
| **Streaming** | Event types, SDK patterns, handling content block deltas |
| **Safety** | Stop reasons (refusal), content filtering, responsible AI |

---

## Official Documentation (platform.claude.com/docs)

The official docs are the primary study source — everything on the exam comes from here.

### Must-Read Pages

| Topic | What to Study |
|---|---|
| Messages API | Request/response format, system prompts, multi-turn, error handling |
| Models & Pricing | Model IDs, context windows, pricing per model |
| Tool Use | Tool definition schema, tool_choice options, handling tool results |
| Extended Thinking | Adaptive thinking, effort levels (low/medium/high/max) |
| Streaming | Event types, SDK patterns for stream handling |
| Prompt Caching | Cache control, placement patterns, TTL, verifying cache hits |
| Structured Outputs | JSON schema constraints, output_config.format, parse() |
| Vision | Image support (base64/URL), media types, size limits |
| PDF Support | PDF handling, limits, examples |
| Batch Processing | Batch API endpoints, request format, polling |
| Token Counting | count_tokens endpoint, cost estimation |
| Agent Design | Agent patterns, tool surface design, context editing, compaction |
| Code Execution | Sandbox limits, pre-installed libraries, container reuse |

---

## Interactive Labs & Hands-On Workshops

### 1. Anthropic Cookbook
**Repo:** [github.com/anthropics/claude-cookbooks](https://github.com/anthropics/claude-cookbooks)

Jupyter notebooks covering every major feature. Run them locally or in Google Colab.

| Directory / File | Topic |
|---|---|
| `tool_use/customer_service_agent.ipynb` | Building a customer service agent with tools |
| `tool_use/calculator_tool.ipynb` | Calculator tool integration |
| `patterns/agents/` | Agent design patterns |
| `multimodal/getting_started_with_vision.ipynb` | Vision/image handling |
| `multimodal/best_practices_for_vision.ipynb` | Vision best practices |
| `multimodal/reading_charts_graphs_powerpoints.ipynb` | Chart/graph interpretation |
| `extended_thinking/` | Extended thinking mode examples |
| `misc/prompt_caching.ipynb` | Prompt caching walkthrough |
| `misc/how_to_enable_json_mode.ipynb` | JSON mode / structured outputs |
| `misc/how_to_make_sql_queries.ipynb` | SQL query generation |
| `misc/building_evals.ipynb` | Building evaluations |
| `misc/building_moderation_filter.ipynb` | Content moderation |
| `misc/pdf_upload_summarization.ipynb` | PDF processing |
| `third_party/Pinecone/rag_using_pinecone.ipynb` | RAG with vector databases |
| `third_party/Wikipedia/wikipedia-search-cookbook.ipynb` | Wikipedia search integration |
| `managed_agents/` | Managed agents examples |
| `claude_agent_sdk/` | Agent SDK examples |
| `skills/` | Skills-based examples |
| `capabilities/` | Classification, RAG, summarization |
| `finetuning/` | Fine-tuning Claude |
| `observability/` | Observability integrations |

### 2. Prompt Engineering Interactive Tutorial
**Repo:** [github.com/anthropics/prompt-eng-interactive-tutorial](https://github.com/anthropics/prompt-eng-interactive-tutorial)

Structured 9-chapter course with exercises. Two versions available:
- `Anthropic 1P/` — for first-party API users
- `AmazonBedrock/` — for Bedrock users

| Chapter | Level | Topic |
|---|---|---|
| 1 | Beginner | Basic Prompt Structure |
| 2 | Beginner | Being Clear and Direct |
| 3 | Beginner | Assigning Roles |
| 4 | Intermediate | Separating Data from Instructions |
| 5 | Intermediate | Formatting Output & Speaking for Claude |
| 6 | Intermediate | Precognition (Thinking Step by Step) |
| 7 | Intermediate | Using Examples |
| 8 | Advanced | Avoiding Hallucinations |
| 9 | Advanced | Building Complex Prompts (Industry Use Cases) |
| Appendix | — | Chaining Prompts, Tool Use, Search & Retrieval |

### 3. Claude Agent SDK Quickstarts

| Language | Repo | Stars |
|---|---|---|
| Python | [github.com/anthropics/claude-agent-sdk-python](https://github.com/anthropics/claude-agent-sdk-python) | 7.6k |
| TypeScript | [github.com/anthropics/claude-agent-sdk-typescript](https://github.com/anthropics/claude-agent-sdk-typescript) | 1.6k |

Each ships with examples/ directory containing runnable programs.

### 4. Anthropic CLI (`ant`)
**Repo:** [github.com/anthropics/anthropic-cli](https://github.com/anthropics/anthropic-cli)

Free CLI tool for experimenting with the Claude API from the terminal. Every API resource is exposed as a subcommand. Great for quick testing without writing code.

### 5. SDK Examples (all languages)

Each SDK repo has an `examples/` directory with runnable code:

| SDK | Repo |
|---|---|
| Python | [github.com/anthropics/anthropic-sdk-python](https://github.com/anthropics/anthropic-sdk-python) |
| TypeScript | [github.com/anthropics/anthropic-sdk-typescript](https://github.com/anthropics/anthropic-sdk-typescript) |
| Java | [github.com/anthropics/anthropic-sdk-java](https://github.com/anthropics/anthropic-sdk-java) |
| Go | [github.com/anthropics/anthropic-sdk-go](https://github.com/anthropics/anthropic-sdk-go) |
| Ruby | [github.com/anthropics/anthropic-sdk-ruby](https://github.com/anthropics/anthropic-sdk-ruby) |
| C# | [github.com/anthropics/anthropic-sdk-csharp](https://github.com/anthropics/anthropic-sdk-csharp) |
| PHP | [github.com/anthropics/anthropic-sdk-php](https://github.com/anthropics/anthropic-sdk-php) |

---

## YouTube Channels

### Official Anthropic Channel
**Channel:** [youtube.com/@anthropic](https://youtube.com/@anthropic)

The official source for product demos, technical deep dives, and tutorials. Look for playlists titled "Building with Claude" and "Claude Fundamentals."

### Community & DevRel Content
Search YouTube for these terms to find additional tutorials:
- "Claude API tutorial"
- "Claude tool use walkthrough"
- "Building agents with Claude"
- "Claude prompt engineering"

---

## GitHub — All Relevant Anthropic Repos

| Repo | Stars | Description |
|---|---|---|
| [anthropics/claude-cookbooks](https://github.com/anthropics/claude-cookbooks) | 46.8k | Notebooks & recipes for Claude |
| [anthropics/prompt-eng-interactive-tutorial](https://github.com/anthropics/prompt-eng-interactive-tutorial) | 36.9k | Interactive prompt engineering course |
| [anthropics/claude-agent-sdk-python](https://github.com/anthropics/claude-agent-sdk-python) | 7.6k | Python agent SDK |
| [anthropics/claude-agent-sdk-typescript](https://github.com/anthropics/claude-agent-sdk-typescript) | 1.6k | TypeScript agent SDK |
| [anthropics/anthropic-cli](https://github.com/anthropics/anthropic-cli) | 566 | CLI for the Claude API |
| [anthropics/anthropic-sdk-python](https://github.com/anthropics/anthropic-sdk-python) | — | Python SDK with examples/ |
| [anthropics/anthropic-sdk-typescript](https://github.com/anthropics/anthropic-sdk-typescript) | — | TypeScript SDK with examples/ |

---

## Recommended Study Path

**Phase 1 — Fundamentals (Week 1)**
1. Read the Messages API docs on platform.claude.com
2. Work through the Prompt Engineering Tutorial (chapters 1-5)
3. Set up the `ant` CLI and make your first API calls

**Phase 2 — Core Features (Week 2)**
4. Study Tool Use docs + run the Cookbook's `customer_service_agent.ipynb`
5. Study Streaming docs + run streaming examples from the SDK
6. Study Prompt Caching docs + run the Cookbook caching notebook

**Phase 3 — Advanced Topics (Week 3)**
7. Study Agent Design patterns + run agent examples
8. Study Structured Outputs + run JSON mode notebook
9. Study Extended Thinking docs + run thinking examples

**Phase 4 — Review & Practice (Week 4)**
10. Re-read the docs for any weak areas
11. Build a small project combining tools, streaming, and caching
12. Watch the Anthropic YouTube channel for any certification-specific content

---

## Quick Reference — Key API Patterns

### Basic Request (Python)
```python
import anthropic
client = anthropic.Anthropic()

response = client.messages.create(
    model="claude-opus-4-8",
    max_tokens=16000,
    system="You are a helpful assistant.",
    messages=[{"role": "user", "content": "Hello"}]
)
```

### Tool Use
```python
response = client.messages.create(
    model="claude-opus-4-8",
    max_tokens=16000,
    tools=[{
        "name": "get_weather",
        "description": "Get weather for a location",
        "input_schema": {
            "type": "object",
            "properties": {
                "location": {"type": "string"}
            },
            "required": ["location"]
        }
    }],
    messages=[{"role": "user", "content": "What's the weather in Paris?"}]
)
```

### Streaming
```python
with client.messages.stream(
    model="claude-opus-4-8",
    max_tokens=16000,
    messages=[{"role": "user", "content": "Tell me a story"}]
) as stream:
    for text in stream.text_stream:
        print(text, end="")
```

### Adaptive Thinking
```python
response = client.messages.create(
    model="claude-opus-4-8",
    max_tokens=16000,
    thinking={"type": "adaptive"},
    output_config={"effort": "high"},
    messages=[{"role": "user", "content": "Solve this step by step..."}]
)
```

### Prompt Caching
```python
response = client.messages.create(
    model="claude-opus-4-8",
    max_tokens=16000,
    cache_control={"type": "ephemeral"},
    system=large_document_text,
    messages=[{"role": "user", "content": "Summarize this"}]
)
```

---

*All resources listed above are free and officially provided by Anthropic or hosted on their GitHub organization.*
