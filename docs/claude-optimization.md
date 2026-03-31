# Claude Optimization

This section focuses on practical optimization patterns for Claude-style models.

## Prompt Design Guidelines

| Pattern | Why It Works | Example |
|---|---|---|
| Clear role + objective | Reduces ambiguity | `You are a compliance analyst. Summarize policy gaps.` |
| Explicit output schema | Controls output length | `Return table: Risk, Evidence, Severity, Action.` |
| Constraint-first prompting | Prevents drift | `Use only provided context; say unknown when missing.` |

## Compression Patterns

1. Convert narrative policy into rule blocks.
2. Use XML-like tags only when needed for separation.
3. Avoid duplicating instructions across user turns.
4. Remove meta text ("think carefully", "do your best") unless needed.

## Long Context Optimization

| Technique | Benefit | Tradeoff |
|---|---|---|
| Context ranking | Better relevance | Requires retrieval scoring |
| Chunk summaries | Lower tokens | Possible detail loss |
| Citation-only excerpts | Traceability + compactness | Needs source management |

## Output Control

Use deterministic formats to reduce unnecessary tokens:

```text
Output format:
- Decision: <approve|reject|escalate>
- Reason: <max 40 words>
- Evidence IDs: [id1, id2]
```

## Example Optimization

### Before

```text
Please read these 12 policy pages and provide a very detailed answer with all possible considerations and edge cases.
```

### After

```text
Task: policy decision support.
Use only chunks ranked top-5 by relevance.
Return:
1) Decision
2) Top 3 reasons
3) Missing data needed
Limit total response to 120 words.
```

## Evaluation Matrix

| Test Case | Baseline Tokens | Optimized Tokens | Quality Delta |
|---|---|---|---|
| Policy QA #1 | 2,900 | 1,540 | Equivalent |
| Policy QA #2 | 3,400 | 1,880 | Improved clarity |
| Policy QA #3 | 2,100 | 1,320 | Equivalent |
