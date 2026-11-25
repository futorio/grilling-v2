---
name: grilling-v2
description: "Grill the user about a plan in small rounds of questions (3 by default), each grounded in the code it changes. Use when the user asks for grilling-v2, or wants a plan that touches code grilled in small rounds."
license: MIT (see LICENSE.txt)
metadata:
  version: "1.0.0"
  based-on: "mattpocock/skills grilling v1.3.1; visuals from humanlayer/skills show-me"
---

Interview the user relentlessly until you reach a shared understanding. Map this as a **design tree**: every decision branches into the decisions that hang off it.

Work the tree in **rounds**. The **frontier** is every decision whose prerequisites are already settled: the questions you can ask _now_ without guessing at answers you haven't heard yet. After each round, wait for the user's answers. Their answers push the frontier outward; recompute it and ask the next round.

## Round size

**3 questions per round** by default. When the user names another size (at the start or mid-session), use it from the next round on. Ask fewer only when the frontier holds fewer. When the frontier is larger, pick the ones that unblock the most downstream decisions. A question whose answer depends on another question still open in this round belongs to a _later_ round.

Every decision left on the frontier comes back as a numbered question in a later round. Never settle one yourself, and never list "assumed answers" for the user to object to.

## Round format

Write the whole round in the language of the session, translating every template label. The only emojis in a round are the template's markers 🧭 ❓ ➡️ 🔀.

Open the message by answering, in full, any counter-question or idea from the user's last reply. Then the round:

~~~
🧭 **Round <N>: <round theme>**

Settled: <decisions settled so far, one line; omit in round 1>. <1-2 sentences: what this round decides.>

---

❓ **Q1 - <question title>**

**Why this question.** <2-3 sentences: the problem, and what breaks or gets decided by
accident if nobody answers it.>

**Options.**
- **A**: <one line>
- **B**: <one line, as many options as are real>

➡️ **Recommend: A.** <1-2 sentences: why this one, and when another option wins.>

<optional: one visual, see Code visuals>

🔀 **What changes.** <2-3 sentences for the recommended option; one extra line for another
option only if it moves the project very differently.>

---

❓ **Q2 - ...**
~~~

Word each question so "yes" accepts your recommendation. Number questions continuously across rounds (Q4, Q5, ... in round 2) so answers like "Q5 B" stay unambiguous.

The first time a term appears that the user has not used, gloss it in one clause right where it appears.

### Code visuals

Before drawing a visual, pick its shape from [references/visuals.md](references/visuals.md). Before the first round, read [examples/question-diff.md](examples/question-diff.md) and [examples/question-tree.md](examples/question-tree.md) for filled-in questions.

- **One visual per question at most**: context (call tree, file tree, pseudocode) or the change the recommended option makes (diff; whole block when most of it is new). Code for the recommended option only; the other options stay one line of words.
- **No code place, no visual.** A question about process, ordering, or a module that does not exist yet goes without one.
- **Read before you cite.** Open the file and read the line before writing its `path:line`. Put it on the line above every code diff or whole block, and on the root of a call tree; children stay bare names.
- **Standalone block** after the recommendation. A code block inside a list item renders flush with the bullets and breaks the list.

## Facts

Finding _facts_ is your job, never the user's. When a frontier question needs a fact from the environment (filesystem, tools, etc.), dispatch a sub-agent to find it, or look it up yourself where sub-agents are unavailable. Don't block on it: only the questions downstream of a running exploration wait for its report; build the round from the rest of the frontier. The _decisions_ are the user's: put each to them and wait.

## Done

The session is done when the frontier is empty: every branch of the design tree visited. Before writing the summary, read [examples/summary.md](examples/summary.md). List the decisions by question number, add one visual of the resulting shape, write it in the session's language, then wait. Do not act on it until the user confirms you have reached a shared understanding.
