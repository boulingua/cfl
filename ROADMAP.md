# CFL — Chinese course roadmap

> Repository: `cfl` · Language: Standard Mandarin Chinese · Signature accent `#2334C7` (light) / `#7E89E7` (dark) · Pentagon mark · Status: **coming soon → building**.
> This roadmap turns the empty `cfl` scaffold (LICENSE + README + brand icons) into a complete boulingua course — content *and* site — conforming to `pagegen`, the `curriculum` framework, and the VG Wort standard.

## 1. Overview

**What this course is.** A free, openly-licensed *Chinese as a Foreign Language* course for the German *Gesamtschule* system and for independent adult learners, built on the boulingua five-step unit model (Activate → Input → Practise → Apply → Reflect). Metalanguage of instruction is **German** (glosses, rubrics, teacher notes); the target language is **Standard Mandarin** in **Simplified characters + Hanyu Pinyin + tone marks**.

**Realistic CEFR scope.** Ship **Pre-A1 (script onboarding) → A1 → A2 → B1**. Declare **`core`** conformance (A1–B1). B2/C1 are explicitly *out of scope for v1* and declared as gaps — freely-available B2+ Mandarin material with full mediation coverage is not realistic for a one-author OER, and the framework expects instantiations to declare gaps honestly rather than fake them.

**Why Chinese.** It is the highest-demand non-European language on the platform, and it stress-tests the boulingua standard against a **non-Latin, tonal, character-based** language — proving the template generalises beyond the Latin-script trio (efl/fle/daf).

**The 3–5 defining challenges (language-specific):**
1. **Triple orthography.** Every item carries three coordinated layers — Hanzi, Pinyin, and tone marks — that must render, align, and stay in sync in prose, decks, worksheets, and audio.
2. **Simplified vs Traditional** is a first-class editorial decision, not a font toggle (different characters, not just glyphs).
3. **Tones are meaning-bearing.** Phonological control (LING.phonological-control) is load-bearing from lesson one; audio and tone-mark fidelity are non-negotiable.
4. **HSK ≠ CEFR.** The dominant Chinese proficiency scale (HSK) must be crosswalked to CEFR descriptor IDs, not substituted for them.
5. **CJK web typography.** The hugo-coder theme ships Latin webfonts only; a CJK-covering, openly-licensed font must be added without bloating the page or breaking the accent/design system.

## 2. Language & localisation decisions

Concrete, opinionated recommendations — one per axis:

- **Script variant → Simplified (简体).** *Recommended.* Matches Mainland/HSK usage, the largest learner base, and Piper's `zh_CN` voice. Traditional is deferred (a possible future `cfl-tw` sibling or an appendix conversion table), not interleaved.
- **Standard variety → Standard Mandarin (Putonghua).** Beijing-based pronunciation, neutral register. No regional topolects (Cantonese/Min etc.) in scope.
- **Romanisation → Hanyu Pinyin with diacritic tone marks** (mā/má/mǎ/mà), never tone numbers in running prose. Numbered Pinyin (ma1) allowed only in machine-facing keys/filenames. Neutral tone unmarked. Every new Hanzi item is introduced as a `汉字 (pīnyīn) — Gloss` triple.
- **Directionality → LTR.** Modern horizontal Simplified Chinese is left-to-right; **no RTL work**, no vertical typesetting. This is a genuine simplification vs a hypothetical Arabic/Hebrew course — note it and move on.
- **Web font → bundle Noto Sans SC (subset), self-hosted.** *Recommended.* SIL OFL (redistribution-safe under the repo's licensing), broad Simplified coverage, clean tone-mark rendering. Add via `assets/css/custom.css` `@font-face` with `font-display: swap`; **subset to the actually-used character set** at build time (units drive coverage) to keep payload sane. Do **not** pull from a CDN — keep the site self-contained and GitHub-Pages-static. The accent/pentagon system in `data/accents.yaml` is untouched (already carries `cfl`).
- **Native-voice TTS → Piper `zh_CN-huayan-medium`.** *Available and recommended.* `scripts/build_audio.py` already takes `<voice.onnx> <lang>`; invoke with `voices/zh_CN-huayan-medium.onnx zh`. Use a second voice for dialogue alternation if/when a second openly-licensed `zh_CN` voice lands; until then single-voice dialogues are acceptable. Verify tone rendering by ear on a sample before batch-generating. Audio is generated **locally and committed** (CI never runs Piper).
- **Level-0 script onboarding → YES, mandatory.** A dedicated **Pre-A1 "起步 / Erste Schritte"** stage (Pinyin system, the four tones + neutral, tone sandhi basics, pronunciation, stroke logic *conceptually*, radicals as reading aids, ~150 highest-frequency characters). **Stroke-order animations/practice are out of scope for v1** (documented as a known gap; a static stroke-order appendix is the fallback).

## 3. Instantiation from pagegen

Stand up the buildable site first (before authoring content):

1. **Copy the template.** Seed `cfl` working tree from `pagegen` (layouts, archetypes, scripts, `data/`, `brand/`, `.github/workflows/build-deploy.yml`, `go.mod`, i18n) — preserving the existing `cfl` LICENSE, README, and `brand/` history. Never track `public/`.
2. **Edit `hugo.toml` marked values only:**
   - `baseURL = "https://boulingua.github.io/cfl/"`
   - `title = "Chinese — S. Le Boulanger"`
   - `languageCode = "de"`, `defaultContentLanguage = "de"` (instruction language is German)
   - `[params].navTitle = "Chinese 中文"`, `description`, `keywords = "Chinese,Mandarin,Hanzi,Pinyin,HSK,CEFR,OER,Gesamtschule"`
   - `params.code = "cfl"` (selects the `#2334C7` accent)
   - `[params.plausible].domain = "boulingua.github.io/cfl"` (keep this block **last** — the TOML sub-table trap)
   - `[[params.social]].url` → the `cfl` GitHub repo
   - `[[menu.main]]` sections → mirror `content/`: Level 0 (Script), A1, A2, B1; Materials (Slide decks / Worksheets); About; Legal.
3. **Regenerate the pentagon.** Run `python brand/make_icon.py` so `icon.svg`/`icon.png`/favicons derive from `cfl`'s accent. Confirm the icon reads clean at 64px next to CJK.
4. **Confirm `data/accents.yaml` carries `cfl`** — it already does (`code: cfl`, `accent: "#2334C7"`, `hover: "#1C2AA0"`). No CSS edits.
5. **First green build.** `hugo --minify --gc` locally, then push to `main` → the `build-deploy.yml` gate battery runs and GitHub Pages publishes. Keep the world-map tile on *coming-soon* until §8's done-criteria are met.
6. **Fill the three legal pages** (`impressum` / `datenschutz` / `haftungsausschluss`) ⟨…⟩ placeholders, and add the VG Wort METIS disclosure to `datenschutz`. Remove `|| true` from the legal-placeholder gate once filled.

## 4. Curriculum conformance target

- **Declared level: `core` (A1–B1).** Publish `conformance.yml` (modelled on `curriculum/examples/de-a1/`) naming `framework: boulingua-curriculum`, `framework_version`, `language: zh`, `declared_conformance: core`.
- **In-scope scales implemented** (every in-scope scale that has an official A1/A2/B1 descriptor): the Reception, Production, and Interaction activity scales (overall oral/reading comprehension, spoken production/interaction, conversation, information-exchange, obtaining goods & services, correspondence, notes/messages/forms, online interaction), the core LING scales (**phonological-control weighted heavily** for tones; orthographic-control reframed as *character/Pinyin control*), SOC sociolinguistic-appropriateness, and PRAG fluency/coherence.
- **Declared `no-official-descriptor`** where the CV genuinely has no cell at these levels (many strategy and mediation scales at A1; PLUR partly), recorded per cell — a silently missing scale is a conformance failure, an explicitly declared empty cell is not.
- **Mediation** implemented lightly at A2/B1 (relaying specific information, acting as intermediary in informal situations) since German↔Chinese mediation is natural in a German-medium course; deeper mediation declared as gap.
- **Mapping plan.** Each unit's front-matter `curriculum` block (`framework: cefr`) lists `implements` descriptor IDs `{LEVEL}.{DOMAIN}.{SCALE}.{SEQ}` drawn from `curriculum/levels/*.md`. A machine-readable **scope/coverage manifest** (scales × levels: implemented vs `no-official-descriptor`) is committed and kept passing under `curriculum/scripts/id-audit.sh` (format, global uniqueness, every `implements` resolves to a real in-scope (scale, level)). Add a per-course `verify_cefr.py` opt-in gate to the workflow.
- **HSK crosswalk (documentation, not a substitute for IDs).** Publish an appendix table; indicative HSK 3.0 alignment: HSK 1–2 → A1, HSK 3 → A2, HSK 4 → B1 (with the caveat that HSK 3.0 is heavier than old HSK). CEFR IDs remain the conformance backbone; HSK is a learner-facing signpost only.

## 5. Content creation plan

Recurring cast & theme: a small **recurring cast** (a German exchange student 安娜 Ānnà, a Chinese classmate 小明 Xiǎomíng, family & shopkeepers) threads dialogues across levels for continuity. Exams are **first-class sibling bundles** (`…-exam/`), never sub-pages. Effort tags: **S** ≤1 day · **M** ~2–4 days · **L** ~1 week+.

- **Phase 0 — Script onboarding (Pre-A1, "起步"), ~4–5 units.** Pinyin & syllable structure; the four tones + neutral + basic sandhi; pronunciation drills; radicals for reading; ~150 high-frequency characters; numbers/dates. Acceptance: learner can read/produce any Pinyin syllable with correct tone, recognise the seeded characters, and self-check with committed audio. **Effort: L** (foundational, audio-heavy).
- **Phase 1 — A1, ~12 units.** Greetings, self/family, nationality, numbers/money, food & ordering, time & daily routine, directions, shopping, classroom language. 1–2 module exams + A1 final. Acceptance: every unit maps to A1 descriptor IDs; committed deck + worksheet + answer key + audio; passes id-audit. **Effort: L.**
- **Phase 2 — A2, ~12 units.** Past experiences, travel & transport, health, hobbies, describing people/places, comparisons, appointments, simple correspondence (WeChat-style messages). Module exams + A2 final. **Effort: L.**
- **Phase 3 — B1, ~10–12 units.** Opinions & preferences, work/study plans, narrating events, culture & festivals, problem-solving transactions, short argument, light mediation (relaying German↔Chinese). Module exams + B1 final. **Effort: L.**
- **Appendices (S–M each):** Pinyin & tone guide, tone-sandhi reference, radical/component list, character-frequency list, measure-words table, grammar summary, HSK↔CEFR crosswalk, glossary (Hanzi/Pinyin/German), assessment grid, Simplified↔Traditional conversion note.
- **Editorial (M):** About this course, Get started (how to use tones/Pinyin/audio).

Per-phase acceptance criteria (all must hold): green `hugo` build; `id-audit.sh` passes; each unit has committed deck + worksheet + answer key + audio + thumbnail; every editorial page ≥1800 chars carries a registered Zählmarke; tone marks render correctly in HTML and PDF.

## 6. Website & materials

- **Section landings via shortcodes** (never raw HTML) for Level 0/A1/A2/B1 and the Materials hub, mirroring `content/materials/_index.md`.
- **Materials pipeline.** Generate branded decks (`.odp` via slidegen) and printable **PDF** worksheets (sheetgen) from the LaTeX templates via `scripts/build_materials_latex.py`; **commit** under `static/materials` + `static/downloads/<level>/`. CI only *verifies* presence + attribution (`verify_downloads.py`) — no TeX Live in the deploy path. LaTeX must use a CJK-capable engine/font (XeLaTeX/LuaLaTeX + Noto Sans SC) so Hanzi and tone marks render.
- **Native-voice audio.** `scripts/build_audio.py … voices/zh_CN-huayan-medium.onnx zh 'content/**/units/*/index.md'` → OGG/Opus, committed. Voice only teaching units (skip exams). **Decision: Mandarin Piper is available**; single-voice acceptable for v1, dialogue alternation added if a second `zh_CN` voice lands.
- **Thumbnails** via `scripts/render_thumbs.py`; declared in unit front-matter `presentation`/`worksheet` `{ file, thumbnail }`.
- **Downloads** surfaced per unit and aggregated on the Materials hub.

## 7. VG Wort — pixel assignment for ALL content pages (required, non-skippable)

Every content page that is an original creative text **≥1800 rendered characters** gets exactly **one** VG Wort Zählmarke, per `pagegen/docs/vgwort-standard.md`:

- **Draw fresh public codes** (32-hex Öffentlicher Identifikationscode) from the author's **T.O.M.** account — one per work, never invented, never reused across pages. Private codes never appear in the repo.
- **Register** each in `data/vgwort.yaml` keyed by `url:` (base-stripped `RelPermalink`) or `path:`, with `public_id` / `pixel_url` / `token`, `min_chars: 1800`, `author`, `registered_at`.
- **Render** solely through the shared resolver (`layouts/_partials/vgwort/url.html`) → `<head>` preload + eager body `<img>` (both driven by the resolver; `loading="eager"`, off-screen not `display:none`, no JS, no consent gate, direct `met.vgwort.de`).
- **Record** each mark in the private **usage registry** (§8 of the standard — kept outside the repo): Used / Projekt=cfl / Sprache=zh / Niveau / Kurstitel / URL / Pixel_URL.
- **Gates:** coverage audit (warning) shows **0** unregistered editorial pages (legal pages excepted); render-verify (blocking) confirms each `pixel_url` appears on exactly one URL; hub guard confirms `met.vgwort.de` is **absent** from `/materials/`. Exclude `vgwort.de` from the link-checker.
- **Excluded (never marked):** home, materials hub, tag/level indexes, paginated continuations, and the three templated legal pages.

**Estimated total marks needed:** ~**58–65** Zählmarken — ≈40 units + ≈10 exams + ≈10 appendices + ≈2 editorial pages that clear the 1800-char Mindestumfang (a few short Level-0 script drills may fall below and take none). Draw ~65 codes to leave headroom for splits.

## 8. Milestones & sequencing

1. **M0 — Scaffold live (site, no content).** §3 complete: `cfl` builds green from `pagegen`, pentagon regenerated, CJK font bundled, legal pages filled, Pages deploying. *Dependency: none.* **~1 week.**
2. **M1 — MVP: Script onboarding + first A1 unit live.** Phase 0 unit(s) + one full A1 unit with committed deck/worksheet/audio/thumbnail, first Zählmarken registered, `id-audit` + VG Wort gates green. **This is the earliest publishable slice.** *Depends on M0, Piper voice, font subset.* **~3–4 weeks.**
3. **M2 — A1 complete** (12 units + exams + core appendices, full A1 descriptor coverage). **~2–3 months.**
4. **M3 — A2 complete.** **~2–3 months.**
5. **M4 — B1 complete + `core` conformance manifest finalised + all appendices.** **~2–3 months.**

**Definition of "done / ready to flip from coming-soon to active on the world map":** Level 0 + A1 fully published (all units with committed materials + audio + thumbnails), `conformance.yml` declaring at least honest A1 coverage validated by `id-audit.sh`, every editorial page ≥1800 chars carrying a registered Zählmarke with all VG Wort gates green, legal pages filled (placeholder gate enforced), and a clean production build on GitHub Pages. Full A1–B1 `core` conformance is the completion target; the flip can happen at M2 (A1 done) with A2/B1 marked in-progress.

## 9. Open decisions & risks

- **Simplified-only vs future Traditional sibling** — confirm before authoring (retrofitting Traditional is costly). *Recommendation: Simplified-only now, Traditional deferred.*
- **Font payload** — full Noto Sans SC is large; the subsetting-at-build step is essential and must not break when new characters are added. Risk: un-subset font bloats first paint.
- **Tone-mark fidelity across the whole chain** — HTML, `.odp`, PDF (XeLaTeX), and Piper audio must all agree. Risk: silent tone-mark corruption; mitigate with a per-phase spot-check.
- **HSK 3.0 vs 2.0 crosswalk drift** — HSK 3.0 rebalanced levels; the crosswalk is indicative, not authoritative. State the caveat prominently.
- **Piper Mandarin quality** — single voice, possible tone artefacts on rare syllables; ear-verify before batch generation; keep a manual-correction path.
- **Stroke order out of scope for v1** — learners may expect it; document the gap and the static-appendix fallback.
- **German-medium instruction with Chinese target** (`languageCode = de`) — ensure i18n strings, `dateFormat`, and SEO metadata stay German while target-language content renders CJK correctly.
- **Character-frequency vs thematic ordering tension** — pedagogy wants themes, character economy wants frequency; resolve per level (frequency-led at Level 0/A1, theme-led thereafter).
