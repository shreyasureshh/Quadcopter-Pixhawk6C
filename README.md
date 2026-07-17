# Quadcopter-Pixhawk6C
Building and Testing a Quadcopter using Pixhawk 6C and PX4
# Quadcopter Build (S500 / Pixhawk 6C / PX4)

A ground-up build, wiring, calibration, and flight-test log for a custom S500-frame quadcopter running PX4 on a Pixhawk 6C. Documented end-to-end: system architecture → electrical wiring → QGroundControl calibration → flight testing → failure analysis → fix.

> Built as part of hands-on UAV training. This repo doubles as a technical reference and a debugging case study — the first flight failed, and the process of diagnosing and fixing it is documented in as much detail as the build itself.

## Specs

| Component | Spec |
|---|---|
| Frame | S500 quadcopter frame |
| Flight Controller | Holybro Pixhawk 6C |
| Motors | 2212, 900–920KV BLDC (x4) |
| ESCs | 40A, 2–6S (x4) |
| Propellers | 1045 (10" diameter, 4.5" pitch), 2-blade |
| Battery | LiPo 5200mAh 35C, 6S (22.2V nominal) |
| GPS | Holybro M9N → later swapped to Here3+ (CAN) |
| Telemetry | 433 MHz, MAVLink protocol |
| Ground Station | QGroundControl (QGC) |
| Firmware | PX4 (tested on v1.16) |
| OS used for GCS | Ubuntu |

Estimated all-up weight ≈ 1.7 kg, sized for a 2:1 thrust-to-weight ratio (≈850 g thrust per motor).

## Repo structure

```
quadcopter-build/
├── README.md              ← you are here
├── hardware/
│   └── BOM.md              ← full component list, specs, wiring pinouts
├── docs/
│   ├── build-log.md        ← system architecture + assembly walkthrough
│   ├── calibration.md      ← QGroundControl setup & calibration steps
│   ├── troubleshooting.md  ← Ubuntu/QGC issues + propulsion debugging
│   └── test-flights.md     ← flight test results + PX4 log analysis
└── firmware/
    └── notes.md            ← PX4 version, flashing notes
```

## Flight test summary

| Test | Outcome | Root cause |
|---|---|---|
| Flight 1 | Failed — toppled on throttle-up | Roll/yaw instability, propeller insecure |
| Flight 2 (no props) | Motors 1 & 4 inconsistent / stopped | Isolated to propulsion system (motor/ESC/wiring) — GPS, power, control link all ruled out |
| Flight 3 | **Success** — stable hover, accurate pitch/roll/yaw tracking | Frame, battery, and propellers replaced |

Full log analysis (pitch/roll/yaw tracking, vibration, power draw, motor outputs) is in [`docs/test-flights.md`](docs/test-flights.md).

## Key learnings

- Assembling a drone and *understanding* every subsystem well enough to debug it are two different skill levels — the second only shows up when something goes wrong.
- Propeller CW/CCW orientation looks near-identical but is safety-critical; getting it wrong is a common, easy-to-miss failure point.
- Systematic elimination (swap GPS → rule out nav; recalibrate ESCs → rule out calibration; check power rail → rule out power) is far more efficient than guessing at the "obvious" cause first.
- A flight failure with zero structural damage (beyond the propeller) is a good outcome — it means the failure mode was caught early and safely.

## What's next

Groundwork from this build feeds into further autonomy work — gesture-based control, and eventually a cooperative drone-rover exploration system (ROS 2 / Nav2 / YOLOv8-based).

## References

- Q. Quan, X. Dai, S. Wang, *Multicopter Design and Control Practice*, Springer, 2020.
- [PX4 Autopilot User Guide](https://docs.px4.io)
- [Holybro Pixhawk 6C Documentation](https://docs.holybro.com/autopilot/pixhawk-6c)
- [QGroundControl User Guide](https://docs.qgroundcontrol.com)
- [MAVLink Documentation](https://mavlink.io)


