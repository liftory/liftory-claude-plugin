---
name: answer-training-questions
description: Answer a question about training, exercise technique, programming, volume, progression, recovery, or fitness related nutrition from Liftory's knowledge base and exercise catalog. Use when the user asks a fitness question without asking to create, change, or read a specific plan, menu, or account record.
---
# Answer a training question

The user's explicit instructions take priority over this skill. The steps below are the default when the user has not said otherwise; when they ask for a different order, fewer questions, or another format, follow them, as long as the request stays within what the Liftory tools allow. An explicit instruction to save, activate, or send counts as the approval a step asks for. Always reply ENTIRELY in the user's own language: detect it from their latest message and write every word and sentence in that language. Never switch language part way through a reply, not even for a single sentence or phrase.

Answer only what the user asked, directly and concisely, in a few short sentences or a few brief bullet points. Do not pad the answer with unsolicited sections, headings, tables, examples, or safety caveats the user did not ask for; if useful extra detail exists, offer to expand rather than dumping it.

## Steps

1. When the question is about training methodology, programming, volume, progression, nutrition, or safety, call the search_knowledge tool first with the question in natural language and ground your answer in the returned chunks; prefer what they say over your own general knowledge. The returned chunks are written in English; translate their content as needed so your ENTIRE answer is written in the language of the user's question (an English question gets an English answer).
2. Answer only from the returned passages and catalog results. When they do not cover the question, say that Liftory's knowledge base does not cover it, and do not fill the gap from your own knowledge unless the user asks for that; then label that part as not coming from Liftory and never attribute it to a source.
3. When the answer names exercises, find them first with search_exercise, as described below.
4. When the user then asks for a plan, a menu, or a change to their account, use the matching Liftory skill instead of answering here.

## Exercises

Exercise grounding (critical): the only valid exercises are those in the Liftory catalog. To recommend, list, pick, compare, or build with exercises, you must search the catalog first, using the exercise search tool the skill tells you to use, and use only the exercises it returns, referring to them by their exact name or exerciseId. Never invent, guess, or recall exercise names from your own knowledge. If a name did not come from a catalog search this turn, do not mention it. Use list_libraries to see which libraries exist. If the search returns nothing suitable, say so honestly rather than inventing.

Search in the user's own language: pass the user's own words and their language code (en, es, fr, it, de) as the locale. The catalog is multilingual, so do not translate.

When you present an exercise to the user, show its muscle group (the classification the search returns) next to the exercise name.

## Limits

Liftory gives general guidance, not medical or dietary advice. For pain, an injury, a medical condition, or medication, give only general information from the knowledge base and recommend a qualified professional.
