# Development, provenance and publication rules

[Build overview](README.md) · [Backlog](backlog.md)

## Frozen starting point

- Fork: `wmstipes/desk-robot`.
- Upstream: `cgro00/desk-robot`.
- Reference commit: `a18454067e93fbe6cc2fb0f8bc3aa2c26ebc55e1`.
- Reference tree: `5bedc2c001ea365bf3790da6bddd24f539698017`.
- README blob before the fork notice: `280eba09574410a21e291c58d0d25567184c041f`.
- Upstream describes its ESP32 V2 build. This Pi/Spark documentation baseline is not a new upstream release.

The change prepends a fork notice and navigation link to the existing README. Original README instructions remain in place; `firmware/`, `server/`, original wiring/protocol documentation and `LICENSE` are not changed. Inherited instructions remain for their original hardware, not ours.

## Changes through branches and pull requests

Use small topic branches from the fork's current main and PRs targeting **wmstipes/desk-robot**, not Casey's repository. A documentation-only PR must not silently alter firmware, dependencies, cloud defaults, running hardware, repository security settings, or CI workflows. Keep application changes and engineering evidence reviewable independently.

Do not force-push or overwrite main to keep it aligned with upstream after custom work begins. Fetch upstream changes, inspect differences, reconcile the fork's README notice and hardware abstractions, and test on a separate integration branch. Update the recorded upstream baseline only when the comparison and acceptance evidence are reviewed.

A merge is not a deployment. An application test is not a physical fit or electrical qualification. Record actual code/firmware versions alongside hardware evidence.

## Attribution and third-party assets

Retain the original MIT license and attribution; do not rewrite history to remove contributors. Do not infer that a code license licenses purchased CAD, voice models, audio recordings, character artwork, model weights, fonts, or other assets. Verify each asset's redistribution terms before committing it. Paid hardware-bundle files are not included in this baseline.

## Public repository hygiene

Never commit real API keys, tokens, Wi-Fi credentials, environment files, private hostnames/network addresses, receipts, delivery details, raw microphone recordings, private camera images, chat histories, or personal memory databases. Review file contents, Git diffs and archive/office metadata before publishing. Use synthetic examples and sanitized evidence summaries; keep private supporting files outside the repository.

A `.gitignore` is not a security boundary and does not remove data already tracked. This documentation-only change does not claim to install a secret scanner or enforce retention controls. Preserve existing example-file patterns; evaluate actual configuration defaults in the implementation workstream.

## Evidence updates

Keep the v0.2 review as a historical snapshot and update the maintained validation register with a dated, scoped result. Link source commit, device/firmware revisions, test conditions, measurements, evidence, reviewer and invalidating changes. When evidence changes the architecture, update the hardware/interface register and relevant risk closure condition in the same PR. Mark uncertainty explicitly; do not convert an estimate into a measured result.

## First repository administration follow-up

Issues must be enabled separately by an authorized account. The preparation session could read the repository but branch creation returned `403 Resource not accessible by integration`; no remote settings or branch was changed. The supplied package includes a guarded local publishing helper. Do not request or paste access tokens into chat. Configure your own trusted Git/GitHub CLI authentication normally.

Once enabled, use the seven backlog work packages as starting issue bodies, search for duplicates first, and replace their pending references with actual issue numbers. Do not automatically create 34 duplicate issues solely because the audit has 34 findings.
