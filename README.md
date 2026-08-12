# LEGO Color Sorter v.1

An autonomous sorting machine built on LEGO Mindstorms EV3 that classifies bricks by color in real time and routes them into matching bins. Fully unattended after a single trigger press — sorts 16 bricks in 20 seconds.

**Demo:** https://tharhtetnyan.github.io/assets/projects/lego-color-sorter-v1.mp4

---

## Overview

The machine combines a gravity-fed magazine, a motorized singulation mechanism, an inline color sensor, and a motorized diverter to move each brick from bulk storage to a color-matched bin without manual intervention. The goal of v.1 was to validate the full feed → sense → sort pipeline end-to-end with a consistent, repeatable cycle time.

| Metric | Result |
|---|---|
| Bricks sorted per run | 16 |
| Total run time | 20 s |
| Average cycle time | ~1.25 s / brick |
| Color classes | 4 |
| Manual intervention after start | None |

---

## System architecture

The sort cycle runs as a closed loop, repeating once per brick:

**1. Magazine feed**
Bricks are pre-loaded into a vertical rail that stacks them and gravity-feeds the bottom-most brick to the singulation stage. The rail constrains bricks to a single-file column so only one is presented per cycle.

**2. Singulation (motorized pusher)**
A motor-driven pusher arm extends to push the bottom brick out of the magazine and into the sensing gate, then retracts to allow the next brick to drop into position. This decouples magazine feed timing from sensing/sorting timing.

**3. Color sensing**
As the brick passes through the gate, an EV3 color sensor takes a reading and the controller classifies it against the four target colors. Sensing happens in the brief window between push and sort, so cycle time is bounded by mechanical motion rather than sensor read time.

**4. Sort (rotating diverter)**
A motorized rail rotates to the output position corresponding to the classified color and releases the brick, which drops into one of four cups acting as color bins. The rail then resets to a neutral or next-target position for the following cycle.

---

## Hardware

| Component | Role |
|---|---|
| LEGO Mindstorms EV3 controller | Central logic, motor/sensor I/O |
| EV3 color sensor | Inline color classification |
| 2× EV3 motors | Magazine pusher, rotating diverter rail |
| Push button (touch sensor) | Start-of-run trigger |
| Technic beam/frame assembly | Structural frame, magazine rail, diverter arm |
| 4× cups | Color-sorted output bins |

> Confirm exact motor variants (Large/Medium servo motor) and whether a touch sensor or the EV3 brick's own buttons trigger the run.

---

## Software / control

Written in Python on the LEGO Mindstorms Education platform, controlling motor and sensor I/O directly on the EV3 brick.


---

## Results

- Completed 16/16 bricks across 4 color classes in the demo run, each landing in its correct color bin
- Consistent per-brick cycle time (~1.25 s) across the run, indicating stable mechanical timing rather than sensor-driven stalls
- No manual reset or intervention required between bricks


---

## Known limitations & next steps

- **Cycle time** is currently sequential (feed → sense → sort, one at a time). Overlapping the next feed step with the current sense/sort step could meaningfully cut total run time.
- **No jam/misfeed detection** — a stuck or double-fed brick at the magazine would stall the run silently.
- **No logging** — sensor readings and classification results aren't currently recorded, which would help quantify accuracy and tune sensor thresholds.
- **Fixed 4-color capacity** — bin count is mechanically limited to the diverter's rotation positions.

---

## Author

Thar Htet Nyan — Biomedical Engineering, Soonchunhyang University
