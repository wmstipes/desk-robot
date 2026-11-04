> **Public documentation import — 4 November 2026.** The review below is the supplied v0.2 snapshot, dated 25 October 2026. Its 34 findings and 18 procedures are preserved; no new physical tests or live deployments were performed for this import. Procurement statements describe the evidence available to that review, not current delivery/condition verification. This document is not a wiring authorization, safety certification, or manufacturing release.
>
> The maintained public status is in [the validation register](../validation.md); [hardware status](../hardware-and-interfaces.md) and [backlog](../backlog.md) distinguish decisions from proposals. No receipts, private network data, recordings, workbook metadata, or paid CAD assets are attached. Source URLs are retained for revision-specific rechecking, not represented as freshly revalidated here.
>
> **Test safety clarification:** references to protection checks do not authorize deliberately shorting live power rails, stalling motors, or defeating protections. Use manufacturer procedures and appropriately protected test equipment. Disable and mechanically support motion before wiring changes.

# Rocky integrated systems review — v0.2
**Review date:** 25 October 2026
**Decision:** Continue staged bench development. Do not release the final enclosure for manufacture or enable a complete moving head before closing the fit, power and neck-validation gates.

## Executive finding
The architecture is coherent. Retain the Pi 5, standard Camera Module 3, color DSI display, reSpeaker Flex, enclosed speaker baseline and five selected sensors. Use one motion architecture and one lighting architecture. The new controllers do not justify replacing the core electronics.

The evidence does **not** establish a fully integrated, safe, print-ready robot. Available body assets are generated concept images and the v0.1 mechanical plan, not a dimensioned assembly. No physical current, temperature, mass, center-of-gravity, acoustic, torque, cable-endurance or collision tests were performed in this review.

This report records 34 findings: several documented interface constraints, several design omissions, and several tests still to perform. They are **not 34 observed failures**. The original workbook was the private editable companion; the public validation register tracks subsequent evidence without publishing that workbook.

## 1. Baseline and procurement
The reviewed baseline is a stationary, mains-adapter-powered desktop companion. No walking, battery, AI accelerator, extra computing HAT or powered USB hub is required.

Order confirmations establish Pi 5 4GB, Active Cooler, 256GB microSD, Camera Module 3 Standard, Waveshare 2.8-inch DSI display, two 300mm camera adapters, five environmental/interaction sensors, one QT adapter, and eight QT cables. The ordered Pi power adapter is the Waveshare 27W SKU 1876-1, not the official Pi supply named in the original starter workbook. PiShop's later order includes both the assembled Mini Black HAT Hack3r and a 2x20 socket riser. Personal billing/shipping information is intentionally excluded.

The Seeed audio order and Arducam camera mount were confirmed by the user. Exact delivered audio revision and speaker plug are not inspected. Plasma 2350 W is the selected lighting direction, not a confirmed purchase in the evidence reviewed. U2D2/Power Hub and XL330/XC330 remain proposals; no servo size or final motor supply has been approved.

The old starter workbook is historical planning, not the current purchased inventory. The public hardware register separates these states explicitly.

## 2. Retained architecture
| Location | Responsibilities |
|---|---|
| Head | DSI display, standard camera, lightweight carrier and covers, shutter provision |
| Neck | Bounded pan/tilt, supported tilt pivot, measured load, guarded cable loops |
| Torso | Pi/cooler, USB audio processor, microphone cassette, speaker, sensors, motion interface, lighting controller, protected low-voltage wiring |
| Spark | Local transcription, conversational/visual models, speech generation and explicit editable memory |
| External | Mains adapters; router/switch; Spark |
| Bench only by default | Phase Dock, Black HAT breakout and camera-only Arducam pan/tilt mechanism |

The Pi owns local interaction state, motion arbitration and safety limits. The Spark may request a named gesture but does not own low-level actuator timing. Lighting and face animation must continue to show truthful local state when Spark is unavailable.

USB allocation can be Flex, proposed U2D2, and Plasma for a low-power wired test, leaving one of the four Pi USB sockets free. Sufficient sockets do not imply sufficient current. The Black HAT duplicates existing GPIO signals rather than adding independent GPIO or power capacity. [S01, S12]

The Pi's sensor bus and Plasma's Qw/ST sensor socket are not interchangeable host links. Do not connect the two powered sensor buses together as a shortcut. Keep current sensors on the Pi; control Plasma over a deliberately implemented USB or Wi-Fi protocol. [S07, S08, S13]

## 3. Electrical decisions that must be fixed before enclosure release
### Shared USB power
Pi 5 has a 1.6A combined USB/fan allowance with a recognized 5A supply; the allowance is normally lower without that recognition. Display/camera and sensor loads also count against overall Pi supply capacity, but not as USB peripherals. Confirm the actual adapter and negotiated capability instead of relying on the '27W' label or forcing a software override. [S01]

As a reference only, eight LEDs budgeted at 60mA each require 0.48A and 2.4W at 5V. This leaves 1.12A of a 1.6A allowance before Flex, fan, U2D2 logic and Plasma overhead. That is not a measured Rocky load, and modern pixels may differ. No measured-current values were supplied with this review; they remain unknown. [S22]

### Plasma backfeed risk
Sheet 1 of Pimoroni's Plasma 2350 W schematic joins USB VBUS and the 5V screw terminal to the same net. Treat them as one power domain, not isolated inputs. Do not attach an external 5V source to the terminal while a normal powered USB cable is connected to the Pi. [S08]

Use either USB-only power for a small, limited bench lighting load, or a suitable separate USB-C supply with local Wi-Fi control. A wired, separately powered data arrangement would need a verified manufactured isolation/power-selection solution; a homemade split cable is not the baseline.

### Motion power
A USB motion interface does not supply motor current. The proposed U2D2 Power Hub accepts one external source at a time. Its DC jack is 5.5mm outside / 2.5mm inside, center-positive: do not assume the earlier 2.1mm adapter suggestion fits. The hub accepts a broad voltage range but does not make a 12V supply appropriate for a low-voltage servo. [S10]

For the XL330-M288 candidate, 5V is recommended and the supported supply range is 3.7–6V. Its 5V stall current is 1.47A; two yield a 2.94A fault-planning sum, not a normal operating prediction. Final wire, connector, protection and supply ratings follow the selected actuator and measured load. [S09]

Keep motor current off the QT chain and Pi USB rail. Do not parallel independent positive supply rails. Grounds and return paths must follow each actual interface design; 'separate power' does not imply galvanic isolation. Use enclosed external mains adapters and only secured, protected low-voltage wiring inside Rocky.

## 4. Motion review
Smart serial servos remain a worthwhile direction because they expose feedback. They are not yet approved actuators for this head. Pololu Maestro is an alternative architecture, not another required board.

Two subtle software conditions matter. The XL330 watchdog responds to missing instruction packets, so status polling can keep it alive even when higher-level intent is stale. Its min/max position registers are not enforced in current-based position mode. Therefore implement an application-level command-age limit, one local motion arbiter, explicit mode checks and a deliberate re-enable procedure. Start with bounded position operation; review any later mode change separately. [S09]

Balance matters before torque. A hypothetical 0.300kg head with a 0.030m center-of-gravity offset produces 0.0883Nm of static gravity torque, excluding acceleration and cable resistance. That is a calculation example, not the measured head.

Provide two-sided tilt support, hard physical travel containment beyond the normal software range, accessible motor cutoff, pinch guards and mechanical support on power loss. A head that falls when torque is removed is not made safe by a stop button. Do not use the camera-only Arducam kit as evidence of final full-head capacity.

Provision smart servos individually, establish unique IDs and verify direction/offset before attaching the full head. Do not hot-plug actuator or ribbon wiring. Limit initial movement severely and extend only after clearance and cable tests.

## 5. Body design disposition
The dark gray shell, black bezel, warm amber eyes and two recessed front light bars remain the styling baseline. Neck and ear rings remain passive orange trim initially. Ignore inconsistent blue/white concept posters as construction instructions.

The earlier body envelope (head 105–115mm wide; torso 155–170mm wide by 140–155mm deep; total 210–230mm high) is a packaging target only. There is no validated evidence that all components, plugs and air passages fit within it. A slightly wider or deeper removable base is preferable to compressing wires or blocking acoustic/air paths.

A 2.8-inch 4:3 active image is approximately 57 by 43mm in landscape by calculation. The external module is larger. Make a 1:1 face mockup: the rendering cannot be used to make the screen magically occupy most of a much wider head. [S02]

### Revised section allocation
| Section | Required contents and access |
|---|---|
| Head carrier | Display and camera fixed independently of cosmetic shells; bezel must not clamp glass; camera optical opening and shutter |
| Rear head cover | Removable access to both ribbon latches and camera mounting hardware |
| Neck / rear cable cover | Separate relaxed cable paths for pan and tilt; strain relief only in appropriate stationary regions; no twisting flat cables into a tight bundle |
| Upper-front torso | Stationary microphone cassette with unobstructed paths to actual ports |
| Rear/right torso | Pi, cooler, connected plugs and a purposeful intake/exhaust path |
| Front torso | Factory-enclosed speaker in removable cradle, proximity insert, two diffused light cartridges |
| Lower side pockets | Temperature/humidity isolated from warm interior; separate gas pocket and vented pressure area |
| Upper outer shoulder | Lux sensor with opaque separation from face, light bars and board indicators |
| Lower/rear tray | Motion interface/hub, Plasma, protected harness, cutoff and service access |
| Bottom plate | Structural frame anchorage, feet, secured ballast only if needed, maintenance access |

The circular microphone array is a 73mm disk with bottom-firing ports, not a ring through which a neck can pass. Preserve the entire board and align openings with the ports. Acoustic seals/supports must not block those ports or conduct excessive motor/speaker vibration. Both Flex boards and their supplied microphone FPC remain stationary. [S05, S06]

Plasma's documented 61×22×12mm assembly does not include USB-plug withdrawal space and wire bends. Model it on an accessible tray, not buried below the neck. U2D2 documentation shows connector-revision variation; use the actual unit for USB cutouts. [S07, S11]

The generated giant speaker, paired camera-like lower 'sensors', arbitrary rear ports, metallic texture and brass insert renderings are not hardware requirements. Keep the aesthetic, but replace these with truthful component geometry. Use captive nuts/machine screws instead of heat-installed inserts. Nylon PCB standoffs are not automatically structural neck fasteners.

## 6. Cable and connector review
The included 120mm DSI cable is documented for initial display setup, not proven to route from the torso through both neck joints. The 300mm camera cable likewise has no qualified repeated-flex life in the reviewed evidence. Camera and display adapter wiring differ. [S02, S28]

Before final neck CAD: make a routing jig with the exact ribbons, supported at their connectors, and keep camera/display active during controlled movement. Avoid sharp folds, torsion, rubbing against printed layers and tension at the latches. Bounded movement only; no continuous spin. A longer cable is not automatically reliable or dynamically rated.

Create a harness schedule containing both ends, pin numbers, voltage, current rating, connector family/pitch, wire size, length and strain relief. 'JST' or 'three pin' alone does not identify compatibility. This schedule is required to keep the build genuinely solder-free.

Design clearance around plugs in the inserted, moving and removal states. Preserve microSD service, Ethernet latch access, USB servicing, fan replacement and access to ribbon latches. Keep power/motor wiring away from sensitive microphone and long sensor paths where practical, then test rather than promise immunity.

## 7. Sensors and bus validation
The selected default I2C addresses are SHT45 0x44, BMP581 0x47, VEML7700 0x10, VCNL4200 0x51 and SGP41 0x59. Their addresses are distinct. This verifies address selection, not the complete electrical bus. [S23–S27]

Five breakouts plus the Pi adapter and a ribbon extension add cable capacitance and potentially parallel pull-ups. TI documents both minimum pull-up resistance (sink-current constraint) and maximum resistance (rise-time constraint). Start with short wiring and a 100kHz bus for validation. A lower clock is not a remedy for excessively strong pull-ups. If errors appear, measure the bus; do not cut resistors to guess at a fix. A preassembled isolating switch/buffer could be justified later, but is not an automatic extra purchase. [S14]

SHT45 needs a small, ventilated room-air pocket with thermal separation from the Pi, amplifier, LEDs and SGP heater. Compare readings with and without internal loads; do not merely subtract an arbitrary offset. Keep the pressure opening out of the fan jet. Validate lux with Rocky's lights on/off and test the proximity sensor behind the actual chosen opening/window. [S15]

SGP41 needs its manufacturer algorithm and correct temperature/humidity compensation and timing. Raw ticks are not VOC or NOx indices, much less CO2 concentration. Check unit conversion, initialization/conditioning and invalid/stale behavior. These are room-condition observations, not smoke/CO alarms or assurances that air is safe. [S16]

## 8. Lighting review
Use a small number of pixels in two stationary, removable cartridges. Prototype one cartridge before fixing aperture dimensions. A separate frosted diffuser, mixing gap, opaque backing and accessible retainer create the effect; gray PA12 alone is not the light diffuser.

The earlier M5Stack U003 suggestion was not a verified six-pixel two-bar harness. Its documented Grove pins provide GND, 5V, signal and NC. Manufacturer daisy-chain claims do not establish a preterminated DOUT connection for our particular build. Splitting a single data input normally mirrors the first pixel sequence; it does not create independently addressed sequential segments. Choose factory-wired DIN/DOUT modules or validate a second supported output before purchase. [S21]

Plasma firmware must implement its own local command protocol, brightness ceiling, startup-off state and command-expiry fallback. This is custom software, not a supplied Rocky feature. Do not let network reconnection replay old flashes. Mute/privacy indications have priority over decorative effects, and a light alone cannot prove physical microphone disconnection.

## 9. Audio, cooling and local AI
Retain the Flex and Seeed speaker for baseline testing. Verify circular-array USB firmware and channel mapping. Capture from the processed path and send playback through the intended same audio device so its echo processing sees the reference. Reject the assumption that a more expensive driver fixes clipping, enclosure leakage or fan/motor noise. A 5W speaker must not be treated as a 10W load because of the amplifier's headline rating. [S04, S29]

Acoustic tests must include fan, motors, lights, keyboard noise, actual seating distance and representative head positions. A quiet open-bench conversation does not validate the enclosed robot.

Casey's v2 protocol is a useful reference, but it is not our completed Pi client: it waits for playback completion before unmuting, uses unencrypted WebSockets, and caps messages at 1MB. Our Pi needs a hardware abstraction layer, bounded image payloads, local capture ownership, truthful privacy state and explicit interruption/cancellation behavior. [S17]

Use one camera capture service to supply low-resolution tracking and requested compressed stills. Do not open the camera independently from multiple services or stream every full-resolution frame to a language model. Freeze/test the software on the actual Pi OS and Spark ARM64/CUDA environment; an example Mac dependency file is not deployment validation. NVIDIA's Spark inference workflow is a supported starting point, not a guarantee for the complete voice stack. [S18, S19]

Use stable USB device identification, local supervised services, session IDs, bounded queues and explicit health status. Disconnects must clear old speech/motion commands. Require local network authentication/encryption, no public port forwarding and no raw recording retention by default. Verify offline operation by cold-starting with internet blocked but LAN available.

## 10. Release decisions
Proceed with baseline Pi, screen/camera and sensor testing; continue microphone/speaker tests once the exact hardware is available. Use the Black HAT as the bench access tool it was purchased to be.

Do not order the finished body, finalize the full-head actuator, or claim full-duplex speech until the corresponding gates pass. No further core-controller, camera, sensor or HAT swap is warranted by this review.

The truly missing items are the exact motor/supply/protection harness, exact prewired LED assemblies and diffusers, verified final DSI/ribbon route, accessible privacy/motion controls, a dimensioned enclosure assembly and test evidence. A multimeter and an appropriate current-measurement method are useful validation tools; any inline USB-C instrument must preserve the required power-delivery behavior. A cheap meter can itself alter the test.

Shapeways' PA12 tolerance and clearance depend on finish. Its listed dimensional rule includes ±0.15mm plus ±0.15% of the longest axis (100mm example: ±0.30mm). Use fit coupons for mounts, fasteners, seams and diffusers, and then validate closed solids, wall thickness, powder access, units, finish and quote. This review supplies no manufacturing-ready geometry. [S20]

## 11. Immediate work sequence
1. Record actual part labels/revisions and photographs; inspect unpowered wiring.
2. Prove Pi baseline, display and camera; then all five sensors.
3. Prove Flex capture/playback and one lighting cartridge independently.
4. Measure combined main/USB power and temperatures before designing around a supply assumption.
5. Build a real-size head carrier/dummy, measure mass/CG and qualify ribbon routing.
6. Test one candidate joint under representative load with explicit fault handling.
7. Build a representative acoustic/thermal packaging mockup.
8. Close CAD/service/fastener checks and print small coupons before full body release.

The underlying goal is unchanged: a friendly, private, expressive gray-and-amber companion. The next improvement is integration evidence, not more unrelated hardware.

## Appendix A — Findings and closure conditions

### R01 — No dimensioned final assembly exists
**Priority:** Blocker. **Type:** Confirmed document gap. **Status:** Open.

Concept renders and v0.1 prose cannot prove collision-free fit.

**Correction:** Create component-envelope assembly including plugs, service space, fasteners and motion sweeps.

**Close when:** CAD model and physical fit coupons agree.

**Basis:** Local v0.1.

### R02 — Combined power budgets unmeasured
**Priority:** Blocker. **Type:** Untested integration. **Status:** Open.

USB+fan allowance is separate from overall Pi supply; audio and lighting compete.

**Correction:** Measure complete main/USB load and boot transients; confirm PD before enclosure.

**Close when:** Rail budgets and combined-load test pass with deliberate margin.

**Basis:** S01.

### R03 — Head load, neck and moving ribbons unqualified
**Priority:** Blocker. **Type:** Untested integration. **Status:** Open.

No measured head mass, CG, ribbon drag or life evidence.

**Correction:** Build balanced head dummy and routing jig before actuator / shell freeze.

**Close when:** One joint passes load, cable and safe-power-off tests.

**Basis:** Local v0.1; S28.

### R04 — Plasma USB and terminal 5V share a rail
**Priority:** High. **Type:** Documented wiring. **Status:** Open.

External terminal power plus normal USB can couple two sources.

**Correction:** Use USB-only low-load test OR separate USB-C supply and Wi-Fi; no parallel positive rails.

**Close when:** Power-off source tests show no unintended feed into Pi.

**Basis:** S08.

### R05 — Wrong supply or barrel adapter possible
**Priority:** High. **Type:** Documented interface. **Status:** Open.

Low-voltage XL330 needs 3.7–6V; hub accepts higher inputs. Hub jack is 5.5/2.5mm.

**Correction:** Specify exact regulated supply and harness; label polarity and voltage; select one hub input.

**Close when:** Independent polarity/voltage/connector check before servo connection.

**Basis:** S09; S10.

### R06 — Communication watchdog is not command-age watchdog
**Priority:** High. **Type:** Documented behavior + inference. **Status:** Open.

Status polling can keep bus active while stale motion logic survives.

**Correction:** Local motion arbiter, command expiry, mode limits, watchdog and deliberate re-enable.

**Close when:** Kill network / process / USB independently; verify appropriate stop behavior.

**Basis:** S09.

### R07 — Current-position mode bypasses position limit registers
**Priority:** High. **Type:** Documented behavior. **Status:** Open.

Smart-servo modes do not all use the same travel limits.

**Correction:** Start with bounded position mode; re-review safety before current-position mode.

**Close when:** Mode-specific test confirms no commanded motion beyond physical safe envelope.

**Basis:** S09.

### R08 — Torque loss can drop an unbalanced head
**Priority:** High. **Type:** Design hazard. **Status:** Open.

A cutoff prevents drive but does not support weight.

**Correction:** Balance head; mechanical support; covered pinch regions; stable base.

**Close when:** Power removal at allowed poses causes no damaging drop, pinch or tip.

**Basis:** Engineering requirement.

### R09 — Two independent LED bars lack verified harness
**Priority:** High. **Type:** Design omission. **Status:** Open.

U003 connector lists signal input and NC, not a plug-ready data output.

**Correction:** Choose prewired DIN/DOUT chain or validated two-output solution; no bare-strip solder pads.

**Close when:** Left/right independent effect test using final factory-terminated harness.

**Basis:** S21.

### R10 — Microphone ports and neck volume not resolved
**Priority:** High. **Type:** Documented geometry + omission. **Status:** Open.

73mm full circular board and bottom ports need their own bay; cannot surround neck through a hole.

**Correction:** Reserve unobstructed microphone cassette separate from fan and speaker.

**Close when:** Acoustic tests pass with final openings, head poses and fan speeds.

**Basis:** S06.

### R11 — Speaker load, output level and connector not validated
**Priority:** High. **Type:** Untested integration. **Status:** Open.

Selected 5W speaker not permission to use 10W amplifier maximum.

**Correction:** Verify factory plug/pinout; conservative gain/limiter; no amplifier chaining.

**Close when:** Clear sustained speech without clipping, excessive heat, buzz or USB reset.

**Basis:** S04; S29.

### R12 — No verified physical microphone mute implementation
**Priority:** High. **Type:** Design omission. **Status:** Open.

Flex reset/boot is not a dedicated privacy switch.

**Correction:** Provide accessible hard-disconnect method, truthful software mute, camera shutter.

**Close when:** No capture/stream under declared mute; hardware claims proved electrically.

**Basis:** S04.

### R13 — No complete solder-free harness schedule
**Priority:** High. **Type:** Design omission. **Status:** Open.

JST branding alone does not define family, pitch, polarity or mating direction.

**Correction:** Record each connector, pinout, cable length, current rating and termination.

**Close when:** Every connection made without soldering, trace cuts or unsecured jumpers.

**Basis:** S10; S21.

### R14 — Casey ESP32 firmware is not Pi software
**Priority:** High. **Type:** Confirmed platform mismatch. **Status:** Open.

DSI face, USB audio and CSI capture need native Pi implementation.

**Correction:** Port protocol/state logic through explicit hardware adapters; freeze baseline.

**Close when:** End-to-end Pi/Spark test works on exact OS and pinned packages.

**Basis:** S17; S18.

### R15 — AEC board does not remove v2 microphone gating
**Priority:** High. **Type:** Documented behavior. **Status:** Open.

Protocol waits for speak_done before unmuting; no true spoken interruption yet.

**Correction:** First prove half-duplex; then implement cancellation, echo-aware VAD and synchronized playback.

**Close when:** User interruption stops output and is transcribed without self-triggering.

**Basis:** S17; S04.

### R16 — Reference WebSocket has token but no TLS
**Priority:** High. **Type:** Documented behavior. **Status:** Open.

Plain ws:// and defaults do not meet intended private service design.

**Correction:** Use authenticated encrypted local endpoint; no port forwarding; explicit image policy.

**Close when:** Unauthorized client rejected; no public service; internet-blocked cold-start test passes.

**Basis:** S17; S19.

### R17 — Distinct addresses do not prove bus integrity
**Priority:** Medium. **Type:** Untested integration. **Status:** Open.

Cable capacitance and parallel pull-ups can cause errors.

**Correction:** Keep runs short, start 100kHz, inspect rise/low levels if errors; no blind resistor cutting.

**Close when:** Stable five-sensor run under motor/audio/LED load; no I2C errors.

**Basis:** S14; S30.

### R18 — Self-heating can falsify room readings
**Priority:** Medium. **Type:** Documented design risk. **Status:** Open.

Pi, SGP heater, LEDs and exhaust share small housing.

**Correction:** Isolate SHT45 ambient pocket; move SGP separately; compare room reference.

**Close when:** Heat-bias and response test meet stated project tolerance.

**Basis:** S15; S16.

### R19 — Self-light and protective windows bias sensors
**Priority:** Medium. **Type:** Design risk. **Status:** Open.

LED leakage affects lux control; proximity cover may reflect/block IR.

**Correction:** Opaque baffles; validate actual window material and face/LED on-off behavior.

**Close when:** No brightness feedback oscillation or false near detection.

**Basis:** S25; S26.

### R20 — Raw ticks are not calibrated gas indices
**Priority:** Medium. **Type:** Documented algorithm requirement. **Status:** Open.

Gas index needs compensation and correct timing; no direct CO2 result.

**Correction:** Use manufacturer algorithm and units; tag warming/invalid/stale output.

**Close when:** Known sample handling and fault tests; no ppm/air-safe claims from raw values.

**Basis:** S16.

### R21 — Decorative vents are not a verified air path
**Priority:** Medium. **Type:** Design risk. **Status:** Open.

Cooling exhaust may heat sensors or reach microphones.

**Correction:** Dedicated intake/exhaust path, accessible fan, thermal soak in closed shell.

**Close when:** No undervoltage or thermal throttling; acceptable acoustic behavior.

**Basis:** S01; S15.

### R22 — Rendered face is larger than real active display
**Priority:** Medium. **Type:** Confirmed concept mismatch. **Status:** Open.

2.8-inch 4:3 active area is about 57x43mm; it cannot fill arbitrary large bezel.

**Correction:** Freeze 1:1 face mockup before head outline; module geometry controls cutout.

**Close when:** User accepts actual-size face and bezel after physical test.

**Basis:** S02.

### R23 — I/O panel artwork does not match Pi geometry
**Priority:** Medium. **Type:** Design omission. **Status:** Open.

Ports on different edges and connectors occupy insertion volume.

**Correction:** Model actual connected plugs, microSD service, tool paths and revision-specific openings.

**Close when:** Cable replacement possible without dismantling neck or damaging ribbons.

**Basis:** S01; S11.

### R24 — Opaque gray PA12 is not a diffuser
**Priority:** Medium. **Type:** Design omission. **Status:** Open.

Smooth inlay requires separate material, mixing gap and retention.

**Correction:** Prototype one removable cartridge; make other rings passive trim.

**Close when:** Uniform low-brightness amber at normal view angles, no light leaks.

**Basis:** S20; S22.

### R25 — Fasteners and finish tolerances not qualified
**Priority:** Medium. **Type:** Design omission. **Status:** Open.

Small clearances can close after finishing; cosmetic skins aren't load paths.

**Correction:** Captive nuts and fit coupons; no heated inserts; structural metal hardware where needed.

**Close when:** Coupons pass, material/finish selected, final mesh checked and priced.

**Basis:** S20.

### R26 — Multiple consumers / oversized stills can break pipeline
**Priority:** Medium. **Type:** Documented protocol + design risk. **Status:** Open.

Reference max message is 1MB; 12MP JPEG may exceed it.

**Correction:** Single capture broker; downscale/encode bounded payloads; separate tracking and VLM snapshots.

**Close when:** Concurrent preview/tracking/snapshot requests remain responsive within memory limits.

**Basis:** S17; S03.

### R27 — Gesture and tracking commands can conflict
**Priority:** Medium. **Type:** Design risk. **Status:** Open.

Independent writers cause jitter and aggressive position reversals.

**Correction:** One local priority arbiter; rate limits, deadband and bounded acceleration.

**Close when:** Concurrent listening/gesture/face-track events produce smooth predictable motion.

**Basis:** Engineering requirement.

### R28 — Wrong array profile/channel mapping degrades processing
**Priority:** Medium. **Type:** Documented configuration. **Status:** Open.

Circular/linear and 2-/6-channel images differ; audio routes must match.

**Correction:** Pin firmware/array geometry and record capture/playback mappings.

**Close when:** Correct processed stream, playback reference, direction orientation and reconnect behavior.

**Basis:** S04; S05.

### R29 — ARM64 and CUDA dependencies not validated
**Priority:** Medium. **Type:** Untested integration. **Status:** Open.

Pinned reference packages are not proof of a working Spark stack.

**Correction:** Reproducible environment; benchmark ASR/VLM/TTS together; avoid unnecessary frameworks.

**Close when:** Cold-start offline local conversation works with logged latency and memory.

**Basis:** S18; S19.

### R30 — Power inputs need strain relief and current protection
**Priority:** Medium. **Type:** Design omission. **Status:** Open.

USB shells or screw terminals should not take cable-pull loads.

**Correction:** Protected low-voltage branches, secured leads, polarity labels, no mains inside robot.

**Close when:** Tug/reseat/short-protection checks without damage; correct rated harness.

**Basis:** S10; S22.

### R31 — USB enumeration and restart can switch devices
**Priority:** Medium. **Type:** Design risk. **Status:** Open.

Card indices/tty numbering are not stable identities; old speech/motion may resume.

**Correction:** Stable hardware matching, session IDs and empty queues on reconnect.

**Close when:** Repeated reboot/replug tests show no stale action or wrong audio endpoint.

**Basis:** Engineering requirement.

### R32 — Enclosure and wiring may impair RF link
**Priority:** Medium. **Type:** Design risk. **Status:** Open.

Metallic finish, ballast and ground planes may shadow antennas.

**Correction:** Keep antenna areas clear; establish wired baseline; test in final enclosure.

**Close when:** No perceptible audio loss at actual desk; reconnect remains safe.

**Basis:** S07; Engineering requirement.

### R33 — Starter workbook no longer reflects ordered parts
**Priority:** Low. **Type:** Confirmed document drift. **Status:** Open.

64GB/official PSU and old controller assumptions are stale.

**Correction:** Use this audit baseline and receipts; preserve old workbook as history.

**Close when:** Inventory matches actual labels and current architecture.

**Basis:** Local receipts; starter workbook.

### R34 — Breakout and camera platform need not fit final robot
**Priority:** Low. **Type:** Confirmed scope distinction. **Status:** Open.

Forcing bench fixtures into shell adds volume without capability.

**Correction:** Keep Black HAT/Arducam test mechanism as development tooling unless justified.

**Close when:** Final BOM excludes redundant servo/LED controllers and unnecessary boards.

**Basis:** S12; Conversation.

## Appendix B — Acceptance tests

### T01 — Unpowered inspection (Power/harness)
Check labels, pin-1 alignment, voltage/polarity, no exposed shorts, proper standoffs and physical support.

**Criterion:** No ambiguous connection or loose conductive part; do not power motors yet.

**Related findings:** R05; R13; R30. **Status:** Not run.

### T02 — Pi baseline (Computer)
Boot exact SD image with cooler/Ethernet, confirm supply capability, log undervoltage/throttle status.

**Criterion:** Stable boot and repeatable baseline before adding peripherals.

**Related findings:** R02; R21; R29. **Status:** Not run.

### T03 — Face/camera baseline (Head electronics)
Test landscape face, dimming, autofocus, simultaneous camera and display, latches and short bench cable.

**Criterion:** No video errors, tension or glass compression; 1:1 bezel mockup accepted.

**Related findings:** R03; R22; R26. **Status:** Not run.

### T04 — Sensor bus (I2C)
Add sensors individually, then run all at 100kHz initial setting with production-length wiring.

**Criterion:** Correct expected addresses; no recurring I2C errors over 1-hour combined activity test (project target).

**Related findings:** R17; R20. **Status:** Not run.

### T05 — Sensor validity (Environmental)
Log temperature/humidity/pressure/gas indices; compare ambient reference with Pi/LEDs off vs active.

**Criterion:** Record bias and stabilization; revise pocket if electronics heat dominates readings.

**Related findings:** R18; R20. **Status:** Not run.

### T06 — Audio bench (Audio)
Use exact circular USB firmware; prove processed capture and playback through same Flex.

**Criterion:** Correct channels, intelligible speech, no clipping or unexpected resets.

**Related findings:** R11; R28. **Status:** Not run.

### T07 — Power budget (All rails)
Measure voltage and simultaneous current with face bright, camera active, voice loud enough, LEDs worst allowed, both joints loaded.

**Criterion:** Known rail/connector ratings respected; 20% steady headroom is proposed design target, not a substitute for transient testing.

**Related findings:** R02; R04; R05. **Status:** Not run.

### T08 — One light cartridge (Lighting)
Run independent pixels with final harness; test data direction, diffuser, dim amber, startup and full allowed test pattern.

**Criterion:** No backfeed, no exposed hot spot, uniform look, brightness ceiling persists after reset.

**Related findings:** R04; R09; R24. **Status:** Not run.

### T09 — One joint dummy load (Motion)
Measure mass/CG and ribbon drag; test one actuator at gentle limits under representative load.

**Criterion:** Stable hold, acceptable heat/noise/current, no contact with hard stops in normal operation.

**Related findings:** R03; R07; R08. **Status:** Not run.

### T10 — Motion fault injection (Safety)
Remove Spark link, stop Pi application, interrupt USB, remove motor power and reboot separately.

**Criterion:** Commands expire, fault does not auto-resume, head remains physically supported.

**Related findings:** R06; R07; R08; R27. **Status:** Not run.

### T11 — Cable endurance screening (Neck)
Cycle bounded pan/tilt with final cable type, supports and display/camera active.

**Criterion:** Project screening: 5,000 representative cycles with zero intermittent faults or visible damage; not a lifetime certification.

**Related findings:** R03. **Status:** Not run.

### T12 — Enclosed acoustic/thermal (Body)
Run combined workload in temporary representative enclosure at actual desk, including motors and fan.

**Criterion:** No thermal throttling/undervoltage; no speech failure attributable to fan, vibration or enclosure echo.

**Related findings:** R10; R18; R21; R28. **Status:** Not run.

### T13 — Sensor optical isolation (Optics)
Vary face and inlay brightness under fixed room lighting; test chosen proximity window.

**Criterion:** No self-amplifying brightness loop; stable proximity without internal reflections.

**Related findings:** R19; R24. **Status:** Not run.

### T14 — Local AI / network (Software)
Run reproducible Spark services, then block internet while keeping LAN and cold-start both devices.

**Criterion:** Conversation/visual request works locally; authenticated encrypted connection; stale images/audio cleared.

**Related findings:** R14; R16; R29. **Status:** Not run.

### T15 — Conversation interruption (Audio software)
After turn-taking works, interrupt during speech from representative desk positions.

**Criterion:** Stops old speech, retains intended user turn, no speaker-triggered loop.

**Related findings:** R15; R28. **Status:** Not run.

### T16 — Privacy / restart (Privacy)
Test mute/shutter/disconnect states and reconnect repeatedly, including fresh boot.

**Criterion:** No recording/stream claimed disabled when active; no stale action or hidden recording.

**Related findings:** R12; R16; R31. **Status:** Not run.

### T17 — Fit / maintenance (CAD)
Fit critical mounts, connectors, nuts, diffuser and service panels; inspect motion swept volumes.

**Criterion:** No collisions, blocked latch/tool access or trapped cables; head removable independently.

**Related findings:** R01; R23; R25. **Status:** Not run.

### T18 — Print release (Release)
Choose finish; inspect solid/manifold geometry, tolerances, powder escape, fastener holes and revision labels.

**Criterion:** All blockers closed with evidence; printer quote reviewed; no full-body order from concept art.

**Related findings:** R01; R03; R25. **Status:** Not run.

## Appendix C — Primary source index
Sources were consulted on 25 October 2026. Board specifications remain subject to the actual delivered revision. Supplier CAD downloads could not be imported in this session; none was used to assert a fit result.

- **[S01] Raspberry Pi hardware / power and GPIO documentation:** https://www.raspberrypi.com/documentation/computers/raspberry-pi.html

- **[S02] Waveshare 2.8-inch DSI LCD 22028:** https://www.waveshare.com/2.8inch-dsi-lcd.htm

- **[S03] Raspberry Pi Camera Module 3:** https://www.raspberrypi.com/products/camera-module-3/

- **[S04] Seeed reSpeaker Flex integration:** https://wiki.seeedstudio.com/respeaker_flex_introduction/

- **[S05] Seeed Flex FAQ / mechanical and cable caveats:** https://wiki.seeedstudio.com/respeaker_flex_faq/

- **[S06] Seeed Flex microphone geometry:** https://www.seeedstudio.com/blog/2026/04/09/introducing-respeaker-flex-the-smart-ear-for-robotics-and-embedded-ai/

- **[S07] Pimoroni Plasma 2350 W:** https://shop.pimoroni.com/products/plasma-2350-w

- **[S08] Plasma 2350 W schematic, sheet 1: VBUS and terminal share 5V:** https://cdn.shopify.com/s/files/1/0174/1800/files/Pimoroni_Plasma_2350_W_Schematic.pdf?v=1732206580

- **[S09] ROBOTIS XL330 specification / control table:** https://emanual.robotis.com/docs/en/dxl/x/xl330-m288/

- **[S10] ROBOTIS U2D2 Power Hub:** https://emanual.robotis.com/docs/en/parts/interface/u2d2_power_hub/

- **[S11] ROBOTIS U2D2 product / connector revision:** https://robotis.us/u2d2/

- **[S12] Pimoroni Mini Black HAT Hack3r:** https://shop.pimoroni.com/products/mini-black-hat-hack3r

- **[S13] Adafruit Pi QT adapter:** https://learn.adafruit.com/adafruit-pi-stemma-qt-breakout/pinouts

- **[S14] TI I2C pull-up calculation:** https://www.ti.com/lit/an/slva689/slva689.pdf

- **[S15] Sensirion temperature/humidity enclosure guidance:** https://sensirion.com/media/documents/FC5BED84/662B494D/Sensirion_Humidity_Temperature_Design_Guide.pdf

- **[S16] Sensirion gas index algorithm:** https://github.com/Sensirion/gas-index-algorithm

- **[S17] Casey desk-robot v2 protocol:** https://raw.githubusercontent.com/cgro00/desk-robot/v2.0/docs/protocol.md

- **[S18] Casey v2 pinned dependencies:** https://raw.githubusercontent.com/cgro00/desk-robot/v2.0/server/requirements.txt

- **[S19] NVIDIA Spark local inference:** https://build.nvidia.com/spark/llama-cpp/overview

- **[S20] Shapeways PA12 material and tolerance:** https://www.shapeways.com/materials/pa12

- **[S21] M5Stack RGB U003 pinout:** https://docs.m5stack.com/en/unit/Unit_RGB

- **[S22] Adafruit pixel power budgeting:** https://learn.adafruit.com/adafruit-neopixel-uberguide/powering-neopixels

- **[S23] SHT4x / SHT45 pinout:** https://learn.adafruit.com/adafruit-sht40-temperature-humidity-sensor/pinouts

- **[S24] BMP581 pinout:** https://learn.adafruit.com/adafruit-bmp580-bmp581-and-bmp585-temperature-and-pressure-sensor/pinouts

- **[S25] VEML7700 pinout:** https://learn.adafruit.com/adafruit-veml7700/pinouts

- **[S26] VCNL4200 pinout:** https://learn.adafruit.com/adafruit-vcnl4200-long-distance-ir-proximity-and-light-sensor/pinouts

- **[S27] SGP41 pinout:** https://learn.adafruit.com/adafruit-sgp41-multi-pixel-gas-sensor-breakout/pinouts

- **[S28] Camera cable pinout distinction:** https://www.adafruit.com/product/5819

- **[S29] Seeed 4-ohm 5W enclosed speaker:** https://www.seeedstudio.com/Mono-Enclosed-Speaker-4R-5W-p-5931.html

- **[S30] Adafruit QT adapter design / pull-ups:** https://blog.adafruit.com/2025/07/09/coming-soon-adafruit-pi-stemma-qt-breakout-for-raspberry-pi-and-compatibles/

## Source files reviewed
- `Rocky_Starter_Components.xlsx` (historical procurement plan).
- `rocky_design/Rocky_Preliminary_Body_Sections_v0_1.md` (preliminary mechanical plan).
- Provided gray concept and exploded-view images (styling only).
- Available purchase confirmations; no personal addresses or contact details reproduced.
