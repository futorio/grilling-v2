# Example: a question with a call tree

The tree gives context the user needs to pick an option. Its root carries `path:line`; children stay bare names.

~~~
❓ **Q4 - When does the script connect to the server?**

**Why this question.** The save is restored on the first game update, and the
handshake sends the restored state. If the connection opens before the restore, the
server gets an empty table and marks every quest as lost.

**Options.**
- **A**: connect at the end of the restore, then send hello
- **B**: connect at load and queue hello until the restore finishes

➡️ **Recommend: A.** One ordering, no queue. B wins if the connection is slow
enough to delay the first frame.

```text
on_first_update (scripts/main.lua:30)
  restore
    read_save
  connect
  send_hello
```

🔀 **What changes.** `connect` moves out of the load hook. Hello always carries the
restored table, so the server needs no "empty hello" case.
~~~
