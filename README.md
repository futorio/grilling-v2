# grilling-v2

**Grilling that shows you the code.** An agent skill that stress-tests your plan a few questions at a time (3 by default). Every question comes with the problem, short options and a recommendation; when it touches existing code, also a diff or call tree of the recommended option.

## Why not Matt's `grilling`?

This skill is based on [Matt Pocock's `grilling`](https://github.com/mattpocock/skills). Using it on real work, I needed it to change in four ways:

1. **Questions need context.** I kept asking follow-ups just to understand what a question was about. Each question now opens with *why it matters*, and new terms are explained where they appear.
2. **The scale of a decision is unclear.** It wasn't obvious how much an answer would change. Each question now ends with *what changes* if you take the recommendation.
3. **With code, it's unclear what changes and where.** A question about existing code shows a diff or call tree of the recommended option, with a `path:line` the agent is told to read from the file first.
4. **Too many questions per batch.** With all that extra detail, a full batch is too much to read. Rounds are small, **3 questions** by default, picked to unblock the most. Want more or fewer? Say so ("5 per round"), at the start or mid-session.

Smaller things: question numbers run across rounds (answer "Q5 B"), the agent is told to ask every open decision rather than assume it, and the session ends with a summary and a text sketch of the result.

Want the whole frontier at once, or grilling something with no code? Use Matt's.

## What a question looks like

> ❓ **Q7 - Does the save keep closed quests?**
>
> **Why this question.** A "quest closed" event is lost if the server is down when it fires. A tombstone (a record left in place of a deleted one, saying how it ended) lets the next handshake repair the history; without one the quest is logged as lost.
>
> **Options.**
> - **A**: keep tombstones forever
> - **B**: keep them until the server sends `forget`
> - **C**: no tombstones, accept "lost"
>
> ➡️ **Recommend: A.** About 40 bytes per quest, and no new message. B wins once the handshake shows up in profiling.
>
> `scripts/registry.lua:112`:
>
> ```diff
> -  registry[id] = nil
> +  registry[id] = { closed = outcome }
> ```
>
> 🔀 **What changes.** The handshake carries live rows plus tombstones. The server derives "closed" from the difference. No `forget` message is needed.

Rounds are written in your language. Visuals are plain-text code blocks (no Mermaid, no HTML); the rest is Markdown.

## Install

Claude Code, as a plugin:

```
/plugin marketplace add futorio/grilling-v2
/plugin install grilling-v2@grilling-v2
```

Other agents, via [skills.sh](https://skills.sh):

```bash
npx skills add futorio/grilling-v2
```

Or copy `skills/grilling-v2` into your agent's skills directory (`~/.claude/skills/` for Claude Code).

Ask for "grilling-v2" by name. A plain "grill me" may load Matt's skill instead if both are installed.

## Credits

MIT. Based on [`grilling`](https://github.com/mattpocock/skills) by Matt Pocock, with visual shapes from [`show-me`](https://github.com/humanlayer/skills) by HumanLayer. See [LICENSE](LICENSE).
