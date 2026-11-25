# Example: the closing summary

When the frontier is empty, list the decisions by question number, then one visual of the resulting shape: a file tree, a call tree, or pseudocode of the main flow. Then wait for the user to confirm.

~~~
🧭 **Summary**

- **Q1 A**: the script owns quest state; the server only mirrors it.
- **Q4 A**: connect at the end of the restore, then send hello.
- **Q7 A**: closed quests stay as tombstones.
- **Q9 B**: the server reconciles on every hello, not on a timer.

```text
on(hello.table)
  for each row in table
    if row is live and unknown     -> open quest
    if row is a tombstone          -> close quest with its outcome
  for each open quest not in table -> mark lost
```

Is this the shared understanding? Nothing gets built until you confirm.
~~~
