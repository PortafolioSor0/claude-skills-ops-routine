---
name: daily-plan
description: Second step of the operator's daily routine for the client account. Builds the day's plan from docs/pendientes.md (unfinished tasks from earlier days plus urgent new ones), proposes an order, and asks the operator which tasks to implement before doing anything. Use when the operator asks "qué hacemos hoy", "planea el día", "qué falta", "qué sigue" or runs /daily-plan, usually after /daily-intake.
---

# Daily plan

Goal: the operator picks, with a single question, what gets done today. **Implement nothing until they answer.**

Speak to the operator in Spanish and start with their name.

## 1. Read the current state

- `docs/pendientes.md`: the single task list.
- The latest note in `docs/reuniones/`: decisions and changes Jordan asked for.
- The "Pendientes conocidos" section of `CLAUDE.md`, and the `handoff.md` of any spec that has dated tasks.
- `git log --since` the last meeting: what has already been done, so nothing gets proposed twice.
- If `daily-intake` didn't run today, check whether there's a new meeting or files in `New/`, and suggest running it first.

## 2. Classify each open task

| Group | Criterion |
|---|---|
| **Urgent** | Deadline within 3 days, or it blocks something with a fixed date (compliance review ~2 weeks ahead, scheduled campaign emails, ad launch dates, the end of the Medicare Annual Enrollment Period on 12/7, a webinar) |
| **Client is waiting** | Jordan asked for it, or it's a document Jordan has to approve or upload |
| **Can be done now** | No blockers, and Claude can do it with the repo, the CRM (API or browser) or Gmail |
| **Blocked** | Depends on Jordan, an outside vendor, compliance or an insurer. It's not proposed; it only shows as "what we're waiting on" |
| **Other session** | Images or visual design: the operator does it in another Claude session. Mention it, don't propose it |

Rules:
- Unfinished tasks from earlier days come before new ones that aren't urgent.
- Writes to the CRM or any outward action (sending an email, publishing, activating a workflow) are proposed **with a warning** that they need the operator's OK. Never touch active workflows.
- Include the time cost when it's large (for example, "browser in the workflow builder, slow").

## 3. Show the plan and ask

1. In chat, briefly:
   - **Urgent:** 1–3 tasks, each with its reason (deadline or blocker).
   - **Can be done now:** the rest, in recommended order.
   - **What we're waiting on:** one line per person or vendor.
2. Then **one** `AskUserQuestion` with `multiSelect: true`, with up to 4 options (the most useful ones; the first marked "(Recommendation)"). The operator can add others under "Other".
3. Implement only what they pick, in that order. After each task:
   - mark it `[x] … · hecho AAAA-MM-DD` in `docs/pendientes.md`;
   - commit (CLAUDE.md: the operator as author, no AI attribution, no push).

## 4. At the end of the day

When the tasks the operator picked are done, suggest running `/client-update` to prepare the page for the next meeting.
