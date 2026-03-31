# Token Fundamentals

Token fundamentals determine cost, latency, and context quality.

## What a Token Represents

A token is a model-specific chunk of text. Short words may be one token; longer words or punctuation patterns may split into multiple tokens.

| Input Type | Token Behavior | Optimization Hint |
|---|---|---|
| Natural language instructions | Moderate token density | Remove redundant phrasing |
| JSON / code blocks | Higher token density | Keep schemas tight |
| Long pasted documents | Very high token load | Chunk + retrieve selectively |

## Cost and Latency Mechanics

| Driver | Cost Impact | Latency Impact | Control Lever |
|---|---|---|---|
| Input tokens | Direct increase | Increases preprocessing time | Prompt compression, RAG filtering |
| Output tokens | Direct increase | Increases generation time | Output caps, structured formats |
| Multi-turn history | Cumulative increase | Slower each turn | Rolling summaries |

## Prompt Compression Techniques

1. Remove repetitive policy statements.
2. Convert prose requirements into compact bullet rules.
3. Replace long examples with one canonical example.
4. Use placeholders for repeated entities.
5. Move stable instructions into system-level templates.

## Context Window Strategy

| Strategy | Description | Best Use Case |
|---|---|---|
| Sliding window | Keep last N turns only | Chat assistants |
| Summarized memory | Store abstracted history | Long-running workflows |
| Retrieval-first context | Inject only relevant chunks | Knowledge assistants |

## Example: Before vs After

### Before (Verbose)

```text
Please analyze the following customer support conversation in detail and provide a complete analysis that includes sentiment, issue category, urgency level, probable root cause, and recommended next best action. Ensure your response is comprehensive, and do not miss details.
```

### After (Compressed)

```text
Analyze support conversation. Return JSON:
{sentiment, category, urgency, root_cause, next_action}.
Prioritize correctness over verbosity.
```

## Measurement Checklist

- Track input/output tokens per endpoint.
- Track token distribution by user workflow.
- Compare cost per successful outcome, not raw request cost only.
- Validate quality with fixed eval sets before deployment.
