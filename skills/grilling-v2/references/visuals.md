# Visual shapes

Adapted from the `show-me` skill in [humanlayer/skills](https://github.com/humanlayer/skills) (MIT, Copyright (c) 2026 HumanLayer). Cut to the shapes that render in a terminal.

Pick the smallest shape that makes the question's point. Keep only the calls, files, states, and boundaries the question needs.

Use plain-text code blocks only (`text`, `diff`, or the file's language). Write status in words ("in spec", "out of spec"), never emojis.

## Pseudocode: logic or an algorithm

```text
on(save)
  if content is unchanged
    return cached result
  write new content
  return fresh result
```

## Call tree: runtime control flow

The root carries `path:line`; children stay bare names.

```text
submitForm (src/forms/submit.ts:42)
  createSession
    persistPrompt
    launchAgent
  navigateToSession
```

## File tree: who owns what, or a broad refactor

```text
src/
├── commands/       # parses user actions
├── sessions/       # owns session state
└── transport/      # sends API requests
```

## Diff: what the recommended option changes

Use a diff when the surrounding shape already exists. Put the `path:line` of the first changed line above a code diff.

Code:

`src/save.ts:10`:

```diff
 function save(doc) {
-  write(doc)
+  if (unchanged(doc)) return cached(doc)
+  write(doc)
+  invalidate(doc.id)
 }
```

File layout:

```diff
 src/
 ├── commands/
+│   └── export.ts        # new command
 ├── sessions/
-└── transport.ts
+└── transport/
+    ├── client.ts
+    └── stream.ts
```

Call tree:

```diff
 submitForm (src/forms/submit.ts:42)
   createSession
     persistPrompt
+    expandMention
     launchAgent
```

State or control flow:

```diff
 on(save)
-  write content
+  if content is unchanged
+    return cached result
+  write new content
+  invalidate cache
```

## Whole block

Show the whole block instead of a diff when most of it is new, or when omitted context would hide ownership or order.

`src/mentions.ts:1`:

```ts
function expandMention(text: string): string {
  return text.replace(/@(\w+)/g, (_, name) => `use the ${name} skill`)
}
```
