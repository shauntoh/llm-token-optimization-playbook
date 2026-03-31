# Why LLM Code Tools Burn Your Limits (and How to Fix It)

## Opening Hook

You start in chat, ask a few questions, and everything feels cheap and fast.

Then you switch to Claude Code, Codex, or a coding agent. Suddenly, usage spikes, responses slow down, and limits appear much earlier than expected.

That jump feels confusing at first because your workload did not feel 10x bigger. But under the hood, token economics changed completely.

## Section 1 — The Real Reason You’re Hitting Limits

The core model is simple:

**Token usage = Context Size x Iteration Count**

- **Context Size** is how much text the model must process each step.
- **Iteration Count** is how many times you repeat that process.

In code workflows, both numbers tend to grow at the same time. That is why limits arrive faster than intuition suggests.

## Section 2 — Chat vs Code (The Big Difference)

Chat is usually a scoped, single-pass interaction. Code tools are iterative systems with heavier context and repeated validation loops.

| Mode | Token Usage | Why |
|------|------------|-----|
| Chat | Low | Scoped, single-pass |
| Code | High | Context + iteration |
| Agents | Extreme | Recursive workflows |

**LLMs are cheap for thinking, but expensive for execution.**

## Section 3 — What’s Actually Burning Your Tokens

Most token burn comes from a few repeatable mechanics:

- Large file or multi-file loading
- Repeated plan-edit-validate cycles
- Hidden overhead (system/tool prompts, wrappers)
- Retries caused by unclear scope

A realistic coding step often looks like this:

- Load files/context: `15k-30k` tokens
- Reasoning/planning: `5k-10k` tokens
- Edit + validation: `5k-10k` tokens

Total per step: **`25k-50k` tokens**.

Run that loop 4-6 times and you are already in **`100k-250k+`** territory.

## Section 4 — The Biggest Mistakes People Make

- Dumping the full repository into context for a local change
- Using vague prompts that force retries
- Doing exploratory thinking inside code tools
- Re-running entire pipelines for one-stage edits
- Letting agents loop without strict boundaries

These are workflow problems, not model problems.

## Section 5 — The Fix (Practical Playbook)

### 1. Separate thinking from execution

Do architecture, decomposition, and tradeoff analysis in chat. Move to code tools only when the task is concrete.

### 2. Control context aggressively

Pass only the files, functions, and logs required for the current step. Summarize everything else.

### 3. Reduce iterations

Use precise prompts and clear acceptance criteria so the first pass is closer to done.

### 4. Use the right tool

- Chat for planning
- Code tools for precise implementation
- CLI/scripts for repeatable transformations

## Section 6 — A Better Workflow (VERY IMPORTANT)

Use this sequence consistently:

1. Plan in chat
2. Execute in code tools
3. Avoid unnecessary loops

Why it works:

- Planning remains low-cost and high-clarity
- Execution stays scoped and deterministic
- Fewer retries means less token compounding

**This alone can reduce usage by 5-10x.**

## Section 7 — Real Example (Before vs After)

| Scenario | Tokens Used |
|----------|------------|
| Naive approach | ~200k |
| Optimized approach | ~15k-40k |

Typical difference:

- Naive: full repo context + broad prompts + repeated reruns
- Optimized: targeted files + explicit task boundaries + stage-level reruns

## Section 8 — Warning Signs You’re About to Hit Limits

Watch for these early signals:

- Responses are getting slower each turn
- You are retrying the same task repeatedly
- Inputs are getting larger every step
- You keep rerunning near-identical workflows

If you see two or more signals, tighten scope immediately.

## Section 9 — If You’re Using Agents (Advanced)

Agents can multiply token usage quickly because they combine recursion, orchestration, and memory growth.

Main pressure points:

- Recursive calls stack context each step
- Multi-step flows add overhead per handoff
- Persistent memory increases prompt size over time

Practical fixes:

- Define hard step boundaries and stop conditions
- Cache intermediate outputs and reuse them
- Limit recursion depth and retry count
- Reset or summarize memory between phases

## Claude-Specific Optimization (Advanced)

### 1. CLAUDE.md usage

`CLAUDE.md` centralizes stable instructions so you do not repeat long policy/setup prompts every turn.

Token impact:

- Reduces repeated instruction payload
- Lowers per-turn overhead in long sessions
- Improves consistency, which reduces retries

Keep it concise. If `CLAUDE.md` becomes bloated, token savings disappear.

### 2. Skills

Skills package reusable capabilities into structured, repeatable patterns.

Token impact:

- Shrinks prompt size by replacing repeated instructions
- Improves task precision, which lowers back-and-forth retries
- Standardizes outputs for easier downstream handling

### 3. MCP (Model Context Protocol)

MCP lets tools fetch precise external context on demand instead of pasting large blocks into prompts.

Token impact:

- Replaces broad context loading with targeted retrieval
- Reduces context window pressure significantly
- Keeps prompts focused on decision-relevant information

| Technique | What It Optimizes |
|----------|------------------|
| CLAUDE.md | Instruction overhead |
| Skills | Prompt repetition |
| MCP | Context size |

## Section 10 — Operating Rules (Strong Ending)

1. Never send more context than needed.
2. Never rerun full workflows unnecessarily.
3. Separate planning from execution.
4. Scope prompts tightly.
5. Cache intermediate results.

## Closing

LLM usage isn’t about how much you use — it’s about how efficiently you structure the work.
