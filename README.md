# Eziclip skills

Agent skills for editing video with [Eziclip Studio](https://eziclip.com/ai) — a real multi-track
video editor that your AI agent drives over MCP. Rendering happens on your machine; your footage is
never uploaded.

## Where this lives

**This folder is published as a public repository: `github.com/Hello-ivan-c/eziclip-skills`** (MIT).
It is the only part of Eziclip that is open — the app, its source and the editing craft library stay
private. In the app's monorepo the source of truth is `packages/skills/`; it is mirrored to the
public repo with `scripts/publish-skills.sh` (see below). Do not edit the public repo directly.

## Install

```bash
npx skills add Hello-ivan-c/eziclip-skills
```

Pick the skills you want. Or install one by hand (Claude Code):

```bash
mkdir -p ~/.claude/skills/eziclip-video
curl -fsSL https://eziclip.com/skills/eziclip-video/SKILL.md -o ~/.claude/skills/eziclip-video/SKILL.md
```

## Skills

| Skill | |
|---|---|
| [`eziclip-video`](skills/eziclip-video/SKILL.md) | The editing workflow: watch, cut with a reason, preview, then export |
| [`eziclip-cli`](skills/eziclip-cli/SKILL.md) | Set up and script the `eziclip` CLI: API key, doctor, JSON output |

## Connect the editor

The skills teach the workflow; the tools come from Eziclip's MCP server:

- hosted link: https://eziclip.com/ai/mcp (open Studio → Connect AI agent → Copy link)

`.mcp.json` and `.claude-plugin/plugin.json` make this repo installable as a Claude Code plugin.

## Maintaining (monorepo side)

```bash
scripts/publish-skills.sh git@github.com:Hello-ivan-c/eziclip-skills.git   # subtree-split + push to main
```

`public/skills/*/SKILL.md` on eziclip.com are copies of `skills/*/SKILL.md` here; a test fails if
they drift.
