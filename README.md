# Why LLM Code Tools Burn Your Limits (and How to Fix It)

## Table of Contents

- [The Problem](#the-problem)
- [The Real Reason You’re Hitting Limits](#the-real-reason-youre-hitting-limits)
- [Chat vs Code](#chat-vs-code)
- [What’s Actually Burning Your Tokens](#whats-actually-burning-your-tokens)
- [The Biggest Mistakes People Make](#the-biggest-mistakes-people-make)
- [The Fix — Practical Playbook](#the-fix-practical-playbook)
- [A Better Workflow](#a-better-workflow)
- [Real Example (Before vs After)](#real-example-before-vs-after)
- [Context Control Techniques](#context-control-techniques)
- [Pipeline Optimization](#pipeline-optimization)
- [Prompt Efficiency Patterns](#prompt-efficiency-patterns)
- [Signs You’re About to Hit Limits](#signs-youre-about-to-hit-limits)
- [Agent Token Explosion](#agent-token-explosion)
- [Claude-Specific Optimization](#claude-specific-optimization)
- [Operating Rules](#operating-rules)
- [Final Principle](#final-principle)
- [About This Playbook](#about-this-playbook)

## The Problem

You start in chat, ask a few questions, and everything feels cheap.

Then you switch to Claude Code, Codex, or an agent workflow. Usage spikes, responses slow down, and limits appear faster than expected.

The work did not feel 10x bigger, but the token model changed.

## The Real Reason You’re Hitting Limits

**Token usage = Context Size x Iteration Count**

- **Context Size**: how much text the model processes per step
- **Iteration Count**: how many times you rerun the loop

In code workflows, both often grow together.

## Chat vs Code

Chat is usually scoped and single-pass. Code tools are iterative and context-heavy.

| Mode | Token Usage | Why |
|------|------------|-----|
| Chat | Low | Scoped, single-pass |
| Code | High | Context + iteration |
| Agents | Extreme | Recursive workflows |

**LLMs are cheap for thinking, but expensive for execution.**

## What’s Actually Burning Your Tokens

Most waste comes from predictable mechanics:

- loading large files or many files
- repeated plan-edit-validate loops
- hidden system/tool overhead
- retries caused by ambiguous prompts

A realistic coding step:

- context load: `15k-30k` tokens
- reasoning/planning: `5k-10k` tokens
- edit + validation: `5k-10k` tokens

Total per step: **`25k-50k` tokens**.

Run that loop 4-6 times and you are in **`100k-250k+`** quickly.

## The Biggest Mistakes People Make

- dumping the full repo for a local change
- using vague prompts
- doing exploration inside code tools
- rerunning full pipelines for one-stage edits
- letting agents run without hard boundaries

These are workflow issues, not model defects.

## The Fix Practical Playbook

Use four controls together:

- separate thinking from execution
- control context aggressively
- reduce iteration count
- use the right tool for each phase

## A Better Workflow

1. Plan in chat
2. Execute in code tools
3. Avoid unnecessary loops

Why this works:

- planning stays low-cost
- execution stays scoped
- fewer retries means less compounding token spend

**This alone can reduce usage by 5-10x.**

## Real Example (Before vs After)

| Scenario | Tokens Used |
|----------|------------|
| Naive approach | ~200k |
| Optimized approach | ~15k-40k |

Typical shift:

- naive: full repo context + broad prompts + repeated reruns
- optimized: targeted files + explicit boundaries + stage-level reruns

## Context Control Techniques

- Pass only required files, functions, and logs.
- Summarize non-critical context before handoff.
- Avoid full repository ingestion for localized edits.
- Keep instruction files concise to avoid overhead.

## Pipeline Optimization

- Break workflows into stages and rerun only changed stages.
- Avoid end-to-end reruns for local fixes.
- Cache stable intermediate artifacts.

Example stage split:

- extraction
- classification
- field extraction
- publishing

If one stage changes, rerun from that stage forward only.

## Prompt Efficiency Patterns

Bad:

```text
Improve this code.
```

Good:

```text
Update function X to handle edge case Y. Do not modify other parts.
```

Why it matters:

- tighter prompts reduce ambiguity
- lower ambiguity reduces retries
- fewer retries reduce token waste

## Signs You’re About to Hit Limits

- responses slow down each turn
- repeated retries on the same task
- larger inputs on each step
- repeated runs of near-identical workflows

If two or more appear, tighten scope immediately.

## Agent Token Explosion

Agents multiply token usage through recursion, orchestration, and memory growth.

Main pressure points:

- recursive calls stack context
- multi-step flows add overhead per handoff
- persistent memory inflates prompt size

Practical controls:

- set hard step boundaries and stop conditions
- cache intermediate outputs
- cap recursion depth and retry count
- summarize or reset memory between phases

## Claude-Specific Optimization

### 1. CLAUDE.md usage

`CLAUDE.md` centralizes stable instructions so you do not repeat long setup prompts every turn.

Token impact:

- cuts repeated instruction overhead
- lowers per-turn payload in long sessions
- improves consistency, reducing retries

Keep it short. Bloated instruction files erase the benefit.

### 2. Skills

Skills package reusable capabilities into structured patterns.

Token impact:

- reduces prompt repetition
- improves task precision and lowers retries
- standardizes outputs for downstream steps

### 3. MCP (Model Context Protocol)

MCP fetches precise context on demand instead of pasting large blocks into prompts.

Token impact:

- replaces broad context loading with targeted retrieval
- reduces context window pressure
- keeps prompts focused on decision-relevant data

| Technique | What It Optimizes |
|----------|------------------|
| CLAUDE.md | Instruction overhead |
| Skills | Prompt repetition |
| MCP | Context size |

## Operating Rules

1. Never send more context than needed.
2. Never rerun full workflows unnecessarily.
3. Separate planning from execution.
4. Scope prompts tightly.
5. Cache intermediate results.

## Final Principle

LLM usage isn’t about how much you use - it’s about how efficiently you structure the work.

## About This Playbook

This guide is for practitioners hitting usage limits in real workflows. It focuses on operational controls that reduce token burn without reducing output quality.
