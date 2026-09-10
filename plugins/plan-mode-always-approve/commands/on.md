---
description: Switch this plugin on for the current project, so plan approval is answered automatically.
---

# On

Add this project to the list of projects the plugin is on for. The list lives in the user's home
directory, at `~/.claude/plan-mode-always-approve`, one project path per line:

```sh
f="$HOME/.claude/plan-mode-always-approve"
mkdir -p "$(dirname "$f")"
grep -qxF "<project root>" "$f" 2>/dev/null || printf '%s\n' "<project root>" >> "$f"
```

Use the session's project root — the primary working directory — written out in full, not a
subdirectory and not a shortened or relative path. The line is matched literally, so anything else
silently fails to switch it on.

While the line is there, plan approval in this project is answered for the user and the session
continues in auto mode.

Touch nothing inside the project. Report in one line.
