# Rocky: Raspberry Pi 5 + Spark build

**Documentation baseline: 4 November 2026. Implementation and hardware validation remain separate work.**

This fork adapts Casey Grothaus's desk-robot concept to a stationary, solder-free companion with a Raspberry Pi body controller and a local NVIDIA Spark AI server. This change adds no Pi client, new firmware, deployed service, or validated enclosure.

> **Do not use the inherited ESP32 power circuit, pin assignments, servo settings, or assembly instructions to wire this Pi-based build.** The original code and instructions remain available as references, not as the installation guide for this hardware.

## Start here

| Document | Purpose |
|---|---|
| [Hardware and interfaces](hardware-and-interfaces.md) | Distinguishes confirmed procurement, selected directions, candidates, and unverified physical state. |
| [Integrated systems review v0.2](reviews/integrated-systems-v0.2.md) | Preserved 34 findings, closure conditions, 18 test procedures, and source index. |
| [Validation register](validation.md) | Version-controlled test status, evidence format, and release gates. |
| [Engineering backlog](backlog.md) | Seven issue-ready workstreams mapped to the audit IDs. |
| [Development and public-data rules](development.md) | Branch/PR workflow, upstream baseline, attribution, and privacy rules. |
| [Original project README](../../README.md#upstream-esp32-reference) | Casey's original instructions below the fork notice. |

## Goals and boundaries

The target is a plug-in desktop companion with expressive screen eyes, bounded pan/tilt, conversational audio, requested visual understanding, five environmental/interaction sensors, and two diffused front light bars. The gray body and amber accents are styling targets; generated artwork is not manufacturing geometry.

No user soldering, trace cutting, exposed mains wiring, battery, walking mechanism, extra AI accelerator, or powered USB hub is part of the initial baseline. Factory-terminated wiring, accessible controls, screw-fastened parts, service access, and external mains adapters are requirements. Final printing is gated on fit, power, thermal, acoustic, and cable tests.

## Proposed responsibility split

| Location | Responsibility | Implementation state in this documentation baseline |
|---|---|---|
| Moving head | DSI display, standard camera, lightweight carrier/covers, shutter provision | Hardware selected; enclosure and moving cable paths unqualified. |
| Pi in stationary body | Face rendering, audio I/O, camera ownership, sensor reads, local interaction state, motion arbitration, connection recovery | Pi-specific integration is not added by this change. |
| Motion subsystem | Two bounded joints, external motor power, position feedback where supported, local fault handling | U2D2/Power Hub and XL330/XC330 are candidates, not an approved motor purchase. |
| Lighting subsystem | Two diffused RGB inlays; amber default; local animation and brightness limits | Plasma 2350 W selected as a direction; LED modules/harness not selected. |
| Spark | Local transcription, conversation/visual inference, speech synthesis, explicitly approved editable memory | Exact models, environment, security, and end-to-end latency need validation. |
| Bench fixtures | Black HAT breakout, prototyping base, camera-only Arducam mount | Development aids, not mandatory final-body contents. |

The Spark requests high-level actions; it does not bypass local motor limits. One local motion arbiter reconciles tracking and gestures. Lighting and facial state must remain truthful when the Spark is unavailable. Raw recordings are not retained by default; account access and autonomous computer control are out of the initial scope.

## What this baseline does not prove

Procurement is not delivery inspection. Delivery is not electrical qualification. A supported interface is not a measured combined-load result. A servo's stall rating is not its continuous operating rating. Concept art is not collision-checked CAD. A documentation merge is not a hardware-test pass.

The imported review is dated 25 October 2026. No later physical results are imported here. "Not run" in that snapshot means no qualifying evidence was supplied with this baseline, not that experiments could not have occurred elsewhere. Update the register with dated evidence before changing a status.

## First implementation milestone

Establish a reproducible Pi baseline, then display/camera operation, sensor readings, and audio capture/playback. Prove one light cartridge and the local Pi/Spark conversation path. Measure power and the actual head before selecting final actuators or releasing CAD. See the backlog for acceptance criteria; do not bypass gates to match a concept image.

## Attribution

Based on [cgro00/desk-robot](https://github.com/cgro00/desk-robot), reference commit `a18454067e93fbe6cc2fb0f8bc3aa2c26ebc55e1`. Preserve the original [LICENSE](../../LICENSE), notices, and history. This fork's planned Pi/Spark architecture is distinct from the upstream ESP32 V2 build. No paid hardware bundle or third-party manufacturing asset is included in this documentation change.
