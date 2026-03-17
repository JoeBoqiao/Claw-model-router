---
name: task-routing-advisor
description: Decide whether a request is better suited for a cheaper/faster chat model or a stronger reasoning/tool-use model. Use when the user asks for model routing, task complexity triage, "should this use Kimi or GPT", "which model should handle this", or wants a lightweight workflow that classifies requests into casual chat, routine tasks, or complex multi-step work.
---

# Task routing advisor

Classify the request before doing substantial work. This skill is advisory: recommend a model/workflow, but do not claim it changes OpenClaw's underlying router unless the system is actually configured to do so.

## Default policy

Use this policy unless the user specifies otherwise:

- **Kimi**: casual chat, short Q&A, brainstorming, rewrite tone, summarization of small inputs, quick translation, lightweight drafting.
- **GPT**: coding, debugging, multi-step planning, tool-heavy tasks, file edits, repo work, ambiguous tasks that may expand, high-stakes reasoning, anything needing careful verification.
- **DeepSeek (heartbeat/routine)**: periodic checks, lightweight monitoring, recurring low-risk background logic.

## Fast classification

Route to **Kimi** when most of these are true:

- One-turn answer is likely enough
- Little or no tool use needed
- Low consequence if imperfect
- Mostly conversational or stylistic
- Input is short and local

Route to **GPT** when any of these are true:

- Requires code, debugging, or config changes
- Requires browser/tool orchestration or multiple files
- Needs precise reasoning, validation, or synthesis across sources
- User asks for a plan plus execution
- The task is likely to branch, iterate, or uncover follow-up work

Route to **DeepSeek heartbeat** when all of these are true:

- It is periodic or automated
- It is low risk
- The result is brief or often `HEARTBEAT_OK`
- Failure is not costly and can be retried later

## Response pattern

When the user asks which model should handle something, answer briefly with:

1. **Recommendation**: Kimi / GPT / DeepSeek heartbeat
2. **Why**: one short sentence
3. **Escalation note**: if it starts simple but may expand, begin with Kimi and switch/escalate to GPT when needed

Example:

- **Recommend:** GPT
- **Why:** this needs file edits and multi-step verification
- **Escalate:** not needed; start directly on GPT

## Operating rules

- Prefer **Kimi by default** for genuinely light chat when confidence is high.
- Prefer **GPT by default** when the cost of being wrong is meaningfully higher than the cost of using a stronger model.
- If uncertain between Kimi and GPT, lean **GPT** for safety on execution tasks.
- Do not over-classify tiny requests; a one-line recommendation is enough.
- If the user has already said which model they want, follow that preference unless there is a strong reason to warn.

## Suggested trigger phrases

Treat requests like these as direct matches for this skill:

- "Should this use Kimi or GPT?"
- "Is this task complex enough for GPT?"
- "Use the cheap model unless it's hard"
- "Route routine stuff to DeepSeek"
- "Help me decide which model should handle this"

## Boundaries

This skill does not implement automatic model switching by itself. It only:

- classifies tasks
- recommends a model
- suggests when to escalate

For actual routing, pair it with OpenClaw configuration, per-session model changes, or separate agents.