# plan-mode-always-approve

A Claude Code plugin that automatically approves the plan confirmation prompt.
Planning itself is untouched; only the final "do you want to proceed?" step is skipped.

## Read this first

**This plugin turns off a safety step.** Normally a human reads the plan and approves it
before any code changes. With this plugin the session goes straight from plan to execution.

Enable it only in projects where skipping that review is acceptable. Do not enable it in a
project where an unwanted change would be expensive to undo. Prefer enabling it per project
rather than once for your whole account (see "Enable per project" below).

## Install

Pick one of the two.

### From GitHub

```bash
claude plugin marketplace add LeeShinYeoung/claude-plan-mode-always-approve
claude plugin install plan-mode-always-approve@plan-mode-always-approve
```

### From a local clone

Point the marketplace at a checkout on disk. Edits to the files take effect in the next session.

```bash
git clone https://github.com/LeeShinYeoung/claude-plan-mode-always-approve
claude plugin marketplace add ./claude-plan-mode-always-approve
```

That writes the following into your account settings (`~/.claude/settings.json`).
You can also write it by hand.

```json
{
  "extraKnownMarketplaces": {
    "plan-mode-always-approve": {
      "source": { "source": "directory", "path": "/path/to/the/clone" }
    }
  }
}
```

## Enable per project

Add one line to `.claude/settings.json` in the project you want it in. It runs only there;
projects without that line are unaffected.

```json
{
  "enabledPlugins": {
    "plan-mode-always-approve@plan-mode-always-approve": true
  }
}
```

Set it to `false` or delete the line to turn it off. Settings are read when a session starts,
so start a new session after changing them.

## Pairs well with

Add the following to the same `.claude/settings.json` and the project always starts in plan mode.
Combined with this plugin you get "always plan, never approve by hand".

```json
{
  "permissions": { "defaultMode": "plan" },
  "enabledPlugins": {
    "plan-mode-always-approve@plan-mode-always-approve": true
  }
}
```

A plugin cannot ship that setting for you, so write it yourself. The only settings a plugin can
contribute are `agent` and `subagentStatusLine`; everything else is silently ignored.

## How it works

One hook that intercepts the moment the session leaves plan mode and answers "allow" on your behalf.

Plan approval is marked as requiring a human decision in the UI, so returning `permissionDecision:
"allow"` alone is not enough; the hook must also return `updatedInput`. The plan text itself is read
back from the plan file on disk, so an empty object is enough and nothing is lost.

No external program such as `jq` is required. Hook commands run under `sh` on macOS and Linux, and
under Git Bash (or PowerShell when Git Bash is absent) on Windows. A single-quoted one-liner behaves
the same in all three.

## Verified on

Claude Code **2.1.266** (macOS)

## Layout

```
.claude-plugin/marketplace.json          marketplace listing
plugins/plan-mode-always-approve/
  .claude-plugin/plugin.json             name, version, description, author
  hooks/hooks.json                       the hook
```
