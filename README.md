# LLM Token Optimization Playbook

A practical playbook for reducing token cost and latency while improving answer quality across prompt-engineered and agentic AI systems.

## Executive Summary

Token optimization is one of the highest-leverage activities for production LLM systems.

| Objective | Why It Matters | Typical Impact |
|---|---|---|
| Reduce input tokens | Lowers request cost and latency | 20-60% prompt reduction |
| Reduce output tokens | Improves response consistency and budget control | 15-40% output reduction |
| Improve context quality | Increases relevance and lowers hallucination risk | Higher answer precision |
| Standardize patterns | Enables repeatable quality across teams | Faster onboarding |

### Core Principles

1. Keep prompts intentional, not verbose.
2. Move static instructions to reusable system templates.
3. Retrieve only relevant context (not entire documents).
4. Constrain output format to avoid unnecessary token sprawl.
5. Measure token usage continuously and optimize iteratively.

## Playbook Sections

- [Token Fundamentals](./docs/token-fundamentals.md)
- [Claude Optimization](./docs/claude-optimization.md)
- [Agent Optimization](./docs/agent-optimization.md)
- [Real-World Examples](./docs/real-world-examples.md)

## Quick Start

1. Baseline current prompts with token + latency metrics.
2. Apply structural prompt compression and retrieval filtering.
3. Introduce output schemas and stopping criteria.
4. Re-test quality with representative eval cases.
5. Roll changes behind feature flags and monitor impact.

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
| Avg input tokens | Mean tokens sent/request | Downward trend |
| Avg output tokens | Mean tokens generated/request | Controlled variance |
| Cost per successful task | Total model cost / successful outcomes | 20%+ reduction |
| P95 latency | 95th percentile end-to-end time | Stable or improved |
| Task success rate | Pass rate on eval set | No regression |

## Contributing

- Add optimization techniques with benchmark evidence.
- Include before/after prompts and token deltas.
- Document constraints and quality tradeoffs.

## License

MIT (recommended; update as needed).
