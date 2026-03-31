# Real-World Examples

Practical before/after scenarios showing token reductions while preserving quality.

## Example 1: Customer Support Triage

| Metric | Before | After |
|---|---|---|
| Input tokens | 1,200 | 520 |
| Output tokens | 380 | 160 |
| Avg latency | 4.2s | 2.1s |
| Accuracy (eval) | 91% | 92% |

### What Changed

- Replaced long prose instruction with JSON schema.
- Injected only latest ticket + policy snippets.
- Added strict output constraints.

## Example 2: Contract Risk Review

| Metric | Before | After |
|---|---|---|
| Input tokens | 4,800 | 2,050 |
| Output tokens | 950 | 320 |
| Cost / review | 1.00x | 0.43x |
| Critical risk recall | 96% | 95% |

### What Changed

1. Ranked clauses by risk keywords before prompt injection.
2. Requested top-5 risks only, not exhaustive narrative.
3. Used short evidence references (clause IDs).

## Example 3: Agentic Research Workflow

| Metric | Before | After |
|---|---|---|
| Tokens / run | 9,400 | 4,100 |
| Tool calls / run | 14 | 7 |
| Retry rate | 28% | 11% |
| Task completion | 88% | 90% |

### What Changed

- Compacted planner prompts.
- Applied one-pass retrieval + one retry maximum.
- Stored compact memory snapshots instead of full transcripts.

## Reusable Optimization Template

```markdown
### Use Case
<Describe task>

### Baseline
- Input tokens:
- Output tokens:
- Latency:
- Quality metric:

### Optimizations Applied
1.
2.
3.

### Result
- Token delta:
- Cost delta:
- Quality delta:
- Key tradeoff:
```

## Rollout Recommendations

- Start with high-volume workflows first.
- Ship behind feature flags.
- Monitor quality and rollback thresholds.
- Standardize successful patterns in internal playbooks.
