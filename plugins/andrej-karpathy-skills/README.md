# Andrej Karpathy Skills for Codex

This directory contains the Codex-specific plugin packaging for this repository.

It is intentionally isolated from the existing Claude Code plugin files:

- Claude Code plugin files stay under `/.claude-plugin/`
- Codex plugin files stay under `/plugins/andrej-karpathy-skills/` and `/.agents/plugins/`
- The Codex plugin ships its own copy of `karpathy-guidelines` under `./skills/karpathy-guidelines/`

That duplication is deliberate so Codex-specific changes do not affect the existing Claude Code plugin.

## Files

- `.codex-plugin/plugin.json`: Codex plugin manifest
- `skills/karpathy-guidelines/SKILL.md`: Codex plugin-local skill copy
- `hooks.json`: empty hook config stub reserved for future Codex hook wiring
- `.mcp.json`: empty MCP stub reserved for future server config
- `.app.json`: empty app stub reserved for future app integrations
- `../../.agents/plugins/marketplace.json`: repo-local Codex marketplace entry

## Recommended install: repo-local marketplace

This repository already includes the structure Codex expects for a repo-scoped local plugin:

- plugin folder: `plugins/andrej-karpathy-skills/`
- marketplace file: `.agents/plugins/marketplace.json`

Use this flow from the repository root:

1. Open this repository in Codex.
2. Confirm these two files exist:
   - `plugins/andrej-karpathy-skills/.codex-plugin/plugin.json`
   - `.agents/plugins/marketplace.json`
3. Restart Codex so it reloads repo-local marketplaces.
4. Open the plugin directory in Codex.
5. Choose the marketplace named `Andrej Karpathy Skills`.
6. Install or enable the plugin `andrej-karpathy-skills`.

## What Codex will load

The marketplace entry points to:

- `./plugins/andrej-karpathy-skills`

That path is resolved relative to the repository root, not relative to `.agents/plugins/`.

The plugin manifest then points Codex at:

- `./skills/`
- `./hooks.json`
- `./.mcp.json`
- `./.app.json`

Inside this plugin directory, that resolves to:

- `plugins/andrej-karpathy-skills/skills/karpathy-guidelines/SKILL.md`
- `plugins/andrej-karpathy-skills/hooks.json`
- `plugins/andrej-karpathy-skills/.mcp.json`
- `plugins/andrej-karpathy-skills/.app.json`

## Verify

After installation, ask Codex to use the skill with a prompt like:

```text
Use karpathy-guidelines to review this implementation plan before coding.
```

If the plugin was loaded correctly, Codex should be able to find and apply the `karpathy-guidelines` skill from the plugin-local copy.

## Updating the plugin

If you change any of these files:

- `plugins/andrej-karpathy-skills/.codex-plugin/plugin.json`
- `plugins/andrej-karpathy-skills/skills/karpathy-guidelines/SKILL.md`
- `.agents/plugins/marketplace.json`

restart Codex so the local marketplace install is refreshed.

## Why the skill is duplicated

The Codex plugin intentionally carries its own copy of `karpathy-guidelines`.

The repository also contains a Claude Code plugin that points at the original repo skill tree. Keeping a duplicated Codex copy avoids accidental breakage if the Codex packaging needs changes later.
