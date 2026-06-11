

# Hardware Guide

## Overview

This guide describes the minimum hardware setup for **ReleaseReady-XR**.

The goal is to provide a low-cost physical interface that allows users to experience bowstring tension while training in XR.

The minimum prototype uses:

* XR HMD
* two hand controllers
* BLE heart-rate strap
* elastic resistance rig

Optional components:

* FSR pressure sensor for grip tension
* Arduino or ESP32 for custom sensor input
* chest harness for more stable resistance anchoring

---

## Minimum Hardware Setup

| Component               | Purpose                             |
| ----------------------- | ----------------------------------- |
| XR HMD                  | Displays the virtual archery range  |
| Two hand controllers    | Track bow hand and draw hand        |
| BLE heart-rate strap    | Captures heart rate and RR interval |
| Elastic resistance band | Simulates bowstring tension         |
| Controller haptics      | Provides release sensation          |

Recommended heart-rate sensor:

* Polar H10 or compatible BLE heart-rate strap

---

## Tension Rig Concept

The tension rig simulates the physical resistance of pulling a bowstring.

Two possible configurations are supported.

### Option A. Hand-to-Hand Elastic Rig

```text
Bow-hand controller  ← elastic band →  Draw-hand controller
```

The elastic band connects both hands.
As the user pulls the draw hand backward, the resistance increases.

Advantages:

* simple
* low-cost
* easy to prototype

Limitations:

* resistance direction may shift depending on hand posture
* controller attachment must be secure

### Option B. Chest-Harness Elastic Rig

```text
Chest harness  ← elastic band →  Draw-hand controller
```

The elastic band connects the draw hand to the user's torso.
This can provide a more stable backward resistance.

Advantages:

* more stable resistance direction
* better simulation of draw force

Limitations:

* requires a harness
* needs more careful safety adjustment

---

## Basic Bill of Materials

| Item                                 | Quantity | Notes                          |
| ------------------------------------ | -------: | ------------------------------ |
| Elastic resistance band              |      1–2 | Start with low resistance      |
| Soft wrist strap or controller mount |      1–2 | Prevent slipping               |
| Carabiner or quick-release clip      |      1–2 | Recommended for safety         |
| Chest harness                        | Optional | For Option B                   |
| Velcro straps                        |  Several | For adjustable mounting        |
| BLE heart-rate strap                 |        1 | Polar H10 or equivalent        |
| FSR pressure sensor                  | Optional | For grip tension sensing       |
| Arduino / ESP32                      | Optional | For custom sensor transmission |

---

## Assembly: Hand-to-Hand Version

1. Attach one end of the elastic band to the bow-hand controller.
2. Attach the other end to the draw-hand controller.
3. Ensure both ends are secured with soft straps or mounts.
4. Start with a low-resistance band.
5. Test the draw motion slowly.
6. Confirm that the elastic band does not hit the face, neck, or HMD.
7. Add a quick-release clip if possible.
8. Calibrate the virtual draw length using controller distance.

---

## Assembly: Chest-Harness Version

1. Wear a soft chest harness.
2. Attach one end of the elastic band to the harness.
3. Attach the other end to the draw-hand controller.
4. Adjust the band length so that resistance begins after the initial draw.
5. Test the motion with low force.
6. Confirm that the draw direction feels stable.
7. Ensure that release does not cause the band to snap toward the body or HMD.

---

## XR Synchronization

The virtual bow draw is driven by controller distance.

Tracked value:

```text
draw_length = distance(bow_hand_controller, draw_hand_controller)
```

The elastic band provides physical resistance while the Unity scene visualizes the same draw length.

This creates synchronization between:

* physical tension
* virtual bowstring draw
* release timing
* controller haptic feedback

---

## Release Feedback

At the release moment, the system provides:

1. elastic relaxation
2. controller haptic pulse
3. virtual arrow launch
4. shot result logging

Recommended haptic pattern:

```text
Release: short strong pulse
Error release: double pulse
Stable release: short clean pulse
```

---

## Optional FSR Grip Module

An FSR pressure sensor can be attached to the bow-hand controller grip.

Purpose:

* estimate grip tension
* detect excessive squeezing
* provide feedback when grip pressure is too high

Basic pipeline:

```text
FSR sensor → Arduino / ESP32 → Serial or BLE → Unity
```

Minimum data format:

```json
{
  "timestamp": 1720000000,
  "grip_pressure": 0.62
}
```

This module is optional.
If time is limited, grip tension can be evaluated through Wizard of Oz coaching.

---

## Calibration

Before each session:

1. Check HMD and controller tracking.
2. Confirm heart-rate strap connection.
3. Measure neutral hand distance.
4. Measure maximum comfortable draw distance.
5. Set elastic resistance to low or medium.
6. Ask the user to perform three practice draws.
7. Save baseline draw length and anchor distance.

Suggested calibration values:

| Value                        | Usage                    |
| ---------------------------- | ------------------------ |
| Neutral hand distance        | Initial state            |
| Max comfortable draw         | Safe range limit         |
| Anchor distance baseline     | Anchor consistency check |
| Resting heart rate           | Physiological baseline   |
| Comfortable resistance level | Safety setting           |

---

## Safety Guidelines

* Do not use high-resistance bands for early testing.
* Do not aim the elastic band toward the face.
* Always test without HMD first.
* Use quick-release clips when possible.
* Avoid sharp hooks or hard metal mounts near controllers.
* Stop immediately if the user feels pain or excessive strain.
* Keep the draw force below the user's comfortable range.
* This prototype is not a real archery device and should not launch physical projectiles.

---

## Maintenance Checklist

Before demo:

* [ ] Elastic band has no visible damage
* [ ] Controller mounts are secure
* [ ] Quick-release works
* [ ] HMD tracking is stable
* [ ] BLE heart-rate sensor connects
* [ ] Unity receives tracking data
* [ ] Haptic feedback triggers on release
* [ ] Emergency stop / pause is available

---

## Known Limitations

* Elastic bands do not perfectly reproduce real bow draw curves.
* Resistance may vary depending on body posture.
* Controller tracking can be unstable if occluded.
* BLE heart-rate data may have latency.
* Grip pressure sensing is optional and may require calibration.

---

## Future Improvements

* Adjustable resistance mechanism
* Safer modular bow handle
* FSR-based grip tension tracking
* IMU-based wrist rotation sensing
* Better release haptic pattern
* Automatic hardware calibration
