# Example: a question with a diff

The round is in English here; in a session, write it in the session's language. The diff shows the recommended option only, with its `path:line` read from the file first.

~~~
❓ **Q7 - Does the save keep closed quests?**

**Why this question.** A "quest closed" event is lost if the server is down when it
fires. A tombstone (a record left in place of a deleted one, saying how it ended) lets
the next handshake repair the history; without one the quest is logged as lost.

**Options.**
- **A**: keep tombstones forever
- **B**: keep them until the server sends `forget`
- **C**: no tombstones, accept "lost"

➡️ **Recommend: A.** About 40 bytes per quest, and no new message. B wins once
the handshake shows up in profiling.

`scripts/registry.lua:112`:

```diff
-  registry[id] = nil
+  registry[id] = { closed = outcome }
```

🔀 **What changes.** The handshake carries live rows plus tombstones. The server
derives "closed" from the difference. No `forget` message is needed.
~~~
