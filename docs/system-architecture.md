# System Architecture

## Overview

**ReleaseReady-XR** is an adaptive XR archery training system that integrates XR tracking, biofeedback, physical tension feedback, and AI coaching into a closed learning loop.

The system is designed to help beginners understand not only *where the arrow landed*, but *why the shot failed*.

Core learning loop:

> **Perceive → Correct → Retry**

---

## Design Goals

ReleaseReady-XR aims to provide:

1. **Self-guided archery training without a physical range**
2. **Real-time awareness of invisible performance factors**

   * heart rate
   * breathing rhythm
   * draw stability
   * anchor consistency
   * release timing
3. **Physical tension feedback**

   * elastic resistance simulates bowstring tension
4. **Cause-oriented feedback**

   * shot results are linked to posture, heart rate, and release data
5. **Beginner-friendly AI coaching**

   * raw metrics are converted into actionable feedback

---

## System Layers

```text
User
 │
 │  HMD + Hand Controllers
 │  BLE Heart-Rate Strap
 │  Elastic Tension Rig
 ▼
Data Collection Layer
 │
 │  6DoF tracking
 │  Heart-rate / RR interval
 │  Draw and release interaction
 ▼
Analysis Layer
 │
 │  Shooting phase detection
 │  Rule-based error flags
 │  Release Ready estimation
 ▼
AI Coaching Layer
 │
 │  Error prioritization
 │  Beginner-friendly feedback generation
 ▼
XR Feedback Layer
 │
 │  Breathing guide
 │  Stability indicators
 │  Ghost overlay
 │  Coach voice / text feedback
 ▼
Perceive → Correct → Retry
```

---

## 1. Data Collection Layer

The data collection layer captures the user's motion, physiological state, and physical interaction with the bow interface.

### XR Tracking

The system uses the 6DoF pose of the HMD and both hand controllers.

Tracked values:

| Signal                    | Source                                      | Usage                            |
| ------------------------- | ------------------------------------------- | -------------------------------- |
| Aim direction             | HMD + bow hand controller                   | Estimate shooting direction      |
| Draw length               | Distance between both controllers           | Estimate bowstring pull distance |
| Anchor distance           | HMD to draw-hand controller distance        | Estimate anchor consistency      |
| Release timing            | Trigger / gesture event                     | Detect shot moment               |
| Follow-through trajectory | Draw-hand controller movement after release | Evaluate release quality         |

Directly measurable tracking values are prioritized over uncertain full-body pose estimation.

### Heart-Rate Sensing

A BLE heart-rate strap, such as Polar H10, provides:

* heart rate
* RR interval
* pre-release physiological stability

The system uses heart-rate trend rather than a single instantaneous value.

### Breathing Guide

Breathing is not directly measured in the minimum prototype.
Instead, the system provides a visual breathing guide ring in XR.

Breathing rhythm:

text
Inhale → Hold → Exhale / Release


The breathing guide is used to help users stabilize before release.

### Physical Tension Interface

An elastic resistance rig connects the draw hand to either:

* the bow hand controller, or
* a chest harness

As the user pulls the virtual bowstring, elastic resistance increases.
This simulates bowstring tension and helps the user physically experience draw force and force distribution.

---

## 2. Analysis Layer

The analysis layer models a single shot as a state machine.

### Shooting State Machine

```text
Ready → Draw → Anchor → Aim → Release → Follow-through → Review
```

| Phase          | Detected by                   | Main Evaluation                 |
| -------------- | ----------------------------- | ------------------------------- |
| Ready          | User idle posture             | Initial stability               |
| Draw           | Increasing hand distance      | Draw length                     |
| Anchor         | Draw hand near face/HMD       | Anchor consistency              |
| Aim            | Stable draw and aim direction | Aim stability, heart-rate trend |
| Release        | Trigger or release gesture    | Release timing                  |
| Follow-through | Post-release hand trajectory  | Follow-through quality          |
| Review         | Shot result generated         | Cause-oriented feedback         |

---

## 3. Release Ready Estimation

The system determines whether the user is ready to release based on multiple factors.

Example criteria:

| Factor               | Indicator                                     |
| -------------------- | --------------------------------------------- |
| Draw stability       | Draw length remains stable                    |
| Anchor consistency   | Anchor distance variance remains low          |
| Aim stability        | Bow hand movement remains within threshold    |
| Heart-rate stability | HR does not sharply increase before release   |
| Release timing       | Release occurs after stable aiming period     |
| Follow-through       | Draw hand moves backward in a controlled path |

Output states:

```text
Release Ready
Needs Stabilization
Needs Breathing Reset
Needs Anchor Correction
Needs Follow-through Correction
```

---

## 4. AI Coaching Layer

The rule-based analysis module generates structured data, such as:

```json
{
  "shot_id": 12,
  "score": 6,
  "phase": "review",
  "heart_rate": {
    "pre_release_avg": 124,
    "trend": "rising"
  },
  "tracking": {
    "draw_length_stability": "unstable",
    "anchor_distance_variance_cm": 3.1,
    "follow_through": "non_linear"
  },
  "error_flags": [
    "Anchor_Unstable",
    "HeartRate_Rising",
    "FollowThrough_NonLinear"
  ]
}
```

The LLM coaching module converts this raw data into beginner-friendly feedback.

Example output:

> Your heart rate rose while aiming, and your anchor position shifted before release. Lower the bow, take one slow breath, and anchor again before shooting.

---

## 5. XR Feedback Layer

The XR feedback layer provides feedback before, during, and after the shot.

### During Aiming

* breathing guide ring
* subtle heart-rate color indicator
* aim stability indicator
* Release Ready signal

### Immediately After Release

* ghost overlay of ideal vs. actual movement
* shot result
* cause-oriented explanation
* next-action coaching

### Across Repeated Attempts

* session history
* repeated error patterns
* improved stability indicators
* adaptive coaching priority

---

## Safety and Privacy Notes

* The system is a research prototype, not a replacement for professional coaching.
* Physical resistance should start at low tension.
* Heart-rate data should be stored locally by default.
* Session logs should avoid personally identifying information unless explicit consent is provided.
* LLM prompts should not include unnecessary personal data.

---

## Planned Modules

```text
unity/
  XR training environment
  bow interaction
  tracking data logger

hardware/
  tension rig guide
  optional FSR grip module

prompts/
  LLM coaching prompt
  feedback examples

experiments/
  pilot study protocol
  sample session logs
```
