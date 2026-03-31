# LLM Token Optimization Playbook

## 🔥 Why Code Burns Tokens Faster Than Chat

Chat usage feels cheap because interaction is usually scoped, single-pass, and context-light. Code tools (Claude Code, Codex, GPT coding agents) consume more tokens because they repeatedly load code context, reason over it, and re-run execution loops.

**Token usage scales with Context Size × Iteration Count.**

| Mode | Token Usage | Why |
|------|------------|-----|
| Chat | Low | Scoped, minimal context |
| Code | High | Full file context + iteration loops |
| Agents | Extreme | Recursive multi-step execution |

**LLMs are cheap for thinking, but expensive for execution.**

## 🧠 Core Mental Model

- **Context size drives base cost**: every extra file, log block, and instruction adds fixed token load.
- **Iteration loops multiply cost**: retrying, revalidating, and re-running workflows compound usage.
- **Hidden overhead is real**: system prompts, tool wrappers, and orchestration metadata quietly consume budget.

**Token usage does not scale linearly — it compounds.**

## 💸 How Tokens Actually Accumulate

Example: one realistic Claude Code step on a medium codebase.

- Load large file(s) and adjacent context: `15k-30k` tokens
- Reasoning and planning pass: `5k-10k` tokens
- Edit + validation + follow-up checks: `5k-10k` tokens

**Total per step: `25k-50k` tokens**

What this means in practice:

- 4 iterations can consume `100k-200k` tokens
- 5-6 iterations can consume `125k-250k+` tokens
- Adding broad context each retry pushes usage higher than expected

**You don’t hit limits because of usage — you hit limits because of repetition.**

## 📊 Real Optimization Example

| Scenario | Tokens Used |
|----------|------------|
| Full repo edit (naive) | ~200k |
| Scoped file edit | ~40k |
| Chat -> plan -> targeted edit | ~15k |

Result: practical workflows commonly achieve **5-10x efficiency gains** by reducing context and retries.

## ⚙️ Decision Framework: Which Tool to Use

Use the tool that matches the phase of work.

| Task Type | Best Tool | Rule |
|---|---|---|
| Problem framing, architecture tradeoffs, decomposition | Chat | Keep it conceptual and context-light |
| Targeted implementation, precise patching, test fixes | Code tools | Pass only required files/functions |
| Repeatable transformations, bulk validation, deterministic runs | CLI/scripts | Automate and reuse outputs |

Operational rules:

- Use **chat** to decide what to do.
- Use **code tools** to do exactly that scoped change.
- Use **CLI/scripts** when work is repetitive or pipeline-based.
- **NEVER use code tools for exploration** when a short planning turn in chat would suffice.

## 🧠 Recommended Workflow

1. Plan in chat.
2. Execute in code tools.
3. Avoid mixing both in the same loop unless strictly necessary.

Why this works:

- Planning in chat minimizes exploratory token burn.
- Execution in code tools focuses spend on deterministic output.
- Clear handoff reduces retries and context drift.

**This separation alone can reduce token usage by 5-10x.**

## 📦 Context Control Techniques

Actionable controls:

- Trim large files before sending context.
- Include only relevant functions/classes/sections.
- Avoid full-repo ingestion unless absolutely required.
- Summarize upstream context before passing it downstream.

Bad vs good patterns:

| Pattern | Bad | Good |
|---|---|---|
| Code context | "Analyze entire repo and fix bug" | "Analyze `auth/session.ts` and `auth/cache.ts` only" |
| Prompt scope | "Refactor this module" | "Update `parseToken()` to handle empty header; no other changes" |
| Handoff | Paste full logs + full file + full history | Provide 10-line summary + exact failing test + target function |

## 🔄 Pipeline Optimization

Do not rerun full systems for local changes. Split workflows into stages and re-execute only affected stages.

Example pipeline (`M1-M4`):

- `M1` Extraction
- `M2` Classification
- `M3` Field extraction
- `M4` Publishing

If a bug is in `M3`, rerun `M3-M4`, not `M1-M4`.

**Only rerun the stage that changed.**

## ✍️ Prompt Efficiency Patterns

Bad prompt:

```text
Improve this code.
```

Good prompt:

```text
Update function X to handle edge case Y. Do not modify other parts.
```

Impact:

- Tight prompts reduce ambiguity.
- Lower ambiguity reduces retries.
- Fewer retries directly reduce token waste.

## ❌ Common Token Waste Patterns

- Dumping entire repo into context for a localized change.
- Re-running full pipelines instead of stage-level reruns.
- Vague prompts that force clarification loops.
- Using code tools for thinking/exploration.
- Uncontrolled agent loops with no stop boundaries.

## 🚨 Signs You Are About to Hit Limits

- Responses are getting slower per turn.
- The same task requires repeated retries.
- Context payloads are large and growing each step.
- You are re-running identical workflows frequently.

## 🤖 Agent Token Explosion

Why it happens:

- Recursive calls stack context across steps.
- Multi-step workflows add orchestration overhead.
- Memory accumulation keeps expanding prompt payload.

Fixes:

- Enforce step boundaries with explicit stop conditions.
- Cache intermediate outputs and reuse them.
- Limit recursion depth and retry count.
- Reset or summarize memory between major phases.

## 📌 Operating Rules

1. Never send more context than needed.
2. Never rerun full workflows unnecessarily.
3. Separate planning from execution.
4. Scope prompts tightly.
5. Cache intermediate results.
