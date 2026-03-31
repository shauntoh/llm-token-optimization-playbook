# Agent Optimization

Agentic systems multiply token usage through planning, tool calls, retries, and memory.

## Agent Token Cost Surfaces

| Surface | Common Waste Pattern | Optimization |
|---|---|---|
| Planner prompts | Overly verbose chain-of-thought scaffolding | Compact plan schema |
| Tool call wrappers | Repeating same tool instructions every turn | Centralize tool specs |
| Memory replay | Injecting full history | Summarize state + deltas |
| Retry loops | Full-context retries | Retry with minimal context |

## Agent Architecture Patterns

1. **Thin planner, strong tools**: keep planning short, offload work to deterministic tools.
2. **State compaction**: store compact state objects, not long narrative logs.
3. **Selective replay**: include only prior steps relevant to current objective.
4. **Guardrailed retries**: retry once with adjusted prompt, then escalate.

## Tool Usage Optimization

| Practice | Effect |
|---|---|
| Define strict tool schemas | Fewer malformed calls |
| Limit tool response payload | Lower follow-up token usage |
| Canonicalize tool outputs | Easier downstream parsing |

## Example Agent Loop (Optimized)

```text
1. Parse user objective into task_id + constraints.
2. Retrieve only required memory keys.
3. Run one tool call with narrow parameters.
4. Synthesize response in fixed template.
5. Store concise result summary (<= 60 tokens).
```

## Failure-Mode Controls

| Risk | Symptom | Mitigation |
|---|---|---|
| Context bloat | Costs grow each turn | Rolling state summaries |
| Tool overuse | Many unnecessary calls | Confidence threshold before tool invocation |
| Output drift | Inconsistent response structure | Enforce output schema |

## Agent KPI Recommendations

- Tokens per successful run
- Tool calls per successful run
- Retry rate
- Cost per task class
- Success rate at fixed latency budget
