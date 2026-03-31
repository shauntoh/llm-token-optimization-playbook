# LLM Token Optimization Playbook

A practical playbook for reducing token cost and latency while improving answer quality across prompt-engineered and agentic AI systems.

## Why Code Burns Tokens Faster Than Chat

Chat feels cheap because each turn is usually scoped to a narrow question with minimal context. Code tools are expensive because they repeatedly process larger context (files, diffs, logs, instructions) across iterative loops.

**Token usage scales with `Context Size × Iteration Count`.**

| Mode | Token Usage | Why |
|---|---|---|
| Chat | Low | Scoped, minimal context |
| Code | High | Full file context + iteration loops |
| Agents | Extreme | Recursive multi-step execution |

**Takeaway:** LLMs are cheap for thinking, but expensive for execution.

## Executive Summary

Token optimization is high leverage for production LLM systems.

| Objective | Why It Matters | Typical Impact |
|---|---|---|
| Reduce input tokens | Lowers request cost and latency | 20-60% prompt reduction |
| Reduce output tokens | Improves response consistency and budget control | 15-40% output reduction |
| Improve context quality | Increases relevance and lowers hallucination risk | Higher answer precision |
| Standardize patterns | Enables repeatable quality across teams | Faster onboarding |

## Core Principles

1. Keep prompts intentional, not verbose.
2. Move static instructions to reusable system templates.
3. Retrieve only relevant context instead of full documents.
4. Constrain output format to prevent token sprawl.
5. Measure token usage continuously and optimize iteratively.

## Recommended Workflow

Use each mode for what it does best:

1. Use chat for planning and thinking.
2. Use code tools for execution and concrete edits.
3. Avoid mixing both unnecessarily in the same loop.

| Phase | Preferred Mode | Goal |
|---|---|---|
| Problem framing | Chat | Clarify requirements and constraints |
| Solution design | Chat | Compare approaches before execution |
| Implementation | Code tools | Apply deterministic changes |
| Verification | Code tools | Run tests, validate outputs |
| Optimization pass | Chat + targeted code run | Tighten prompts and workflow |

## Common Token Waste Patterns

Avoid these high-cost behaviors:

- Dumping entire repositories into context when only a few files are needed.
- Re-running full pipelines for small localized changes.
- Using vague prompts that trigger retries and rework loops.
- Using code tools for exploration that should happen in chat planning first.

## Quick Start

1. Baseline prompt, latency, and quality metrics.
2. Apply prompt compression and retrieval filtering.
3. Introduce strict output schemas and stopping criteria.
4. Re-test on representative eval cases.
5. Roll out behind flags and monitor regressions.

## Playbook Sections

- [Token Fundamentals](./docs/token-fundamentals.md)
- [Claude Optimization](./docs/claude-optimization.md)
- [Agent Optimization](./docs/agent-optimization.md)
- [Real-World Examples](./docs/real-world-examples.md)

## Suggested Repository Structure

```text
llm-token-optimization-playbook/
|-- README.md
`-- docs/
    |-- token-fundamentals.md
    |-- claude-optimization.md
    |-- agent-optimization.md
    `-- real-world-examples.md
```

## KPI Dashboard Template

| KPI | Definition | Target |
|---|---|---|
| Avg input tokens | Mean tokens sent per request | Downward trend |
| Avg output tokens | Mean tokens generated per request | Controlled variance |
| Cost per successful task | Total model cost / successful outcomes | 20%+ reduction |
| P95 latency | 95th percentile end-to-end time | Stable or improved |
| Task success rate | Pass rate on eval set | No regression |

## Contributing

- Add optimization techniques with benchmark evidence.
- Include before/after prompts with token deltas.
- Document constraints and quality tradeoffs.

## License

MIT (recommended; update as needed).
