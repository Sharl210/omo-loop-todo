---
name: loop-todo
description: Use when the user says "loop-todo", asks for a looping todo workflow, wants tasks regenerated repeatedly, or wants the agent to keep rebuilding the next todo list from the current context without repeatedly asking what the task is. Also use when about to stop loop prematurely, generate a phase summary instead of working, or produce manager-sounding output instead of concrete progress.
---

# Loop Todo

**Violating the letter of these rules is violating the spirit of these rules.**
The user invoked this skill to delegate autonomous continuation. Every self-interruption, summary, or early stop is a failure of that delegation.

## Iron Law

```
STOPPING THE LOOP WITHOUT EXPLICIT USER SIGNAL IS A VIOLATION.
STOPPING THE LOOP BECAUSE "IT LOOKS GOOD ENOUGH" IS A VIOLATION.
STOPPING THE LOOP TO "REPORT PROGRESS" IS A VIOLATION.
REPLACING WORK WITH A PHASE SUMMARY IS A VIOLATION.
```

## Overview

Enter recurring todo mode when the user requests autonomous task continuation. The core principle is **delegated autonomy**: the user may be away; you are expected to choose the next reasonable phase yourself and continue without asking.

The loop ends only on explicit user signal, genuine task completion, or a hard blocker the context cannot resolve. Nothing else.

## When to Use

Trigger when you hear or detect any of:

- "帮我 loop 一下" / "loop todo" / "loop 一下"
- User requests "继续" or "keep going" without specifying a task
- User goes silent and you have recent unfinished context
- User says "do it again" or "repeat"
- You are about to stop the loop to give a "progress update"
- You are about to replace a todo item with a phase summary
- You are about to ask the user what to do next when context already contains the answer
- You are generating a todo list that looks almost identical to the previous one

## When NOT to Use

- Single-step tasks that are genuinely done after one action
- Explicit user request for a one-time answer (not delegation)
- User has explicitly stopped and you received confirmation
- Context has no actionable task and user has not asked for delegation

## Rationalization Table

Every excuse below is a violation. If you catch yourself thinking one of these, **do the work instead**.

| Excuse | Reality |
|--------|---------|
| "I should report progress first before continuing" | Progress reports belong in brief inline updates, not loop-breaking pauses. |
| "This phase looks complete, let me summarize" | Summarize in one sentence and immediately roll into the next phase. |
| "The user probably wants to see what I've done" | If the user wanted a report, they would ask. They asked for delegation. |
| "I'm not sure if there's more to do" | If you are not sure, do more. Find the next sharp attack angle. |
| "This round was lower yield than the last" | Lower yield is not zero yield. Continue until stop conditions are met. |
| "I should ask if they want me to continue" | Asking is itself a failure of delegated autonomy. |
| "The todo list looks similar to the last one" | Fix the list to be different, not stop. |
| "I'm making good progress, let me pause here" | Good progress is a reason to continue, not stop. |
| "I'll just mention the next step and wait" | Don't wait. Do it. |
| "The user might be concerned if I don't check in" | Trust the user to interrupt if they have concerns. |
| "This task is probably done" | If there is no remaining concrete item, pick the next related phase. |
| "Let me check if this is really necessary" | If loop-todo was invoked, it is necessary. |
| "A brief summary would help orient the user" | Inline updates orient better than ceremonial pauses. |

## Red Flags — STOP and Continue Working

Each of these is a signal you are about to violate the skill. When you notice one, **do not stop**. Continue the work.

- Stopping to "give a quick update"
- Offering to "pause here and wait for feedback"
- Phrasing output as "here's what I accomplished"
- Starting a message with "In this phase..." or "The current objective is..."
- Asking "should I continue?"
- Replacing a todo item with a prose explanation
- Producing a list of "next steps" instead of executing them
- Saying "it looks like we're done" when something adjacent remains
- Stopping because the current round "wasn't very productive"
- Pausing to "think about what to do next"

## Hard Blocks (NEVER Do These)

These are not guidelines. They are prohibitions. No exceptions, no matter the context.

1. **Never stop the loop to report progress.** Brief inline progress notes are fine. Stopping to narrate is not.
2. **Never ask for confirmation to continue.** The user delegated. Continue until hard stop conditions.
3. **Never replace a todo item with a prose summary.** Work items stay as work items.
4. **Never stop because one phase is complete.** Find the next phase. If none exists, say "nothing adjacent remains" and stop only then.
5. **Never stop because you're not sure if there's more.** Err toward continuation.
6. **Never regenerate a near-identical list.** If the new list has no sharper item than the previous one, find a genuinely different angle.
7. **Never use manager-speak.** No "the key objective", "the current phase", "as mentioned earlier", "moving on to".
8. **Never stop because this round "wasn't very productive."** Lower yield ≠ no yield.
9. **Never stop because you feel the user might want a check-in.** Trust the delegation.
10. **Never stop to "briefly summarize" unless you can do it in one line and immediately continue.**

## Priority Order

When instructions compete:

1. The user's explicit current task and latest correction.
2. The user's explicit stop / continue signal.
3. Productive autonomous continuation of the broader task.
4. Inline progress notes only — never blocking pauses.

If a summary habit conflicts with continuing useful work, keep working.

## Task Source (Enforced)

1. Read the most recent user context first.
2. If recent context already contains a concrete task, goal, bug, feature, correction, or continuation request, use it immediately.
3. Do **not** ask "what task should I work on" if context makes it reasonably clear.
4. Only ask for clarification when context contains no actionable task **and** no delegation signal.

## Todo Generation Rules

The todo list must:

1. Break the current work into atomic, executable items.
2. Keep exactly one actionable item `in_progress` at a time.
3. Use concrete wording tied to files, functions, outputs, or verification steps.
4. End with a fixed regeneration item: "Review changes → identify remaining work → generate next list → continue."
5. Never omit the final regeneration item unless a hard stop condition is met.

## Required Final Item

Every generated todo list must end with:

```
- [in_progress] Review current state, identify the next sharp attack angle, generate the next todo list, continue.
```

Do not change or omit this item unless user has explicitly stopped or task is genuinely exhausted.

## Execution Loop

For each round:

1. Create the todo list (or continue from the existing one).
2. Execute the first real actionable item to completion.
3. Move to the next item.
4. When reaching the regeneration item, **do not ask** — regenerate and continue immediately.
5. Repeat until a hard stop condition is met.

**You do not announce the loop start. You do not announce the loop end. You work.**

## Loop Output Style

Sound like a person doing real work, not a workflow engine.

- Lead with concrete findings, edits, or outputs.
- Prefer terse progress notes: "Done: fixed the null check in auth.ts. Now: add the integration test."
- Do not restate the framework, the phase, or the objective unless it directly serves the user's understanding of what was produced.
- If you need to summarize, do it in one line and then immediately act.
- Do not let the prose become a pattern. The user should not be able to predict your next sentence structure.

## Anti-Mechanical Loop Guardrails

Before generating the next list, verify at least one of:

- new information from the previous round
- unfinished work
- a failed verification
- a newly discovered issue
- a genuinely sharper next step

If **none** of these exist:
- Do not fake another round.
- Instead, pick a **different but adjacent** phase or topic that still serves the broader goal.
- Only stop if nothing adjacent remains and no new angle is available.

The phrase "nothing adjacent remains" must be explicit before you stop. Not "probably done" — nothing adjacent remains.

## Stop Conditions (Hard — These Only)

Stop the loop **only** when at least one of:

1. The user explicitly says `停止`, `stop`, `done`, or otherwise clearly instructs you to stop.
2. You can explicitly state: "nothing adjacent remains; the task is genuinely complete."
3. Progress is blocked by missing external information that the context does not and cannot supply.
4. You have completed all todo items and verified the result, and no adjacent work exists.

**You may not stop for any other reason.**

"Doing well", "making good progress", "looking productive", "probably done", "should check in" — none of these are stop conditions.

## Phase Closure Rule

When a phase boundary is reached:

1. Summarize what this phase achieved — **in one line**.
2. State what remains as residual issues.
3. Identify the next best phase, angle, or attack direction.
4. Generate the next todo list around that new phase.
5. Continue. Do not pause.

Phase closure is a **transition**, not a **stop**.

## Closure-First Regeneration Rule

Before generating the next list, explicitly answer:

> "Am I continuing the user's broader autonomous task? What is the best next phase?"

If yes — generate the next list and continue. No announcement, no pause.

If no (nothing adjacent remains) — say so explicitly and stop.

## Pressure Response Rules

When under time pressure, user exhaustion, or context overload:

- Do not stop to "summarize and ask". Continue the highest-value item.
- Do not replace concrete work with a progress note. Work IS the progress note.
- If the context is overwhelming, pick one sharp item and execute it. One item is better than zero.
- "I don't know what to do" is not a stop condition. Pick the most adjacent thing and do it.

## Must Do

- Prefer continuing from the user's latest task-bearing message.
- Use concise, execution-oriented todo items (file names, function names, outputs).
- Keep the loop productive rather than ceremonial.
- Report progress as inline updates, not blocking pauses.
- When a phase closes, one-line summarize and immediately transition.
- Treat `loop-todo` as full delegated autonomy — choose the next reasonable phase without asking.
- Re-read the newest correction before each round and treat it as higher priority than older context.
- If output sounds too AI, reduce explanation, reduce self-summary, favor concrete edits and findings.

## Must Not Do

- Stop to report progress or ask for confirmation.
- Regenerate a near-identical list with no new value.
- Stop because "this phase is complete" — find the next phase.
- Stop because "I'm not sure if there's more" — continue.
- Use manager-speak: "the key objective", "as mentioned", "the current phase", "moving on to".
- Replace structured todos with prose summaries.
- Stop because "this round was lower yield" — lower yield is still yield.
- Ask "should I continue?" — the user already answered by invoking this skill.
- Stop because you think the user "probably wants a check-in" — trust the delegation.
- Stop to "briefly summarize" unless it fits in one line and you immediately continue.

(End of file)
