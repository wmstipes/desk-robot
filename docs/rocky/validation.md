# Validation register

[Build overview](README.md) · [Hardware](hardware-and-interfaces.md) · [Full findings](reviews/integrated-systems-v0.2.md)

**Baseline imported 4 November 2026.** All 18 test IDs are carried over from the 25 October source review. Their recorded status remains **Not run / evidence not imported**. This does not assert that no experiments happened elsewhere. No test passes are created by this documentation change.

This file is the maintained public status register. The source review remains a dated snapshot. Close an R finding only when its closure condition has evidence, not merely because a task was discussed or code merged.

## Evidence and status rules

Allowed test states: **Not run**, **Blocked**, **In progress**, **Failed**, **Passed**. Record the execution date, source commit, device/firmware versions, hardware revision, connection layout, load/conditions, instrument/method, observed results with units, and sanitized evidence link. State reviewer and retest triggers. Missing measurements are unknown, never zero. Hardware or enclosure changes can invalidate a previous result.

Do not post personal recordings, faces, credentials, private hostnames/IP addresses, receipt details or paid assets as public evidence. Use synthetic audio and images or de-identified summaries where possible. Retain private artifacts outside this repository.

**Safety:** begin with unpowered inspection. Use supported procedures, conservative limits, and qualified supervision where needed. Do not intentionally short energized rails or mechanically stall a motor to prove a protection feature. Support the head before removing torque. Do not hot-plug ribbons, GPIO adapters, or actuator wiring.

## Release gates

The first bench gate is T01-T06, executed incrementally rather than attaching every device at once. Lighting and one-joint prototypes follow their individual checks. No complete-body print release until electrical, motion/cable, thermal/acoustic, privacy, serviceability and fit blockers have traceable evidence. T18 is the release review, not a substitute for earlier tests.

The 1-hour sensor test, 20% steady-headroom target and 5,000-cycle cable screen are **project proposals**, not vendor certifications, universal safe limits, or guaranteed service life. Approve the actual conditions before execution.

## Test records

### T01 — Unpowered inspection (Power/harness)

**Status:** Not run / evidence not imported.

**Procedure:** Check labels, pin-1 alignment, voltage/polarity, no exposed shorts, proper standoffs and physical support.

**Acceptance:** No ambiguous connection or loose conductive part; do not power motors yet.

**Findings:** R05; R13; R30.

**Execution date / revision / measurements / evidence / reviewer:** Pending.

### T02 — Pi baseline (Computer)

**Status:** Not run / evidence not imported.

**Procedure:** Boot exact SD image with cooler/Ethernet, confirm supply capability, log undervoltage/throttle status.

**Acceptance:** Stable boot and repeatable baseline before adding peripherals.

**Findings:** R02; R21; R29.

**Execution date / revision / measurements / evidence / reviewer:** Pending.

### T03 — Face/camera baseline (Head electronics)

**Status:** Not run / evidence not imported.

**Procedure:** Test landscape face, dimming, autofocus, simultaneous camera and display, latches and short bench cable.

**Acceptance:** No video errors, tension or glass compression; 1:1 bezel mockup accepted.

**Findings:** R03; R22; R26.

**Execution date / revision / measurements / evidence / reviewer:** Pending.

### T04 — Sensor bus (I2C)

**Status:** Not run / evidence not imported.

**Procedure:** Add sensors individually, then run all at 100kHz initial setting with production-length wiring.

**Acceptance:** Correct expected addresses; no recurring I2C errors over 1-hour combined activity test (project target).

**Findings:** R17; R20.

**Execution date / revision / measurements / evidence / reviewer:** Pending.

### T05 — Sensor validity (Environmental)

**Status:** Not run / evidence not imported.

**Procedure:** Log temperature/humidity/pressure/gas indices; compare ambient reference with Pi/LEDs off vs active.

**Acceptance:** Record bias and stabilization; revise pocket if electronics heat dominates readings.

**Findings:** R18; R20.

**Execution date / revision / measurements / evidence / reviewer:** Pending.

### T06 — Audio bench (Audio)

**Status:** Not run / evidence not imported.

**Procedure:** Use exact circular USB firmware; prove processed capture and playback through same Flex.

**Acceptance:** Correct channels, intelligible speech, no clipping or unexpected resets.

**Findings:** R11; R28.

**Execution date / revision / measurements / evidence / reviewer:** Pending.

### T07 — Power budget (All rails)

**Status:** Not run / evidence not imported.

**Procedure:** Measure voltage and simultaneous current with face bright, camera active, voice loud enough, LEDs worst allowed, both joints loaded.

**Acceptance:** Known rail/connector ratings respected; 20% steady headroom is proposed design target, not a substitute for transient testing.

**Findings:** R02; R04; R05.

**Execution date / revision / measurements / evidence / reviewer:** Pending.

### T08 — One light cartridge (Lighting)

**Status:** Not run / evidence not imported.

**Procedure:** Run independent pixels with final harness; test data direction, diffuser, dim amber, startup and full allowed test pattern.

**Acceptance:** No backfeed, no exposed hot spot, uniform look, brightness ceiling persists after reset.

**Findings:** R04; R09; R24.

**Execution date / revision / measurements / evidence / reviewer:** Pending.

### T09 — One joint dummy load (Motion)

**Status:** Not run / evidence not imported.

**Procedure:** Measure mass/CG and ribbon drag; test one actuator at gentle limits under representative load.

**Acceptance:** Stable hold, acceptable heat/noise/current, no contact with hard stops in normal operation.

**Findings:** R03; R07; R08.

**Execution date / revision / measurements / evidence / reviewer:** Pending.

### T10 — Motion fault injection (Safety)

**Status:** Not run / evidence not imported.

**Procedure:** Remove Spark link, stop Pi application, interrupt USB, remove motor power and reboot separately.

**Acceptance:** Commands expire, fault does not auto-resume, head remains physically supported.

**Findings:** R06; R07; R08; R27.

**Execution date / revision / measurements / evidence / reviewer:** Pending.

### T11 — Cable endurance screening (Neck)

**Status:** Not run / evidence not imported.

**Procedure:** Cycle bounded pan/tilt with final cable type, supports and display/camera active.

**Acceptance:** Project screening: 5,000 representative cycles with zero intermittent faults or visible damage; not a lifetime certification.

**Findings:** R03.

**Execution date / revision / measurements / evidence / reviewer:** Pending.

### T12 — Enclosed acoustic/thermal (Body)

**Status:** Not run / evidence not imported.

**Procedure:** Run combined workload in temporary representative enclosure at actual desk, including motors and fan.

**Acceptance:** No thermal throttling/undervoltage; no speech failure attributable to fan, vibration or enclosure echo.

**Findings:** R10; R18; R21; R28.

**Execution date / revision / measurements / evidence / reviewer:** Pending.

### T13 — Sensor optical isolation (Optics)

**Status:** Not run / evidence not imported.

**Procedure:** Vary face and inlay brightness under fixed room lighting; test chosen proximity window.

**Acceptance:** No self-amplifying brightness loop; stable proximity without internal reflections.

**Findings:** R19; R24.

**Execution date / revision / measurements / evidence / reviewer:** Pending.

### T14 — Local AI / network (Software)

**Status:** Not run / evidence not imported.

**Procedure:** Run reproducible Spark services, then block internet while keeping LAN and cold-start both devices.

**Acceptance:** Conversation/visual request works locally; authenticated encrypted connection; stale images/audio cleared.

**Findings:** R14; R16; R29.

**Execution date / revision / measurements / evidence / reviewer:** Pending.

### T15 — Conversation interruption (Audio software)

**Status:** Not run / evidence not imported.

**Procedure:** After turn-taking works, interrupt during speech from representative desk positions.

**Acceptance:** Stops old speech, retains intended user turn, no speaker-triggered loop.

**Findings:** R15; R28.

**Execution date / revision / measurements / evidence / reviewer:** Pending.

### T16 — Privacy / restart (Privacy)

**Status:** Not run / evidence not imported.

**Procedure:** Test mute/shutter/disconnect states and reconnect repeatedly, including fresh boot.

**Acceptance:** No recording/stream claimed disabled when active; no stale action or hidden recording.

**Findings:** R12; R16; R31.

**Execution date / revision / measurements / evidence / reviewer:** Pending.

### T17 — Fit / maintenance (CAD)

**Status:** Not run / evidence not imported.

**Procedure:** Fit critical mounts, connectors, nuts, diffuser and service panels; inspect motion swept volumes.

**Acceptance:** No collisions, blocked latch/tool access or trapped cables; head removable independently.

**Findings:** R01; R23; R25.

**Execution date / revision / measurements / evidence / reviewer:** Pending.

### T18 — Print release (Release)

**Status:** Not run / evidence not imported.

**Procedure:** Choose finish; inspect solid/manifold geometry, tolerances, powder escape, fastener holes and revision labels.

**Acceptance:** All blockers closed with evidence; printer quote reviewed; no full-body order from concept art.

**Findings:** R01; R03; R25.

**Execution date / revision / measurements / evidence / reviewer:** Pending.
