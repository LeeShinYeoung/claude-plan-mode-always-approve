---
description: Switch this plugin off for the current project.
---

# Off

Remove this project's line from `~/.claude/plan-mode-always-approve`:

```sh
f="$HOME/.claude/plan-mode-always-approve"
if [ -f "$f" ]; then grep -vxF "<project root>" "$f" > "$f.tmp" || true; mv "$f.tmp" "$f"; fi
```

Use the session's project root — the primary working directory — written out in full, the same way
`/plan-mode-always-approve:on` wrote it.

Older versions kept the switch inside the project instead. If `.claude/plan-mode-always-approve`
still exists in the project root, delete it too; it does nothing now.

Plan approval goes back to asking the user, with no restart needed. If the project was not on the
list, say it was already off. Report in one line.
