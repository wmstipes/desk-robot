# Engineering backlog

[Build overview](README.md) · [Validation](validation.md) · [Findings and source index](reviews/integrated-systems-v0.2.md)

These are **issue-ready work packages, not created GitHub Issues**. Repository Issues remained disabled when the baseline was prepared, and the integration rejected branch creation. No remote issue numbers are assigned here. Enable Issues with an authorized account, then create/deduplicate tasks and link them back to these stable IDs.

All work packages are Open; all findings R01-R34 remain open pending their own closure evidence. Grouping tasks does not weaken acceptance criteria. Documentation completion does not close hardware risks.

## WORK-01 — Reconcile inventory and validate power / solder-free harness

**State:** Open; not created as a GitHub issue.

**Findings:** R02, R04, R05, R13, R30, R33. **Tests:** T01, T02, T07.

### Acceptance criteria

- [ ] Inspect actual part labels and revisions; distinguish ordered, received, functioning and proposed hardware.
- [ ] Confirm Pi supply capability and measure whole-system and USB/fan loads without forcing a software override.
- [ ] Document voltage, polarity, connector family/pitch and ratings for every supply and harness; verify no-backfeed arrangement.
- [ ] Define motor protection/cutoff and retained mechanical support. Publish sanitized measurements; no energized short tests.

**Evidence:** Pending. Add sanitized measurements and exact revision/conditions; do not close from a documentation-only change.

## WORK-02 — Build and qualify the Pi client / local Spark integration

**State:** Open; not created as a GitHub issue.

**Findings:** R14, R16, R26, R27, R29, R31, R32. **Tests:** T03, T14, T16.

### Acceptance criteria

- [ ] Keep the ESP32 firmware intact and implement explicit Pi camera, face, audio, sensor and controller adapters.
- [ ] Use bounded image payloads, stable device identification, session IDs and a single local motion arbiter.
- [ ] Provide authenticated encrypted LAN communication, bounded queues and explicit no-cloud-default behavior.
- [ ] Cold-start with internet blocked and LAN available; record dependency versions, latency and memory; clear stale work on reconnect.

**Evidence:** Pending. Add sanitized measurements and exact revision/conditions; do not close from a documentation-only change.

## WORK-03 — Validate Flex audio, conversation interruption and privacy controls

**State:** Open; not created as a GitHub issue.

**Findings:** R11, R12, R15, R28, R31. **Tests:** T06, T12, T15, T16.

### Acceptance criteria

- [ ] Pin circular-array firmware, processed channel mapping and playback reference on the actual board.
- [ ] Validate speaker connector/load and conservative output limits with no clipping or resets.
- [ ] Prove ordinary turn-taking before interruption; cancel stale audio without self-triggering.
- [ ] Implement truthful software mute, a verified hard-disconnect method, camera shutter and repeatable restart behavior.

**Evidence:** Pending. Add sanitized measurements and exact revision/conditions; do not close from a documentation-only change.

## WORK-04 — Qualify the five-sensor bus and enclosure readings

**State:** Open; not created as a GitHub issue.

**Findings:** R17, R18, R19, R20. **Tests:** T04, T05, T13.

### Acceptance criteria

- [ ] Inspect GPIO/ribbon orientation unpowered; test sensors individually then together with final-length wiring.
- [ ] Check pull-ups, capacitance and error behavior; do not blindly cut resistors or assume addresses alone prove compatibility.
- [ ] Use manufacturer SGP41 compensation/index processing and mark invalid, warming or stale values.
- [ ] Qualify temperature bias, airflow, lux shielding and proximity window with other subsystems active.

**Evidence:** Pending. Add sanitized measurements and exact revision/conditions; do not close from a documentation-only change.

## WORK-05 — Select and validate the full-head neck / moving cables

**State:** Open; not created as a GitHub issue.

**Findings:** R03, R06, R07, R08, R27. **Tests:** T09, T10, T11.

### Acceptance criteria

- [ ] Measure complete head mass, center of gravity and cable drag before final servo selection.
- [ ] Prototype real CSI/DSI routing and qualify constrained travel without latch tension, pinches or sharp folds.
- [ ] Validate one joint with representative load and mode-specific limits; communication polling must not keep stale intent alive.
- [ ] Test process/network/USB/power loss separately; head remains supported and faults require deliberate re-enable.

**Evidence:** Pending. Add sanitized measurements and exact revision/conditions; do not close from a documentation-only change.

## WORK-06 — Build one diffused light cartridge with a verified prewired harness

**State:** Open; not created as a GitHub issue.

**Findings:** R04, R09, R19, R24. **Tests:** T08, T13.

### Acceptance criteria

- [ ] Select exact solder-free LED modules and DIN/DOUT chain or a tested independent-output implementation.
- [ ] Choose one Plasma power arrangement; no parallel 5V sources through ordinary powered USB.
- [ ] Validate left/right addressing, reset-to-off behavior, brightness ceiling and communication timeout.
- [ ] Test actual diffuser and mixing distance; no glare, obvious hotspots or optical feedback into sensors.

**Evidence:** Pending. Add sanitized measurements and exact revision/conditions; do not close from a documentation-only change.

## WORK-07 — Validate component CAD, acoustic/thermal packaging and print release

**State:** Open; not created as a GitHub issue.

**Findings:** R01, R10, R21, R22, R23, R25, R32, R34. **Tests:** T12, T17, T18.

### Acceptance criteria

- [ ] Model actual component envelopes with connected plugs, fasteners, motion sweeps and service/tool access.
- [ ] Reserve the intact microphone disk and correct acoustic ports; separate warm air from sensing and microphone cavities.
- [ ] Approve a 1:1 face mockup and fit coupons; keep the neck structurally independent of removable cosmetic panels.
- [ ] Select finish and manufacturing tolerances; validate RF, stability, thermal/acoustic behavior and release evidence before full-body printing.

**Evidence:** Pending. Add sanitized measurements and exact revision/conditions; do not close from a documentation-only change.
