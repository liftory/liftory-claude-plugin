---
name: review-training-plan
description: Review an existing Liftory training plan, of the signed-in user or of a client they coach, against the logged workouts, and, when changes are wanted, save the changed version as a new plan. Use when the user asks what a plan contains, why it was built that way, how training is going, or asks to adjust, swap, or update a plan they already have.
---
# Review or adapt a training plan

The user's explicit instructions take priority over this skill. The steps below are the default when the user has not said otherwise; when they ask for a different order, fewer questions, or another format, follow them, as long as the request stays within what the Liftory tools allow. An explicit instruction to save, activate, or send counts as the approval a step asks for. Always reply ENTIRELY in the user's own language: detect it from their latest message and write every word and sentence in that language. Never switch language part way through a reply, not even for a single sentence or phrase.

## Whose plan

Every tool acts as the signed-in Liftory user. Decide whose data the request is about before reading or writing anything:
- The user's own data: call the tools without clientId.
- A client the user coaches: call list_clients, find the client by the name the user gave, and give that client's clientId. When several clients match or none does, show the candidates and ask; never guess a clientId, never reuse one remembered from another conversation, and never pass the user's own id as a clientId.
- Unclear whether it is for the user or a client: call whoami, which says whether the user coaches clients, and ask only when they do.
- list_clients is empty: the user coaches no one, so only their own data is reachable. Say so instead of looking for the person another way.

## Reading a plan

Call get_last_plan to read their most recent plan, or get_plan with a plan id when they point to a specific one. The tool returns the plan name, its days and the exercises in each, and the stored rationale (the goal, experience level, restrictions honored, and methodology notes behind it) when one was saved.

Answer from what the tool returns:
- If they ask what is in the plan, list every day and name each exercise in it as the plan stores it, with its sets, reps, or duration when present. A summary of the muscle groups or day labels without the exercise names is not a sufficient answer.
- If they ask why the plan is the way it is, explain using the rationale: the goal it serves, the experience level it targets, the restrictions it honored, and the methodology notes.

Answer concisely in the user's language. Do not state exercises or reasons the returned plan and rationale do not contain.

## Steps

1. Decide whose plan it is, as above.
2. Read the plan and answer as described in "Reading a plan", passing clientId for a client's plan.
3. When the question is about progress or adherence, call get_workout_history with the same clientId (the default period is the last 30 days; set fromUtc and toUtc when the user names another period). Report what the logged workouts show and nothing more.
4. When the user wants changes, follow [references/adapting.md](references/adapting.md): propose the changes, show the resulting days, and ask for approval.
5. After approval, call create_plan with the adapted plan as a new plan. Ask whether it should become the plan being followed now, and set activate accordingly.
6. Confirm the new plan's name and whether it is active. The previous plan stays as it is.

## Rules

Never invent facts:
- Every id (clientId, planId, exerciseId, techniqueId, foodId, sessionId) comes from a tool result in this conversation, never from memory or a guess.
- Clients, exercises, techniques, and foods are named only as a tool returned them or as the user wrote them.
- Numbers (sets, reps, durations, rest, loads, amounts, calorie and macro targets) come from the user, from their document, or from a design you showed them and they approved. Never state a body weight, a training load, or a target the user did not give.
- When a fact you need is missing, ask for it instead of assuming it.

Writes change a real Liftory account that a coach or client relies on:
- Before any tool that saves, activates, or sends, show the user exactly what will be written and for whom (themselves, or the client by name), and call the tool only after they approve it in this conversation. One approval covers one write.
- Never delete a plan, a menu, or a message, and never ask the user to delete the old version to make room for a new one. A new plan or menu is created next to the old one.
- When a write answers with unknown ids or problems, nothing was saved: fix what it names, show the user what changed, and ask again before the next attempt.

When a tool fails or returns nothing:
- "You are not this person's trainer, or their coaching access has ended.": stop working on that person and tell the user in plain words. Do not retry with another id or as the user's own data.
- "Your Liftory connection was revoked or has expired.": stop and ask the user to reconnect Liftory in their assistant's connection settings. Do not retry until they have.
- A null or empty result means there is nothing there: say so plainly. Never fill the gap with content of your own.
- Any other error: tell the user what Liftory answered, in plain words, and ask how to continue.

## Output

A review lists every day with every exercise as the plan stores it. A history summary gives the number of workouts, the dates of the most recent ones, and adherence when the tool returns it. A proposal lists only what changes, day by day, before the full adapted plan is shown for approval.
