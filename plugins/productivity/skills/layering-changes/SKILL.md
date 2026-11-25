---
name: layering-changes
description: Use when the user brings a software change — a task, feature, fix, refactor, or idea — and wants to know what to think about first, second, third. Sorts open questions into levels (Why, What, Design, Build) and proposes steps. Does not ask or answer the questions itself.
---

# Layering Changes

Lay out the thinking order for a software change. Map the change onto levels, mark what is already settled and what is open, and point to where to start. Software development only.

This skill does not interview the user and does not answer the open questions. It only shows which questions exist and in what order to close them. Unlike `grilling` or `idea-refine`, it never tries to settle everything at once.

## Terms

- **Change** — whatever the user brings: a task, feature, fix, refactor, or raw idea.
- **Level** — one layer of thinking about the change. Levels are listed below.
- **Open question** — something on a level that is not yet clear from the input.
- **Settled** — a level whose open questions are closed: a decision, a fact, a constraint, or gathered context.
- **Step** — one working session that closes one or more levels and ends at a natural pause.

## Process

1. Read the input. Do not ask anything.
2. For each level, set a status:
   - **settled** — clear from the input;
   - **open** — has open questions; list 1–3 key ones;
   - **skip** — not relevant to this change (e.g. a refactor with no behavior change skips What).
3. Point to the first open level from the top. That is where to start.
4. Group levels into steps (see Steps).

## Order rules

- Go top-down by default.
- Skip a level only if it is settled or not relevant.
- Do not start a lower level while a higher level has open questions that could change it.
- If a lower level reveals a problem with a higher one, go back up and note it.

## Levels

The boundary test for each level: can it be discussed without knowing the next level? If yes, it belongs here. A mistake on a level forces rework of everything below it.

### 1. Why (problem / goal)

- **Covers:** what is wrong or missing now, who needs the change, how we will know it worked.
- **Belongs here if:** it can be discussed without picturing any screen or behavior.
- **Who can answer:** whoever needs the change.
- **Settled when:** the goal and the success signal can be stated in one or two sentences.
- **Example question:** "Do we need export at all?"

### 2. What (requirements / behavior)

- **Covers:** what the user sees and does, main scenarios, edge cases, what is out of scope.
- **Belongs here if:** it can be checked by hand without knowing how it is built.
- **Who can answer:** a user or a tester.
- **Settled when:** every scenario has an expected result and scope limits are listed.
- **Example question:** "Export to CSV or PDF?"

### 3. Design (data / API / structure)

- **Covers:** data shape, API contracts, which parts of the system change, anything costly to redo later.
- **Belongs here if:** the user does not see it, but changing it later is expensive.
- **Who can answer:** a developer.
- **Settled when:** data, interfaces, and affected modules are named.
- **Example question:** "Run export synchronously or through a queue?"

### 4. Build (implementation)

- **Covers:** steps and their order, tasks, tests, rollout and migration.
- **Belongs here if:** it is easy to change while working.
- **Who can answer:** whoever does the work.
- **Settled when:** the first concrete step is clear.
- **Example question:** "Backend first or frontend first?"

## Steps

Split into steps at pauses, not at every level. A pause is an open question the user cannot answer right now: it needs someone else, research, reading code, or a prototype.

- **Small change** (Why and What are settled): one step.
- **Default pairs:** Why + What (product view) and Design + Build (engineering view). Settle What before going into Design.
- Each step can run as a separate session.

## Recommended skills

Third-party skills that work well inside a step. Suggest one only when it fits the open level, and only if it is installed. Scope it to that level's open questions, not the whole change.

- **`idea-refine`** (`addyosmani/agent-skills`) — Why is open and the idea is still vague.
- **`grilling`** (`mattpocock/skills`) — a level has open questions that need to be worked through one by one.
- **`planning-and-task-breakdown`** (`addyosmani/agent-skills`) — Design is settled and Build needs ordered tasks.

## Output

Keep it short: no explanations, no preamble. The reader should get it at a glance.

- Reply in plain markdown, not inside a code block.
- Always show all four levels, in the same order, one list item each, level name in bold.
- Start each item with a status icon: ✅ settled, ❓ open, ⏭️ skip.
- Mark the starting level with 👉 at the end of its item.
- Write open questions as short phrases (up to ~8 words), separated by `·`.
- Under the levels, list steps: ▶️ for the first step, ⏸️ for steps that wait on an earlier one.
- If a recommended skill fits a step, add it at the end of that step: `→ grilling`.

Example (the source of the reply, rendered as markdown):

```markdown
- ✅ **Why**
- ❓ **What** — CSV or PDF? · what if list is empty? 👉
- ❓ **Design** — sync or queue?
- ❓ **Build**

- ▶️ **Step 1** · What → `grilling`
- ⏸️ **Step 2** · Design + Build — after step 1
```
