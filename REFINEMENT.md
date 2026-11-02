# Refinement — 30 October 2026

Status: development baseline, not a teaching release.

## Audit and completed work

The repository's existing roadmap and brand assets were preserved. The course
now has an explicit `zh` target, German interface, pinned Hugo module,
working status/materials/legal navigation and an in-progress curriculum manifest.
This addresses the previous absence of a runnable course site. No dummy lesson,
exam result, audio approval or educational coverage was invented.

The 13 newly instantiated sites passed strict Hugo builds, local-link checks
and manifest resolution with the proposed kit/framework checkouts. Norwegian
and Dutch additionally had obsolete generated build outputs removed from Git's
index; local builds remain disposable. All 15 development courses passed the full local gate battery, including real Chromium keyboard/mobile and accessibility checks, with the proposed shared checkouts on 30 October 2026. This verifies the development shell, not teaching content.

## Priorities and completion criteria

1. Publish a coordinated dependency release after the shared release blockers
   are resolved. Local platform integration and browser checks are complete.
2. Prepare the roadmap's first complete teaching unit and assessment with a
   competent language reviewer. Validate source/rights, descriptor mapping,
   task/answer consistency and usable PDFs before expanding the sequence.
3. Review voice previews or arrange a documented human recording where audio
   is planned. A checksum or readable WAV does not establish pronunciation quality.
4. Complete the roadmap's coherent first learner level before changing the
   public suite status to active. Keep later levels explicitly in progress.

The repository's detailed roadmap supplies language-specific acceptance
criteria. Publication, native-language approval and hosted CI for these changes
have not occurred. Exact unblocked validation and remaining findings continue
in the suite refinement record.


## Analytics notice consistency — 31 October 2026

Plausible remains disabled. The privacy notice explicitly says so; copied claims of active Plausible collection were removed. This does not remove or make claims about separately documented hosting or other services.

Privacy pages declare their analytics policy, language and built URL. New kit gate C9 checks actual HTML scripts and visible notices against configuration; it rejects missing, duplicate or mismatched scripts and contradictory notices. Six gate regressions and three scaffold tests passed. Representative strict builds/C9 passed for website, nsf, nvt, gfl, Scriptlibrary and all three resource-hub notice languages; the suite integration run verifies the remaining sites. This is configuration/content evidence, not certification of the remote server or legal compliance.


## Explicit release dependencies — 31 October 2026

The shared workflow caller now names the same immutable org commit in `uses` and
`org_ref`; `release-pins.json` records the actual historical kit/framework pins.
These historical pins are explicitly not the remediated release. The new org
workflow rejects absent/malformed versions, mismatched source commits and schema
bytes, and nonconforming course manifests; hubs pin dependencies without claiming
course conformance. The coordinated updater prepares exact published foundation
sets, updates consumers/checksums together and retains rollback inputs. Separate
read-only candidate CI can validate immutable pre-release commits without release
tags or deployment rights. Hosted execution and new foundation publication remain
unobserved. See the org `docs/release-pins.md`.


## Local verification — 2 November 2026

The strict candidate-kit Hugo build and complete configured gate battery passed
locally. Declined gates, warning ramps and external checks remain listed in the
battery output; this is not hosted CI or deployment evidence. The historical
release pins have not been promoted to new foundation releases.
