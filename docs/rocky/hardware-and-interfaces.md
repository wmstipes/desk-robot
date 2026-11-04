# Hardware and interface register

[Build overview](README.md) · [Review](reviews/integrated-systems-v0.2.md) · [Validation](validation.md)

**Recorded 4 November 2026 from the supplied project review and approved decisions.** This is a sanitized baseline, not a new inspection or shopping list. Receipt numbers, delivery addresses, contact details, private network names, recordings, and account credentials are intentionally excluded.

## Status rules

- **Procurement confirmed:** supported by the earlier reviewed order or explicit project confirmation; not proof of delivery, condition, exact board revision, or completed testing.
- **Selected direction:** intended architecture; purchase/receipt is not confirmed in this baseline.
- **Candidate:** requires an engineering decision before purchase or enclosure freeze.
- **Evidence pending:** physical labels, photos, measurements, or qualifying test results have not been imported.

Every row remains evidence pending for complete-system validation. Do not infer safe wiring from a procurement status.

## Component inventory

| ID | Component / variant | Qty | Baseline status | Intended placement / next evidence |
|---|---|---:|---|---|
| H01 | Raspberry Pi 5, 4GB | 1 | Procurement confirmed | Stationary body; record revision and OS/kernel. |
| H02 | Official Pi 5 Active Cooler | 1 | Procurement confirmed | Dedicated fan socket; confirm cable seating and unobstructed airflow. |
| H03 | Official Raspberry Pi microSD, 256GB, blank as ordered | 1 | Procurement confirmed | Record installed image; older starter plan's 64GB value is superseded. |
| H04 | Waveshare 27W USB-C supply, PiShop SKU 1876-1, attached 1.2m cable | 1 | Procurement confirmed | Not the official Pi supply named in the old plan; inspect label and verify negotiated capability. |
| H05 | Waveshare 2.8-inch DSI LCD, 22028 / PiShop 1104-1 | 1 | Procurement confirmed | Head; 480x640 panel with landscape software orientation; measure full module. |
| H06 | Raspberry Pi Camera Module 3 Standard, visible-light | 1 | Procurement confirmed | Head; not Wide, NoIR, or AI Camera. |
| H07 | Pi 5 camera adapter, 22-pin to 15-pin, 300mm | 2 | Procurement confirmed | One used, one spare; camera pinout is not display pinout. |
| H08 | Included Waveshare DSI-Cable-12cm | 1 | Package contents documented; receipt inspection pending | Bench cable only; final moving route and length unresolved. |
| H09 | reSpeaker Flex XVF3800 Circular-4, without XIAO, 100005504 | 1 set | Order reported | Core and microphone array stationary; inspect firmware, connectors and acoustic ports. |
| H10 | Seeed enclosed speaker, 4 ohm / 5W, 114993346 | 1 | Ordered baseline reported; delivery contents unverified | Stationary cradle; verify factory plug, pinout, gain and output limits. |
| H11 | Adafruit Pi STEMMA QT adapter, 6365 | 1 | Procurement confirmed | Bench GPIO breakout; inspect condition and orientation before power. |
| H12 | SHT45 with PTFE filter, Adafruit 6174 | 1 | Procurement confirmed | Thermally isolated, room-air pocket. |
| H13 | BMP581, Adafruit 6407 | 1 | Procurement confirmed | Ambient-pressure access, not direct fan flow. |
| H14 | VEML7700, Adafruit 4162 | 1 | Procurement confirmed | Room-light opening shielded from face and inlays. |
| H15 | VCNL4200, Adafruit 6064 | 1 | Procurement confirmed | Forward optical opening; qualify any protective window. |
| H16 | SGP41, Adafruit 6455 | 1 | Procurement confirmed | Separate vented pocket; manufacturer compensation/index processing. |
| H17 | STEMMA QT/Qwiic cables, 100mm / 200mm | 4 / 4 | Procurement confirmed | Five links needed for five boards; three spares; final bus must be tested. |
| H18 | Mini Black HAT Hack3r PIM169, assembled | 1 | Procurement confirmed | Bench access; duplicates existing GPIO, not extra independent signals or power. |
| H19 | 2x20 socket riser, PiShop 927-1 | 1 | Procurement confirmed | Use only after fit/pin-alignment check; not a permission to stack blindly. |
| H20 | Arducam camera-only pan/tilt kit B0283 | 1 | Ordered by project owner | Its controller serves camera experiments; not qualified for the full screen head. |
| H21 | USB-A to USB-C data cable for Flex | 1 | Planned; presence unverified | Verify data capability and routing. Additional Plasma cable is separate. |
| H22 | M2.5 and M3 nylon mounting assortments | 1 each | Procurement confirmed | Bench electronics; not automatic approval for load-bearing neck fasteners. |
| H23 | Pimoroni Plasma 2350 W PIM729 | 1 | Selected direction; purchase unconfirmed | Stationary lighting controller; inspect revision and connectors. |
| H24 | ROBOTIS U2D2 and Power Hub | 1 set | Candidate | USB logic plus independently supplied motor branch. |
| H25 | DYNAMIXEL XL330 / XC330 actuator family | 2 | Candidates, exact models not selected | Select after head mass, center of gravity, cable drag, noise and thermal tests. |
| H26 | Prewired LED modules, diffusers, harness | TBD | Not selected | Two independently addressable inlays, solder-free assembly. |
| H27 | Motor supply, protection, cutoff, privacy controls | TBD | Not selected | Must be a complete voltage/polarity/connector-rated design before powered movement. |

The prototyping platform and caliper/scale are development resources; their exact received configuration is not established here. No smart servo, new speaker upgrade, or extra HAT is silently added to the purchased inventory.

## Intended interfaces (not a wiring authorization)

| Endpoint | Interface | Peer | Guardrail / acceptance |
|---|---|---|---|
| Pi CAM/DISP0 | CSI camera adapter | Standard Camera Module 3 | Camera-specific cable; unpowered installation; qualify moving route. |
| Pi CAM/DISP1 | DSI display adapter | Waveshare 22028 | Correct display pinout and software configuration; 120mm supplied cable is only the starting point. |
| Pi FAN | Dedicated four-pin fan lead | Active Cooler | Not interchangeable with QT; USB/fan power allowance applies. |
| Pi 40-pin GPIO | Pin-for-pin ribbon / breakout | Black HAT and QT adapter | Check pin 1, every occupied row, socket seating and cooler clearance while unpowered. |
| QT chain | 3.3V I2C | SHT45, BMP581, VEML7700, VCNL4200, SGP41 | Confirm addresses and bus integrity; never power motors/LED strips from this chain. |
| Pi USB-A | USB audio | Flex XMOS USB-C | Same audio processor for microphone capture and speaker playback; record stable hardware identity. |
| Flex | Supplied microphone ribbon | Circular array | Both boards stationary; preserve array geometry and ports. |
| Flex amplified output | Verified speaker connector | Enclosed 4-ohm speaker | No second amplifier chained to output; verify load, plug, polarity and gain. |
| Pi USB-A (proposed) | USB serial/interface | U2D2 | Logic interface is not motor power. |
| U2D2/Power Hub (proposed) | Compatible serial bus | Selected actuators | Correct voltage, unique IDs, one hub power input, protected supply and cutoff. |
| Pi USB or LAN (proposed) | Application protocol | Plasma 2350 W | USB-only power OR separate USB-C power with wireless control; never join independent 5V sources. |
| Plasma output (unselected harness) | Addressable LED data/power | Two light cartridges | Verify DIN/DOUT or independent outputs; generic Grove and QT ports are not interchangeable. |
| Pi Ethernet/Wi-Fi | Authenticated encrypted LAN service | Spark | No public port forwarding; no default cloud fallback; clear expired sessions/commands. |

Default sensor addresses recorded in the source review: VEML7700 `0x10`, SHT45 `0x44`, BMP581 `0x47`, VCNL4200 `0x51`, SGP41 `0x59`. Verify the delivered boards; distinct addresses do not establish electrical bus integrity.

## Power records to complete before enclosure release

| Domain | Required record | Current evidence |
|---|---|---|
| Pi main input | Actual supply label, negotiated capability, voltage drop, boot and simultaneous loads | Not imported. |
| USB + fan | Combined Flex, cooler, interfaces and optional USB lighting load; sustained and transient behavior | Not imported; the prior review cites 1.6A with a recognized 5A supply, not per port. |
| Motor branch | Selected actuator voltage, current profile, supply, protection, cutoff, connectors and wire ratings | Actuators not selected. |
| Lighting | Exact pixels, allowed worst pattern, controller overhead, power source, no-backfeed test | LEDs and harness not selected. |
| QT/sensor branch | Actual wiring length, regulator load, pull-ups, address/voltage and error tests | Qualification pending. |

Unknown measured values are **unknown, not zero**. Do not claim that a supply rating, a spare USB socket, or an LED brightness preference proves adequate headroom. Motor cutoff must leave the head mechanically supported. Do not intentionally short live supplies or stall actuators as an improvised protection test.

## Harness and packaging release record

For each lead, record both endpoints, connector family/pitch/keying, pinout/polarity, voltage, maximum current, cable length, strain relief, moving/static classification and source part number. Photographs shared publicly must omit addresses, serial identifiers, network credentials and private background content. All solder-free claims require a completed harness audit.

Source: the supplied [integrated review](reviews/integrated-systems-v0.2.md), including its source index S01-S30. The register is a documentation import, not an independent remeasurement of manufacturer specifications.
