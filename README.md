# Claw Model Router

A lightweight OpenClaw skill for **model-routing advice**.

This repository currently contains one skill:

- `skills/task-routing-advisor/`

## What this skill does

`task-routing-advisor` helps an agent **decide which model is a better fit for a request**.

It is designed for setups where different models have different jobs, for example:

- **Kimi** for everyday conversation and lightweight drafting
- **GPT** for coding, debugging, multi-step work, and tool-heavy tasks
- **DeepSeek** for heartbeat checks or routine background logic

The skill does **not** replace OpenClaw's actual model router. Instead, it provides a reusable decision policy the agent can follow when choosing how to handle work.

## What it is good for

Use this skill when you want the agent to:

- classify requests by complexity
- recommend **Kimi vs GPT** for a task
- identify when something should be escalated from a cheap/light model to a stronger one
- keep **heartbeat / routine checks** on a lower-cost model
- apply a consistent model-selection policy across conversations

Examples:

- casual chat or quick rewrite → recommend **Kimi**
- code changes, config edits, or debugging → recommend **GPT**
- low-risk periodic monitoring → recommend **DeepSeek heartbeat**

## What it does not do

This skill is **advisory**, not a hard router.

It does **not**:

- automatically switch OpenClaw's active model by itself
- override system-level routing rules
- create a full multi-agent orchestration layer

To get real model switching, pair this skill with:

- OpenClaw model configuration
- per-session model changes
- heartbeat model overrides
- separate agents for different workloads

## Repository layout

```text
skills/
  task-routing-advisor/
    SKILL.md
```

## Typical workflow

1. User asks a question or gives a task.
2. The skill classifies the request as light, routine, or complex.
3. The agent recommends a model:
   - **Kimi** for casual/light work
   - **GPT** for complex execution
   - **DeepSeek** for recurring low-risk background checks
4. The user or agent configuration decides whether to actually switch models.

## Why this exists

In real use, model choice often matters as much as prompting:

- stronger models are better for difficult execution tasks
- lighter models are cheaper and often good enough for normal conversation
- background automation usually does not need premium reasoning every time

This skill captures that judgment in a reusable form.
