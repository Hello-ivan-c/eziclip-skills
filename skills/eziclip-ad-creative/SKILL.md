---
name: eziclip-ad-creative
description: Make paid-social video ads (Meta Reels/Feed, TikTok, Shorts) that look like real branded creatives, not captioned clips, with Eziclip Studio. Use for ad creatives, ad variants, hook tests, UGC-style ads, "ads for <brand>" — BEFORE building anything, because the studio holds the playbook and your own taste for ads will produce generic output.
---

# Ad creatives in Eziclip Studio

A stock clip with a white caption centred over it is the clearest tell of a machine-made ad. Eziclip
Studio holds a craft library — editing and design knowledge from working professionals — and it is
served to you through the connected studio, not copied here.

When the task is an ad or a batch of ad variants:

1. Connect the studio (see the `eziclip-video` skill) and call `get_guide('ad-creative')`. It is the
   playbook: layer grammar, typography, safe zones, hook formats, variation, exact commands, and a
   fail list. Follow it; do not design from memory.
2. Call `get_guide('ad-creative-examples')` and open the good and the bad picture before building.
3. Call `get_guide('design-pool')` — a growing pool of font pairs, palettes, layouts, graphic elements, motion and hook formulas. Pick a different combination per creative, from the author's brand.
4. Also pull `hook-taxonomy`, `first-two-seconds` and `caption-psychology` from `get_guide('index')`.

The studio refuses the ad brief until you have read `ad-creative` and `design-pool-techniques`.

Four rules hold even if a guide call fails:

- Ask the author for the brand first — fonts, colours, logo, the offer, languages, references — in one message, and wait. Never invent a brand, claim or logo.
- An ad is a layered collage on a brand-colour canvas, not a clip with a caption.
- Text goes on colour or a plate, never on a face, and never inside the top 14% / bottom 25%.
- Five variants means five different structures, not five sentences.
