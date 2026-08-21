# CFL — Chinese course roadmap

> Repository: `cfl` · Language: Standard Mandarin Chinese · Signature accent `#2334C7` (light) / `#7E89E7` (dark) · Pentagon mark · Status: **coming soon → building**.
> This roadmap turns the empty `cfl` scaffold (LICENSE + README + brand icons) into a complete boulingua course — content *and* site — conforming to the `kit` platform, the `curriculum` framework, and the VG Wort standard.

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

- **Script variant → Simplified (简体).** *Recommended.* Matches Mainland/HSK usage, the largest learner base, and the only licence-clear Piper `zh_CN` voice. Traditional is deferred (a possible future `cfl-tw` sibling or an appendix conversion table), not interleaved.
- **Standard variety → Standard Mandarin (Putonghua).** Beijing-based pronunciation, neutral register. No regional topolects (Cantonese/Min etc.) in scope.
- **Romanisation → Hanyu Pinyin with diacritic tone marks** (mā/má/mǎ/mà), never tone numbers in running prose. Numbered Pinyin (ma1) allowed only in machine-facing keys/filenames. Neutral tone unmarked. Every new Hanzi item is introduced as a `汉字 (pīnyīn) — Gloss` triple.
- **Directionality → LTR.** Modern horizontal Simplified Chinese is left-to-right; **no RTL work**, no vertical typesetting. This is a genuine simplification vs a hypothetical Arabic/Hebrew course — note it and move on.
- **Web font → bundle Noto Sans SC (subset), self-hosted.** *Recommended.* SIL OFL (redistribution-safe under the repo's licensing), broad Simplified coverage, clean tone-mark rendering. Added to the kit's `fonts.yaml` manifest, which `build_fonts.py` (F3) subsets and emits the `@font-face` and `font-display: swap` declarations for; **the subset is driven by the actually-used character set** (units drive coverage) to keep the payload sane. Not a course-side file — a course repo has no `assets/css/` and no `static/fonts/`. Do **not** pull from a CDN — keep the site self-contained and GitHub-Pages-static. The accent/pentagon system in `data/accents.yaml` is untouched (already carries `cfl`).
- **Native-voice TTS → Piper `zh_CN-chaowen-medium` (CC0).** *Available, and the licence position decides it.* `zh_CN-huayan-medium`, recommended in an earlier draft of this document, states `License: Unknown` on its model card and cannot ship — an unstated licence is not permission, and audio synthesised from a model inherits its training data's terms, so it cannot sit inside a CC BY-SA 4.0 course. `zh_CN-xiao_ya-medium` is explicitly **non-commercial** and is excluded for the same reason. `chaowen` is CC0 and carries no obligation. the kit's `audio/build_audio.py` already takes `<voice.onnx> <lang>`; invoke it with the `zh` language argument. `chaowen` is also the **only** licence-clear Mandarin speaker in the catalogue, so single-voice dialogues with speaker labels are the plan, not a stopgap — add a second voice only if a second licence-clear `zh_CN` voice lands. Verify tone rendering by ear on a sample before batch-generating. Audio is generated **locally and committed** (CI never runs Piper).
- **Voice IDs are never hand-typed.** The ID, tier and licence above are read from `kit/audio/voices.yml`, which is the single source of truth and must never be hand-copied into prose and keeps the rejected `huayan`/`xiao_ya` rows with their licences so the decision stays auditable. A voice ID retyped from memory is how a 404 gets written into a download script.
- **Level-0 script onboarding → YES, mandatory.** A dedicated **Pre-A1 "起步 / Erste Schritte"** stage (Pinyin system, the four tones + neutral, tone sandhi basics, pronunciation, stroke logic *conceptually*, radicals as reading aids, ~150 highest-frequency characters). **Stroke-order animations/practice are out of scope for v1** (documented as a known gap; a static stroke-order appendix is the fallback).

## 3. Standing up the site on the kit

Stand up the buildable site first (before authoring content). Nothing shared is copied — `cfl` imports it.

1. **Create the repo around the kit.** `cfl` holds a short `hugo.toml` (baseURL, title, languageCode, `[params]`, menus) whose `[module]` block carries `[[module.imports]] path = "github.com/boulingua/kit"`, a `go.mod`/`go.sum` requiring `github.com/boulingua/kit` at the pinned tag (`v1.0.0`), `boulingua.yml` (the per-course config the gate battery reads), the twelve-line `.github/workflows/deploy.yml` calling the org's reusable workflow, an empty `content/` skeleton and the legal pages — preserving the existing `cfl` LICENSE, README, and `brand/` history. `layouts/`, `assets/`, `archetypes/`, `i18n/` and `scripts/` are **not** copied and must never appear here: they are the drift surface, and a file that is not in the repo cannot fork. Hugo pulls the first four from the module; CI checks the kit out at the pinned tag so `scripts/` and the gate battery sit on disk before Hugo runs. Never track `public/`.
2. **Vendor `_materials/`.** Run `kit materials sync` — it assembles the kit's `latex/`, `fonts/` and `brand/icons/` into the flat `_materials/` tree the LaTeX build expects — and commit it. This is the only vendored surface, because XeLaTeX cannot read a Hugo module; it is hash-gated against `kit.lock`, so an edited file fails the build instead of forking quietly. (Earlier drafts told you to copy `_materials/` from the template; `pagegen` never had one.)
3. **Edit `hugo.toml` marked values only:**
   - `baseURL = "https://boulingua.github.io/cfl/"`
   - `title = "Chinese — S. Le Boulanger"`
   - `languageCode = "de"`, `defaultContentLanguage = "de"` (instruction language is German)
   - `[params].navTitle = "Chinese 中文"`, `description`, `keywords = "Chinese,Mandarin,Hanzi,Pinyin,HSK,CEFR,OER,Gesamtschule"`
   - `params.code = "cfl"` (selects the `#2334C7` accent from the kit's `data/accents.yaml`). Setting it is not optional: the kit's default is the neutral graphite `template` accent, so a course that forgets looks obviously unconfigured rather than looking like DaF.
   - `[params.plausible].domain = "boulingua.github.io/cfl"` (keep this block **last** — the TOML sub-table trap)
   - `[[params.social]].url` → the `cfl` GitHub repo
   - `[[menu.main]]` sections → mirror `content/`: Level 0 (Script), A1, A2, B1; Materials (Slide decks / Worksheets); About; Legal.
4. **Regenerate the pentagon.** Run the kit's `brand/make_icon.py` so this repo's `icon.svg`/`icon.png`/favicons derive from `cfl`'s accent. Confirm the icon reads clean at 64px next to CJK.
5. **Confirm the kit's `data/accents.yaml` carries `cfl`** — it already does (`code: cfl`, `accent: "#2334C7"`, `hover: "#1C2AA0"`). The file arrives with the module; no CSS edits, and no `data/accents.yaml` in this repo.
6. **First green build.** `hugo --minify --gc --panicOnWarning` locally, then push to `main` → `deploy.yml` builds under the same flags, runs the gate battery and GitHub Pages publishes. Keep the world-map tile on *coming-soon* until §8's done-criteria are met.
7. **Fill the three legal pages** (`impressum` / `datenschutz` / `haftungsausschluss`) ⟨…⟩ placeholders, and add the VG Wort METIS disclosure to `datenschutz`. Remove `|| true` from the legal-placeholder gate once filled.

## 4. Curriculum conformance target

- **Declared level: `core` (A1–B1).** Publish `conformance.yml` (modelled on `curriculum/examples/de-a1/`) naming `framework: boulingua-curriculum`, `framework_version`, `language: zh`, `declared_conformance: core`.
- **In-scope scales implemented** (every in-scope scale that has an official A1/A2/B1 descriptor): the Reception, Production, and Interaction activity scales (overall oral/reading comprehension, spoken production/interaction, conversation, information-exchange, obtaining goods & services, correspondence, notes/messages/forms, online interaction), the core LING scales (**phonological-control weighted heavily** for tones; orthographic-control reframed as *character/Pinyin control*), SOC sociolinguistic-appropriateness, and PRAG fluency/coherence.
- **Declared `no-official-descriptor`** where the CV genuinely has no cell at these levels (many strategy and mediation scales at A1; PLUR partly), recorded per cell — a silently missing scale is a conformance failure, an explicitly declared empty cell is not.
- **Mediation** implemented lightly at A2/B1 (relaying specific information, acting as intermediary in informal situations) since German↔Chinese mediation is natural in a German-medium course; deeper mediation declared as gap.
- **Mapping plan.** Each unit's front-matter `curriculum` block (`framework: cefr`) lists `implements` descriptor IDs `{LEVEL}.{DOMAIN}.{SCALE}.{SEQ}` drawn from `curriculum/levels/*.md`. A machine-readable **scope/coverage manifest** (scales × levels: implemented vs `no-official-descriptor`) is committed.
- **Gate.** The reusable workflow `boulingua/.github/.github/workflows/course-build.yml@v1` runs `python .curriculum/scripts/conformance_audit.py resolve --manifest conformance.yml --content content` — format, global uniqueness, every `implements` resolving to a real in-scope (scale, level) **in this repo**. `id-audit.sh` audits the framework's *own* level files and **cannot** validate this repo. Do not wire it here.
- **HSK crosswalk (documentation, not a substitute for IDs).** Publish an appendix table; indicative HSK 3.0 alignment: HSK 1–2 → A1, HSK 3 → A2, HSK 4 → B1 (with the caveat that HSK 3.0 is heavier than old HSK). CEFR IDs remain the conformance backbone; HSK is a learner-facing signpost only.

## 5. Content creation plan

Recurring cast & theme: a small **recurring cast** (a German exchange student 安娜 Ānnà, a Chinese classmate 小明 Xiǎomíng, family & shopkeepers) threads dialogues across levels for continuity. Exams are **first-class sibling bundles** (`…-exam/`), never sub-pages. Effort tags: **S** ≤1 day · **M** ~2–4 days · **L** ~1 week+.

- **Phase 0 — Script onboarding (Pre-A1, "起步"), ~4–5 units.** Pinyin & syllable structure; the four tones + neutral + basic sandhi; pronunciation drills; radicals for reading; ~150 high-frequency characters; numbers/dates. Acceptance: learner can read/produce any Pinyin syllable with correct tone, recognise the seeded characters, and self-check with committed audio. **Effort: L** (foundational, audio-heavy).
- **Phase 1 — A1, ~12 units.** Greetings, self/family, nationality, numbers/money, food & ordering, time & daily routine, directions, shopping, classroom language. 1–2 module exams + A1 final. Acceptance: every unit maps to A1 descriptor IDs; committed deck + worksheet + answer key + audio; the §4 gate resolves at A1. **Effort: L.**
- **Phase 2 — A2, ~12 units.** Past experiences, travel & transport, health, hobbies, describing people/places, comparisons, appointments, simple correspondence (WeChat-style messages). Module exams + A2 final. **Effort: L.**
- **Phase 3 — B1, ~10–12 units.** Opinions & preferences, work/study plans, narrating events, culture & festivals, problem-solving transactions, short argument, light mediation (relaying German↔Chinese). Module exams + B1 final. **Effort: L.**
- **Appendices (S–M each):** Pinyin & tone guide, tone-sandhi reference, radical/component list, character-frequency list, measure-words table, grammar summary, HSK↔CEFR crosswalk, glossary (Hanzi/Pinyin/German), assessment grid, Simplified↔Traditional conversion note.
- **Editorial (M):** About this course, Get started (how to use tones/Pinyin/audio).

Per-phase acceptance criteria (all must hold): green `hugo` build; the §4 conformance gate resolves at that level; each unit has committed deck + worksheet + answer key + audio + thumbnail; every editorial page ≥1800 chars carries a registered Zählmarke; tone marks render correctly in HTML and PDF.

## 6. Website & materials

- **Section landings via shortcodes** (never raw HTML) for Level 0/A1/A2/B1 and the Materials hub, mirroring `content/materials/_index.md`.
- **Materials pipeline.** Generate branded decks (`.odp`) and printable **PDF** worksheets from the kit's LaTeX templates (`kit/latex/`, vendored into `_materials/`) via `scripts/build_materials_latex.py`; **commit** under `static/materials` + `static/downloads/<level>/`. CI only *verifies* presence + attribution (`verify_downloads.py`) — no TeX Live in the deploy path. LaTeX must use a CJK-capable engine/font (XeLaTeX/LuaLaTeX + Noto Sans SC) so Hanzi and tone marks render.
- **Native-voice audio.** `build_audio.py … <chaowen .onnx> zh 'content/**/units/*/index.md'` → OGG/Opus, committed; the `.onnx` path comes from the `cfl` row in `kit/audio/voices.yml`, never typed in by hand. Voice only teaching units (skip exams). **Decision: Mandarin audio ships on `zh_CN-chaowen-medium` (CC0)** — the licence-clear voice, not the one an earlier draft named (§2). Single-voice with typographic speaker labels is the shipping form; dialogue alternation is added only if a second **licence-clear** `zh_CN` voice lands.
- **Thumbnails** via `scripts/render_thumbs.py`; declared in unit front-matter `presentation`/`worksheet` `{ file, thumbnail }`.
- **Downloads** surfaced per unit and aggregated on the Materials hub.

## 7. VG Wort — pixel assignment for ALL content pages (required, non-skippable)

Every content page that is an original creative text **≥1800 rendered characters** gets exactly **one** VG Wort Zählmarke, per `kit/docs/vgwort-standard.md`:

- **Draw fresh public codes** (32-hex Öffentlicher Identifikationscode) from the author's **T.O.M.** account — one per work, never invented, never reused across pages. Private codes never appear in the repo.
- **Register** each in `data/vgwort.yaml` keyed by `url:` (base-stripped `RelPermalink`) or `path:`, with `public_id` / `pixel_url` / `token`, `min_chars: 1800`, `author`, `registered_at`.
- **Render** solely through the shared resolver (`layouts/_partials/vgwort/url.html`) → `<head>` preload + eager body `<img>` (both driven by the resolver; `loading="eager"`, off-screen not `display:none`, no JS, no consent gate, direct `met.vgwort.de`).
- **Record** each mark in the private **usage registry** (§8 of the standard — kept outside the repo): Used / Projekt=cfl / Sprache=zh / Niveau / Kurstitel / URL / Pixel_URL.
- **Gates:** coverage audit (warning) shows **0** unregistered editorial pages (legal pages excepted); render-verify (blocking) confirms each `pixel_url` appears on exactly one URL; hub guard confirms `met.vgwort.de` is **absent** from `/materials/`. Exclude `vgwort.de` from the link-checker.
- **Excluded (never marked):** home, materials hub, tag/level indexes, paginated continuations, and the three templated legal pages.

**Estimated total marks needed:** ~**58–65** Zählmarken — ≈40 units + ≈10 exams + ≈10 appendices + ≈2 editorial pages that clear the 1800-char Mindestumfang (a few short Level-0 script drills may fall below and take none). Draw ~65 codes to leave headroom for splits.

## 8. Milestones & sequencing

1. **M0 — Scaffold live (site, no content).** §3 complete: `cfl` builds green on the kit module with `_materials/` vendored, pentagon regenerated, CJK font bundled, legal pages filled, Pages deploying. *Dependency: none.* **~1 week.**
2. **M1 — MVP: Script onboarding + first A1 unit live.** Phase 0 unit(s) + one full A1 unit with committed deck/worksheet/audio/thumbnail, first Zählmarken registered, conformance + VG Wort gates green. **This is the earliest publishable slice.** *Depends on M0, Piper voice, font subset.* **~3–4 weeks.**
3. **M2 — A1 complete** (12 units + exams + core appendices, full A1 descriptor coverage). **~2–3 months.**
4. **M3 — A2 complete.** **~2–3 months.**
5. **M4 — B1 complete + `core` conformance manifest finalised + all appendices.** **~2–3 months.**

**Definition of "done / ready to flip from coming-soon to active on the world map":** Level 0 + A1 fully published (all units with committed materials + audio + thumbnails), `conformance.yml` declaring at least honest A1 coverage, validated by the §4 gate, every editorial page ≥1800 chars carrying a registered Zählmarke with all VG Wort gates green, legal pages filled (placeholder gate enforced), and a clean production build on GitHub Pages. Full A1–B1 `core` conformance is the completion target; the flip can happen at M2 (A1 done) with A2/B1 marked in-progress.

## 9. Open decisions & risks

- **Simplified-only vs future Traditional sibling** — confirm before authoring (retrofitting Traditional is costly). *Recommendation: Simplified-only now, Traditional deferred.*
- **Font payload** — full Noto Sans SC is large; the subsetting-at-build step is essential and must not break when new characters are added. Risk: un-subset font bloats first paint.
- **Tone-mark fidelity across the whole chain** — HTML, `.odp`, PDF (XeLaTeX), and Piper audio must all agree. Risk: silent tone-mark corruption; mitigate with a per-phase spot-check.
- **HSK 3.0 vs 2.0 crosswalk drift** — HSK 3.0 rebalanced levels; the crosswalk is indicative, not authoritative. State the caveat prominently.
- **Piper Mandarin quality** — `zh_CN-chaowen-medium` is the sole licence-clear Mandarin voice (§2), so there is no second option to fall back to on quality: if tones come out wrong on rare syllables the ladder goes straight to transcript-only for the affected items. Ear-verify before batch generation; keep a manual-correction path (per-segment respelling).
- **Stroke order out of scope for v1** — learners may expect it; document the gap and the static-appendix fallback.
- **German-medium instruction with Chinese target** (`languageCode = de`) — ensure i18n strings, `dateFormat`, and SEO metadata stay German while target-language content renders CJK correctly.
- **Character-frequency vs thematic ordering tension** — pedagogy wants themes, character economy wants frequency; resolve per level (frequency-led at Level 0/A1, theme-led thereafter).
