# drama-3-prompt-writing

A Claude skill for writing text prompts for **Fish Audio Drama 3** run locally with the [ComfyUI-Drama3](https://github.com/PedroMarinhoDev/ComfyUI-Drama3) node.

It covers bracket directions, point and scoped tags, director's notes (`Direction:` / `Line:`), language hints and multi-speaker dialogue (`<|speaker:N|>`), plus the pitfalls of the local node (spoken parentheses, ~300-byte chunking, pause cues at line start, reference-clip baseline). No API, no code: the skill outputs ready-to-paste prompts.

Tutorial & Tests:
- [Same Scene, One Change: How to Direct Fish Audio Drama 3 in ComfyUI](https://www.youtube.com/watch?v=t0Sl48SH7dM)
- [Fish Audio Drama 3 : 4 Dialogue Modes Tested (ComfyUI Guide)](https://www.youtube.com/watch?v=Xc_wJaLJgjw)

## Install

**Claude (claude.ai / app)**: download [`drama-3-prompt-writing.zip`](https://github.com/Xoonity/drama-3-prompt-writing/releases/latest/download/drama-3-prompt-writing.zip), then upload it in **Customize → Skills** and turn it on.

**Claude Code, Cursor, Codex and other coding agents**

```bash
npx skills add https://github.com/Xoonity/drama-3-prompt-writing
```

Or manually for Claude Code:

```bash
git clone https://github.com/Xoonity/drama-3-prompt-writing
cp -r drama-3-prompt-writing/drama-3-prompt-writing ~/.claude/skills/
```

## Usage

Ask Claude for a Drama 3 prompt, e.g. *"Write a Drama 3 prompt for a tense two-character scene"* or paste your script and ask to add directions.

## Author

Made by [Cosmo Edge Labs](https://www.youtube.com/@CosmoEdgeLabs) — tutorials and tests on local AI models (ComfyUI, TTS, video).

## Sources

- [ComfyUI-Drama3 — Tags and directions](https://github.com/PedroMarinhoDev/ComfyUI-Drama3)
- [Fish Audio — Drama 3 guide](https://docs.fish.audio/features/text-to-speech/drama-3)
- [Fish Audio Drama 3 technical report](https://github.com/fishaudio/fish-speech/blob/main/docs/assets/Fish_Audio_Drama_3_Technical_Report.pdf) (Appendix C: full syntax)

## License

MIT. Not affiliated with Fish Audio. Drama 3 weights and outputs are under the Fish Audio Community License.
