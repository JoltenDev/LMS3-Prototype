# LMS3-Prototype

Lane Management System (LMS) prototype demonstrating a complete system designed from curated requirements.

This prototype was produced by Claude Code Sonnet 5.5, strictly defined by the requirements the team created in the SRS.

## Overview

The LMS combines three driver-assist functions into one supervised system:

- **Lane Centering System (LCS):** steers the vehicle toward the middle of its lane.
- **Lane Departure Warning System (LDWS):** alerts the driver when the vehicle drifts across a lane marking without intent.
- **Lane Keeping System (LKS):** applies a corrective steering input when a departure is unintentional.

The prototype is a single self-contained HTML page. It simulates a top-down highway view and shows what each subsystem is doing as conditions change, so the team can review the requirements as behavior rather than text.

## Running the prototype

No build step or dependencies are needed.

1. Download or clone this repository.
2. Open `Lane_Management_System_prototype.html` in a modern desktop browser (Chrome, Edge, Firefox or Safari).
3. Pick a scenario from the scenario tabs and use the scenario buttons or the keyboard to drive it.

## Operating range

The LMS operates only above **35 mph** and below **85 mph**.

| Speed | LMS state | Indicator |
|---|---|---|
| 35 mph or below | Standby | Grey, "LMS is on standby" |
| Above 35 mph and below 85 mph | Active | Solid, "LMS is active" |
| 85 mph or above | Unavailable | "LMS unavailable; speed is out of range" |

A chime plays whenever the LMS disengages, and the systems re-enable automatically when speed returns to range.

## Scenarios

| # | Scenario | What it demonstrates |
|---|---|---|
| 1 | Ideal conditions | 50 mph on a straight road with clear markings. Lane boundaries are detected and the vehicle stays centered. |
| 2 | Curved road | 60 mph through a curve with minor marker noise (construction cones). Path prediction anticipates outward drift and LCS and LKS correct for it. |
| 3 | Subsystem failure and environmental degradation | Snow cover or camera dropout removes marker data. The system does not guess on missing data, hands control to the driver and alerts them with a chime, an amber fault light and a HUD message. |
| 4 | Driver override and control transfer | Case A: an intentional lane change with the turn signal suppresses warnings and intervention. Case B: steering torque above the override threshold disengages LKS. Case C: drifting opposite the turn signal still triggers a warning. |
| 5 | Speed boundaries | A sweep from 30 mph to above 85 mph and back, showing standby, active and unavailable states. |
| 6 | Unintentional lane departure | A distracted driver drifts toward the left boundary. LDWS warns (HUD, chime and haptic), then LKS steers back to lane center. |

## Controls

| Input | Action |
|---|---|
| **W** or **Up** | Increase speed (hold) |
| **S** or **Down** | Decrease speed (hold) |
| **A** or **Left** | Apply steering torque left (hold) |
| **D** or **Right** | Apply steering torque right (hold) |
| **H** | Toggle hands on or off the wheel |
| **Enter** | Acknowledge a warning |

On-screen controls include speed, curvature and torque sliders, the turn signal, a hands-on-wheel toggle, an LMS on and off switch, and **Push Left** and **Push Right** buttons that apply a lateral disturbance to the vehicle.

Each scenario also has its own buttons. Some run an "auto driver" script. Everything the auto driver does, and how the LMS responds, is written to the on-screen log.

## Design notes

- **No guessing:** when lane data is invalid or missing, the LMS disengages instead of estimating.
- **Turn signal logic:** warnings are suppressed only when a departure matches the signal direction. Departing the opposite way triggers a warning.
- **LMS off means off:** with the LMS disengaged, the vehicle does not follow the road. On a curve it drifts out of its lane.
- **Path certainty:** certainty drops with road curvature, snow and construction-cone noise. If it falls below what the current speed requires, the LMS hands control back to the driver.
- **Hands on wheel:** a single toggle covers hands on and hands off. Releasing the wheel also zeroes steering torque.

## Limitations

This is a simulation for demonstration and review. It is not production control software, uses simplified vehicle and road dynamics, and must not be used to make safety decisions.
