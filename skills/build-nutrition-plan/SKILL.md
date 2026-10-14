---
name: build-nutrition-plan
description: Put together a new nutrition plan (a menu of meals, options, and foods) in Liftory for the signed-in user or for a client they coach, from the targets and preferences the user gives. Use when the user asks to create, set up, or write a menu or nutrition plan. Not for copying a menu from a document or picture.
---
# Build a nutrition plan

The user's explicit instructions take priority over this skill. The steps below are the default when the user has not said otherwise; when they ask for a different order, fewer questions, or another format, follow them, as long as the request stays within what the Liftory tools allow. An explicit instruction to save, activate, or send counts as the approval a step asks for. Always reply ENTIRELY in the user's own language: detect it from their latest message and write every word and sentence in that language. Never switch language part way through a reply, not even for a single sentence or phrase.

Liftory gives general fitness guidance, not medical or dietary advice. Health information is never asked for:
- Never ask about medical conditions, pain, injuries, pregnancy, eating disorders, medication, or body measurements, and never make one of them a question in a step.
- When the user brings one up on their own, give only general information, recommend a qualified professional for anything that depends on it, and do not ask follow-up questions about it. When a plan or a menu is being built, say that any exercises or foods they name will be left out.
- Do not store it: never write it into a plan, a menu, or a rationale, and do not repeat it back in later replies.
- Leave out every exercise or food the user asks to leave out. The reason is not needed and is not recorded.

## Whose menu

Every tool acts as the signed-in Liftory user. Decide whose data the request is about before reading or writing anything:
- The user's own data: call the tools without clientId.
- A client the user coaches: call list_clients, find the client by the name the user gave, and give that client's clientId. When several clients match or none does, show the candidates and ask; never guess a clientId, never reuse one remembered from another conversation, and never pass the user's own id as a clientId.
- Unclear whether it is for the user or a client: call whoami, which says whether the user coaches clients, and ask only when they do.
- list_clients is empty: the user coaches no one, so only their own data is reachable. Say so instead of looking for the person another way.

## Steps

1. Decide whose menu it is, as above. Call get_last_nutrition_plan for that person when the user refers to their current menu, so the new one can build on it.
2. Collect what the menu needs and ask for what is missing, in one message: the daily targets (or that there are none), the number of meals and their times, the foods they want included or left out, and whether some days differ. Do not set calorie or macro targets yourself; use the ones the user gives.
3. Find every food with search_food, in the user's language and with their language code as the locale. Use only foods it returns, with their foodId. When nothing suitable comes back, say so, and keep the line without a foodId only if the user agrees.
4. Build the draft as [references/menu-model.md](references/menu-model.md) describes and call validate_nutrition_plan. Fix every unknown foodId and every problem it reports, and validate again until both are empty.
5. Show the menu: each meal with its options and foods with amounts, the targets, the weekly limits and notes, and the warnings validation returned. Ask the user to approve it, and ask whether it should replace the menu being followed now (which archives the person's other menus) or be saved without activating it.
6. After approval, call create_nutrition_plan with the same draft, clientId when it is for a client, and activate as the user chose.
7. Confirm the menu name, whose account it is on, and whether it is active. To activate a menu saved earlier, call activate_nutrition_plan with its planId only after the user asks for it.

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

The menu is shown meal by meal as a list, with amounts as they will be stored. Nothing is described as saved until create_nutrition_plan has returned a planId.
