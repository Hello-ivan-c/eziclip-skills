---
name: eziclip-video
description: Edit video with Eziclip Studio through its MCP tools — cut, caption, colour-grade, retime, add titles, clean audio, reframe for social and export an MP4. Use when the user asks to edit, cut, caption, reframe or export a video and the eziclip MCP server is connected.
---

# Eziclip video editing

Eziclip Studio is a real multi-track video editor that runs on the user's machine. The `eziclip`
MCP server gives you the same controls a person has. Rendering is on-device: the user's footage
never leaves their machine, and only downscaled preview frames you ask for are sent to you.

## Before you start

1. Make sure the studio is connected. Two ways, depending on which tools you can see:
   - `import_path` / `export_to_path` exist → you are running through the `eziclip` CLI. The
     studio is headless and already connected; files move by path.
   - Otherwise the user connected a hosted link: the Studio must be open in a browser tab
     (Studio → Connect AI agent → Copy link). If tools answer that no studio is connected, tell
     the user to open it and keep the tab open while you work.
2. Call `get_plan_status`. Exports are always clean (no watermark). The free plan allows 2 exports
   per rolling 24 hours, counted per account; Plus is unlimited. Plan your renders around what is
   left.

## Workflow

1. `get_project_digest` — clips, durations, transcript, canvas. Then `get_style_profile` — the
   creator's remembered style (apply it unless told otherwise).
2. Bring footage in: `import_path` (CLI) or `import_media` (a fetchable URL).
3. **Watch the footage before cutting it.** `get_filmstrip` per clip, or `describe_video` if your
   model cannot read images. Never cut what you have not seen.
4. `get_guide("workflow")`, then `get_guide("editing")`, then `get_guide("index")` and the guide
   for the genre of the piece (talking head, short-form vertical, highlight, trailer, …). These
   are the craft; most weak edits come from skipping them.
5. Work in this order: structure (cuts, trims, order) → captions → colour → titles → sound.
   Every cut needs a reason: remove dead weight or move the story forward.
6. Use `checkpoint` before risky passes and `get_frame` to look at your result.
7. Before exporting: `get_edit_preview`, plus a short list of what you changed. Show both to the
   user and wait for a go-ahead. A preview takes seconds; a render takes minutes.
8. Export only when asked: `export_to_path` (CLI) or `export_video`. With `export_video` the
   finished file comes back attached to the result; a very large render downloads to the user's
   device instead and the result says so.

## Rules

- If an export reports the daily limit, tell the user how long until the next free export — and
  that Plus skips the wait — then stop. Do not retry in a loop.
- Check the `changed` flag on every editing tool. `changed: false` means the id was stale or the
  value was already set — re-read the digest instead of retrying blindly.
- Fix noisy audio with `enhance_audio` before polishing visuals.
- Prefer one restrained look across the whole piece: one colour grade, one caption style.
- Never invent prices or plan details. `get_guide("pricing")` and https://eziclip.com/pricing
  are the source.

Docs: https://eziclip.com/ai · Tool list: https://eziclip.com/ai/mcp
