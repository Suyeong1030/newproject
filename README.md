# Archery_Project


# ReleaseReady-XR

**Adaptive XR archery training system with biofeedback, force-feedback bow interface, and AI coaching.**

ReleaseReady-XR is an open-source research prototype for self-guided archery training in XR.
It helps beginners understand how posture, heart rate, draw stability, release timing, and physical tension affect shooting outcomes.

Unlike conventional VR archery games that focus mainly on aiming and scoring, ReleaseReady-XR provides a closed learning loop:

> **Perceive → Correct → Retry**

Users train in a virtual archery range, receive real-time guidance during aiming, and review cause-oriented feedback after each shot.


## Overview

Archery performance is not determined by aiming alone. A stable shot requires coordinated control of posture, breathing rhythm, heart rate stability, draw length, anchor consistency, release timing, and follow-through.

However, many of these factors are difficult for beginners to perceive in real time.
A beginner may know that an arrow missed the target, but may not understand whether the cause was unstable anchoring, elevated heart rate, poor release timing, or inconsistent force distribution.

ReleaseReady-XR addresses this problem by integrating:

* **XR-based archery simulation**
* **6DoF tracking from HMD and hand controllers**
* **BLE heart-rate sensing**
* **Elastic tension-based physical bow interface**
* **Rule-based shooting analysis**
* **LLM-based coaching feedback**

---
### XR Archery Training Environment

Users can practice archery without access to a physical range.
The XR environment provides a repeatable training space where aiming, draw, release, and follow-through can be observed and analyzed.

### 6DoF Posture and Motion Tracking

The system uses HMD and hand controller tracking to estimate:

* Aim direction
* Draw length
* Anchor distance
* Release timing
* Follow-through trajectory

Directly measurable tracking values are prioritized over uncertain full-body pose estimation.

### BLE Heart-Rate Integration

ReleaseReady-XR supports BLE heart-rate straps such as Polar H10.
Heart rate and RR interval data are used to estimate physiological stability before release.

### Elastic Tension-Based Bow Interface

A mechanical elastic rig provides physical resistance while drawing the bow.
As the user pulls the virtual bowstring, the elastic resistance increases, allowing the user to physically experience draw tension and force distribution.

At release, elastic relaxation and controller haptics provide a tactile shooting sensation.

### Release Ready Detection

The system evaluates whether the user is ready to release based on:

* Draw stability
* Anchor consistency
* Aim stability
* Heart-rate trend
* Release timing
* Follow-through quality

If the system detects instability, it provides corrective feedback before or after the shot.

### AI Coaching Feedback

Rule-based analysis generates structured error flags.
An LLM-based coaching module converts these raw signals into beginner-friendly feedback.

Example:

```json
{
  "heart_rate": 132,
  "heart_rate_trend": "rising",
  "error_flags": ["Anchor_Unstable", "FollowThrough_NonLinear"],
  "score": 5
}
```

Coaching output:

> Your heart rate rose sharply during aiming, and your anchor position shifted before release. Lower the bow, take one slow breath, and try anchoring again before shooting.

---

## System Architecture

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
 │  Draw tension interaction
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
 │  Stability cues
 │  Ghost overlay
 │  Coach voice / text feedback
 ▼
Perceive → Correct → Retry


## Hardware Components

Minimum setup:

* XR HMD
* Two hand controllers
* BLE heart-rate strap
* Elastic resistance band for tension feedback

Recommended setup:

* Meta Quest or PC VR headset
* Polar H10 or compatible BLE heart-rate strap
* Elastic tension rig connecting draw hand and bow hand or chest harness
* Optional FSR pressure sensor for grip tension analysis

---

## Software Stack

* Unity
* XR Interaction Toolkit
* BLE heart-rate data receiver
* Rule-based shooting phase detector
* LLM coaching prompt pipeline
* Optional Arduino / ESP32 module for grip sensing

---

## Repository Structure

```text
ReleaseReady-XR/
├── unity/                 # Unity XR prototype
├── hardware/              # Tension rig and optional sensor guides
├── prompts/               # LLM coaching prompts and examples
├── docs/                  # Architecture, setup, and experiment documentation
├── experiments/           # Study design and sample logs
├── scripts/               # Data parsing and analysis utilities
└── media/                 # Demo images, videos, diagrams
```

---

## Research Use

This project is intended for research and prototyping in:

* Sports HCI
* XR training systems
* Biofeedback interaction
* Physical computing
* LLM-based coaching interfaces
* Adaptive skill learning systems



