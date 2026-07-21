# cfl — Chinese as a foreign language

<img src="brand/icon.png" alt="Chinese icon" width="64" align="right">

**Chinese as a Foreign Language** — a course in the making for the
[boulingua](https://github.com/boulingua) multilingual teaching platform by
S. Le Boulanger: free, openly-licensed materials for the German *Gesamtschule*
system and for independent learners.

> **Status — scaffold.** This repository is being set up; curriculum units,
> downloadable materials and audio will follow. The established sister sites show
> the shape of a finished course:
> [DaF](https://boulingua.github.io/daf/) ·
> [EFL](https://boulingua.github.io/efl/) ·
> [FLE](https://boulingua.github.io/fle/).

## About Chinese

Standard Mandarin; simplified Chinese characters (Hanzi) with Pinyin romanisation and the four tones.

## Planned structure

Curriculum-aligned units following the boulingua five-step model
(**Activate → Input → Practise → Apply → Reflect**), each providing:

- differentiated exercises with answer keys and teacher notes;
- downloadable **open-format** materials — editable **.odp** slide decks and
  printable **PDF** worksheets;
- **native-voice audio** for vocabulary, dialogues and texts, generated with the
  shared [audiogen](https://github.com/boulingua/audiogen) workflow;
- researched context from **openly-licensed / public-domain** sources only,
  correctly cited, with no copyrighted material reproduced.

Levels and tracks are **to be defined** (CEFR A1–C1 and/or grade-based tracks,
in line with the sister sites).

## Licensing

- **Content:** Creative Commons Attribution — CC BY 4.0 (to be confirmed on first release).
- **Code:** MIT (see [`LICENSE`](LICENSE)).

## Author

S. Le Boulanger — <https://github.com/boulingua>

## Signature colour & icon

This project's signature accent is **`#2334C7`** (light theme) / **`#7E89E7`** (dark theme), paired with the **pentagon** mark (`brand/icon.svg`). The accent is *flag-safe* — the hue does not appear in the Chinese flag — is distinct from every other boulingua language, and is kept clear of the boulingua hub blue. The whole colour system lives in the [boulingua hub](https://github.com/boulingua/website#per-language-accent-colours).

## Use of LLM tools

Portions of this project were prepared with assistance from large language model tooling for narrowly defined, non-authorial tasks: copyediting, prose smoothing, Markdown/LaTeX formatting, scaffolding of boilerplate files (CI configs, build scripts), code refactoring. The tools used were Chat AI, the LLM service of KISSKI (GWDG), and a self-hosted Mistral Small (24B, Apache-2.0) run locally via Ollama and the ollamar R package — local inference only, with no data sent to third parties for the self-hosted model.
