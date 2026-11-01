<img width="2160" height="1440" alt="MA2-BannerLogoFixed" src="https://github.com/user-attachments/assets/791e7b0e-c32a-4a85-9884-9ea314dff82b" />
https://github.com/user-attachments/assets/264021d8-f86c-45ed-8f32-099cde2dcee9


# MissionAir

An open-source, self-piloted drone platform with onboard edge AI (object detection + embedding-based command matching) and server-side LLM reasoning over Tailscale.

Built for civil-purpose missions as part of a school assessment. Weapons-adjacent functions are explicitly out of scope.

## Overview

MissionAir combines a lightweight, lean-rebuild airframe with a two-tier AI architecture:

- **Onboard (Raspberry Pi 5 + Hailo-8L, 13 TOPS)** — real-time object detection and a fast embedding-based command matcher (`sentence-transformers/all-MiniLM-L6-v2`, run via ONNX Runtime in C++) for simple direct commands.
- **Server-side (self-hosted, reached over Tailscale)** — a larger LLM handles multi-step, context-aware reasoning for gimbal control and mission decisions. Onboard embeddings are sent up rather than raw frames, keeping bandwidth low over a cellular link.

Gimbal control is custom-built rather than using MAVSDK's gimbal plugin — MG90 servos are driven directly via PWM, with a command-producer pattern so keyboard input and AI-generated commands both converge on the same `GimbalCommand` struct.

## Hardware

| Component | Spec |
|---|---|
| Frame | 3D-printed, ~240g, 500mm class |
| Flight controller | Matek H743-SLIM V3 |
| Companion computer | Raspberry Pi 5 (8GB) |
| Edge AI accelerator | Hailo-8L AI HAT+ (13 TOPS) |
| Battery | Generic 4S LiPo |
| Power | Dedicated UBEC for Pi 5, separate from flight controller BEC |
| Altitude sensor | TF-Luna |
| Gimbal | MG90 servos, custom PWM control (pitch/yaw/roll) |

Depth sensing (VL53L5CX) and audio (mic/speaker) are deferred to Phase 2.

Target build cost: ~$567 AUD.

## Software

On-drone software is a C++ and Python solution (`MissionAir Operative Software 1.0 LLM Integration`) covering:

- Gimbal control (`GimbalController` / `GimbalCommand` / `GimbalInput`)
- Human/manual control input
- Onboard object detection + search behavior
- Embedding-based command matching and LLM tool integration
- A simplified/demo operating mode

LLM reasoning runs as a separate program on a self-hosted server, not embedded in the drone's own codebase — the drone sends structured requests over Tailscale and receives `GimbalCommand`-compatible decisions back.

## Status

Actively in development. Hardware and software architecture are still evolving as the build progresses.
Not a representation of what will be in the final source code. Trials in progress, hardware testing, etc.

## Scope

This project is built for civil-purpose use cases. Weapons-adjacent functionality is explicitly out of scope and will not be added.









<img width="2160" height="2160" alt="MA2Small-FixedFavicon" src="https://github.com/user-attachments/assets/947f174d-5048-49f4-97e9-17e436bac426" />


## License

MIT — see [LICENSE](LICENSE).
