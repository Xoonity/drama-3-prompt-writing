---
name: drama-3-prompt-writing
description: Write and refine text prompts for Fish Audio Drama 3 TTS run locally (ComfyUI-Drama3 node): bracket directions, point and scoped tags, director's notes, multi-speaker dialogue with speaker tags.
---

# Drama 3 — prompt writing (local)

Scope: writing the text that goes into the **Drama 3 Generate** node (ComfyUI-Drama3, local open weights). No API, no code, no install. The output of this skill is always a ready-to-paste text prompt.

References: ComfyUI-Drama3 README, section "Tags and directions" (https://github.com/PedroMarinhoDev/ComfyUI-Drama3); full syntax in Appendix C of the Fish Audio Drama 3 technical report (https://github.com/fishaudio/fish-speech/blob/main/docs/assets/Fish_Audio_Drama_3_Technical_Report.pdf); Fish Audio Drama 3 guide (https://docs.fish.audio/features/text-to-speech/drama-3).

## Syntax supported by the local node

Directions are free natural language, not a fixed list. Any wording works.

| Control | How to write it | Example |
|---|---|---|
| Style prefix | `[style]` at the start of a line, describing the whole delivery | `[with bright eager excitement] We won!` |
| Point tag | `[tag]` exactly where a sound or pause lands | `Well [sighs] fine, you win. [laughs]` · `[pause]` · `[gasps]` · `[clears throat]` · `[shaky breath]` |
| Scoped tag | `<tag>…</tag>` around the words it shapes | `Promise me <whisper>you won't tell her</whisper>.` |
| Director's note | `Direction:` + note, then `Line:` + text on the next line | `Direction: icy calm, as if you already accepted it.` / `Line: So this is goodbye.` |
| Speaker | `<\|speaker:N\|>` at the start of a line, N = 0 to 15 | `<\|speaker:0\|> Ready?` / `<\|speaker:1\|> Always.` |
| Language hint | `[language: xx]` after a line; not spoken | `Welcome back to the show. [language: en]` |

Scoped tags documented by Fish Audio: `<whisper>`, `<emphasis>`, `<soft>`, `<fast>`, `<slow>`, `<stress>`, `<forceful>`. Prefer these; don't invent new scoped tags.

Directions can be written in a different language from the line (e.g. a French direction on an English line).

### Rules that are easy to get wrong

- **Round parentheses are spoken.** Never put stage directions in `( )`; use `[ ]`.
- **Speaker N = the voice in slot `voiceN`** of Drama 3 Generate. Always give the user the mapping speaker number → character → voice slot. Give every speaker a voice: a speaker without one invites voice swaps in long dialogues.
- **Long text is cut into ~300-byte chunks** (when `split_long_text` is on), never inside a tag. A `Direction:`/`Line:` pair must fit in one chunk: keep the line short, or tell the user to turn off `split_long_text`.
- **No relative notes** ("softer than before", "same as last time"): the node doesn't send previous takes, so the model can't compare. Describe the target delivery in absolute terms.
- **Don't open a line with a pause cue** like `[long pause]`, especially in solo takes (`speakers` = one take per line): the take may stay silent. Put the pause between phrases instead.
- **The reference clip sets the baseline.** A flat reference gives a flat take whatever the direction says. If the user wants a delivery far from the clip, suggest a more fitting reference rather than piling on directions.

### From Fish Audio docs, unverified in the local node

Flag these as experimental if you use them:
- Language switch on a phrase: `<french>…</french>`, `<spanish>`, `<german>`, `<italian>`, `<japanese>`, `<chinese>`, `<korean>`.
- Phoneme pinning: `<|phoneme_start|>EH1 N JH AH0 N IH0 R<|phoneme_end|>` (English CMU Arpabet, digit = stress; Chinese tone-number pinyin; Japanese romaji).

## How to write a good direction

- Brief it like a voice actor: emotion + intent + pacing + who the character is talking to. `[barely holding it together, forcing a smile]` beats `[sad]`.
- Be specific and actable: "dangerously quiet", "stumbling over words", "talking to a child who just woke up".
- One direction per change of delivery; start a new bracket when the delivery shifts mid-line.
- Point tags where a real performer would breathe, sigh or laugh; don't put one in every sentence.
- Scoped tags on a word or short phrase, not a whole paragraph.
- Choose the format by need: a style prefix for a quick tone; a `Direction:`/`Line:` pair when the note is long or describes subtext.

## Multi-speaker scenes

Write a readable version first, then the prompt.

Readable:
```text
Narrator: [calm, measured storyteller voice] The ship had been silent for three days.
Captain: [low, gravelly, tired but in control] Status report.
Engineer: [fast, nervous, stumbling over words] Uh, sir, the reactor is, it is not great.
Captain: [slow, dangerously quiet] Define "not great."
Engineer: [voice breaking, close to panic] It is going to <emphasis>blow</emphasis>.
```

Prompt for Drama 3 Generate (one line per turn):
```text
<|speaker:0|> [calm, measured storyteller voice] The ship had been silent for three days.
<|speaker:1|> [low, gravelly, tired but in control] Status report.
<|speaker:2|> [fast, nervous, stumbling over words] Uh, sir, the reactor is, it is not great.
<|speaker:1|> [slow, dangerously quiet] Define "not great."
<|speaker:2|> [voice breaking, close to panic] It is going to <emphasis>blow</emphasis>.
```
Mapping: speaker 0 = Narrator → voice0, speaker 1 = Captain → voice1, speaker 2 = Engineer → voice2.

When lines need to react to each other (interruptions, rising tension), mention the node's `speakers` setting: *one take per line* (default) keeps voices clean but lines don't hear each other; *hearing the line before* or *together* gives more natural reactions but voices may bleed.

## Output format

For every request, return:
1. The prompt in a code block, ready to paste into Drama 3 Generate.
2. For dialogues: the speaker → character → voice slot mapping.
3. Only if relevant, one or two notes (chunking risk, experimental tag used, reference-clip advice). No other commentary.

When the user gives a raw script, keep their words unchanged; add only directions and tags, unless they ask for a rewrite.
