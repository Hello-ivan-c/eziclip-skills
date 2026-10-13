---
name: eziclip-cli
description: Operate the eziclip command-line tool — sign in from the browser, check Chrome and the plan, and run the headless Eziclip Studio MCP server. Use when the user wants to set up or troubleshoot eziclip in a terminal, CI or an agent config. For actually editing video once MCP is connected, use the eziclip-video skill and the studio tools — not ffmpeg.
---

# eziclip CLI

`eziclip` starts Eziclip Studio headless on the user's machine and serves it to an agent over MCP.
Files go in and out by path — no upload, no base64, no size cap.

Editing happens **inside** the MCP session (import_path → studio tools → export_to_path). Do not
re-implement the edit with ffmpeg in the shell while `eziclip mcp` is available — see the
`eziclip-video` skill.

## Setup

```bash
eziclip login                     # opens the browser: the user presses Allow once
eziclip doctor --json             # Chrome, studio address, key, plan
claude mcp add eziclip -- npx -y eziclip mcp
```

- Needs Node 20+ and Google Chrome (or Chromium / Edge) — video export uses Chrome's H.264 encoder.
  Set `EZICLIP_CHROME` to a specific browser.
- The key is read from `--api-key`, then `EZICLIP_API_KEY`, then `~/.config/eziclip/credentials.json`.
  In CI set `EZICLIP_API_KEY` (a key made in Profile → API keys); do not run `login`.
- Never print or commit the key. Revoke it in Profile → API keys if it leaks.
- Inside an MCP session you do not need `login`: with no key the server offers a `sign_in` tool. Call it, show the user the link and the code it returns, and call it again once they press Allow — the editing tools appear.

## Contract (safe to script)

- `--json` or `-q` (or piped stdout): JSON on stdout. Errors are JSON on stderr:
  `{"error":{"message":"…","code":"auth_error"}}`. Exit code `0` = success, `1` = error.
- `eziclip commands` prints the command tree as JSON — use it for discovery.
- Error codes: `missing_key`, `bad_key`, `auth_error`, `network_error`, `rate_limited`, `accounts_off`.

## Commands

| Command | What it does |
|---|---|
| `eziclip mcp` | MCP server on stdio (what an agent runs) |
| `eziclip login` | sign in: the user presses Allow in the browser |
| `eziclip login --key …` | validate and save a key made in Profile (CI) |
| `eziclip whoami` | account the key belongs to |
| `eziclip logout` | forget the saved key |
| `eziclip doctor` | Chrome, studio, key and plan (exits 1 when something is wrong) |

Options for `mcp`: `--origin`, `--api-key`, `--no-launch`, `--headed`, `--profile`, `--port`, `--cookie`.

## Troubleshooting

- "No Chrome found" → install Chrome or set `EZICLIP_CHROME`.
- "requires sign-in" → call the `sign_in` tool (or run `eziclip login`) and have the user press Allow; for CI set `EZICLIP_API_KEY`.
- "auth_error" → the key is wrong or revoked; create a new one.
- Free agent exports used up → the CLI says how long until the next one and gives the link to go unlimited. Tell the user, do not retry in a loop; their own exports in the Studio are never limited.

Docs: https://eziclip.com/ai/cli
