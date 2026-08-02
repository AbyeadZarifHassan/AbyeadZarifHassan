<!-- Goes in the repository named exactly: AbyeadZarifHassan -->

# Abyead Zarif Hassan

I finished my B.Sc in Robotics and Mechatronics Engineering at the University of Dhaka in 2026.

For my thesis I put a vision-language model in charge of a drone's obstacle avoidance and then flew it. The question was whether a model that large can decide fast enough to sit inside a control loop. It can, but only if you run it off the aircraft, and only some models.

Answering that properly meant building most of the apparatus myself: a simulation environment in Unity wired to ROS 2, a nine-scene factorial benchmark, the safety layer, the mission and waypoint handling, a ground station, and then the flight campaign on campus. It took the better part of a year. I'd do it again.

I'm applying for MSc, MASc and PhD positions starting Fall 2027.

![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)
![PyTorch](https://img.shields.io/badge/PyTorch-EE4C2C?style=flat&logo=pytorch&logoColor=white)
![OpenCV](https://img.shields.io/badge/OpenCV-5C3EE8?style=flat&logo=opencv&logoColor=white)
![ROS 2](https://img.shields.io/badge/ROS%202-22314E?style=flat&logo=ros&logoColor=white)
![Unity](https://img.shields.io/badge/Unity-000000?style=flat&logo=unity&logoColor=white)
![C++](https://img.shields.io/badge/C++-00599C?style=flat&logo=cplusplus&logoColor=white)

---

## What I work on

**Vision-language models, multimodal AI**

I benchmarked six models as the reasoning engine inside a flight control loop, across two deployment categories: cloud APIs (GPT-5, GPT-4o, Gemini 2.0 Flash) and locally-run open source models (LLaVA-7B, Llama 3.2-Vision, Qwen2.5-VL-3B). Same task, same prompt, same parser for all of them.

The prompt uses chain-of-thought instruction, so the model has to articulate what it sees and why before it commits to a direction. That was a deliberate choice: an opaque command is untraceable when something goes wrong, and I wanted a log I could read afterwards. The parsing layer that turns free-form model output into an executable command is model-agnostic, so a new VLM drops in without touching the control pipeline. Getting that layer reliable enough to trust every single time was most of the work.

**Embodied AI, autonomous navigation**

The full loop, camera to command, on real hardware and not only in simulation.

Most of the engineering ended up being safety. A finite state machine governs the avoidance sequence. Proximity thresholds are tiered rather than binary. There's an altitude envelope, an attempt counter that escalates when a manoeuvre keeps failing, a heuristic fallback for when the model doesn't answer in time, and an operator override available at any point. The VLM decision refreshes every thirty frames instead of every frame, with temporal smoothing, because per-frame replanning made the aircraft oscillate. You want all of that in place before letting a language model near something with propellers.

**Computer vision, perception**

Depth comes from a single camera through DepthAnything-V2, a ViT-L/14 encoder with a DPT decoder. No LiDAR, no stereo pair. The camera field of view is split into nine regions so depth becomes a spatial decision signal rather than one number, and frames come off a 1080p60 FPV link downsampled to 640x480 before inference to stay inside the latency budget.

Before that I built a licence plate reader out of classical CV, thresholding and contours and segmentation and OCR, then spent most of the time breaking it on purpose. That was the most useful thing I did early on, because it showed me exactly what learned perception is replacing.

**Deep learning, machine learning**

PyTorch, foundation-model inference under a hard latency budget, and the evaluation design.

Nine scenes varying obstacle density, lighting, weather and whether obstacles moved, arranged factorially so I could attribute a performance difference to a cause instead of guessing. One design decision I'm still glad about: the simulated sensors publish the same message types as the real ones, so simulation and hardware run through identical code paths. If sim and real disagreed, it couldn't be blamed on the sensor interface.

**Reinforcement learning**

No trained policy yet. But the problem is already framed as one, and I'd rather show that than claim experience I don't have.

The thesis defines an observation space (RGB frame, depth map, roll, pitch, yaw, altitude, GPS, velocity, plus a window of previous states for temporal context) and a discrete action space of eleven commands. Everything runs in episodes, 265 of them in the benchmark. I evaluated on path efficiency, collision rate, safe-action alignment and decisions per episode, against a hand-coded geometric baseline.

So there is an environment, an action space, a metric set, and a strong prior policy in the VLM. The missing piece is the learning, and that is precisely why it's the thing I most want to be trained in.

---

## What I'd like to work on next

**Obstacles that move.** Everything I tested got noticeably worse once obstacles started moving and scenes got crowded, and the reason isn't subtle. These models reason one frame at a time and have no idea where anything is heading. Underneath the robotics, this is a temporal representation problem: the models were trained on single images and have no real mechanism for grounding across time. My observation space carries a window of previous states, but a frozen image-trained model barely attends to it. Video-native architectures, or better ways of encoding state history so an existing model can actually use it, are what I want to dig into.

**Speed without the cloud.** The open source models I ran locally took minutes per decision on a 4GB card. That isn't a tuning problem, it's a different regime. Leaning on a cloud API for obstacle avoidance is also a bad idea the moment you lose signal, which is exactly when you need it. Framed as a modelling question, it becomes one I find genuinely interesting: how far can a vision-language model be compressed before its spatial reasoning falls apart? Distillation from a large teacher, quantisation, or a cascade where a small fast model handles the common case and defers upward when uncertain.

**Learning from the model instead of obeying it.** A VLM gives you a sensible guess about which way to go. It tells you nothing about whether the path it chose was efficient. That makes it a prior, not a policy. Using a foundation model to initialise or shape a learned policy, and using its own stated confidence as a signal rather than throwing it away, is the direction I most want training in. My thesis logged that confidence on every decision and never did anything with it.

---

## Thesis

*Advancing UAV Flight and Obstacle Avoidance with Cross-Modal Deep Learning Integration*
Supervised by Dr. Md Mehedi Hasan, University of Dhaka, April 2026

One camera feeds DepthAnything-V2. The RGB frame and the depth map go to a vision-language model together, with a prompt that makes it explain its reasoning before committing to a move. What comes back gets parsed into one of eleven commands and sent to a Pixhawk 6C over MAVLink. All the heavy computation happens off the aircraft, which is the whole point: a small airframe can't carry the compute, and it doesn't have to.

Two phases. First a simulation benchmark to screen all six models across the nine scenes, then hardware flights on the university campus with the ones that survived. Screening in simulation first meant I never risked the aircraft on a model that was never going to work.

**What I built for it**

- Unity simulation environment (URP for lighting, fog and rain; URDF import for the airframe; simulated RGB, depth, IMU and GPS sensors)
- Unity to ROS 2 Humble bridge, with the reasoning loop, telemetry monitor and command input running as concurrent processes
- Depth pipeline with nine-region field-of-view segmentation
- Prompt design and the model-agnostic command parser
- Safety layer: state machine, tiered thresholds, altitude envelope, timeout fallback, escalation, manual override
- Mission and waypoint subsystem over the MAVLink mission protocol, with geospatial offset computation and an interactive map
- Multi-threaded ground station showing live video, the depth map, telemetry and a decision log

One thing worth saying plainly: the hardware results came out better than the simulation ones. That sounds good until you know the outdoor sites were deliberately chosen to be simpler, single obstacles with clear space to escape sideways, because crashing a real quadrotor into a cluster of obstacles is expensive. The two sets of numbers aren't comparable and I don't present them as though they are.

`Python` `PyTorch` `OpenCV` `ROS 2` `Unity` `MAVLink` `Pixhawk`

> The code is private for now while I work on a paper from it. Happy to give access or walk anyone through the system who asks. It goes public once the paper does.

---

## Other things I've built

**[Automatic License Plate Recognition](https://github.com/AbyeadZarifHassan/Number-Plate-Recognition)**
A plate reader with no pretrained detector anywhere in it. Preprocessing, character segmentation, then OCR, all classical. I spent most of the time breaking it on purpose, dim light, added noise, motion blur, to find where hand-tuned features give up. `Python`, `OpenCV`

**3-DOF Pick-and-Place Manipulator**
Modelled the arm in SolidWorks, worked out the inverse kinematics so I could give it a position instead of three angles, and ran it off an Arduino Nano with PID control tuned to stop it overshooting. `SolidWorks`, `Arduino`, `C++`

**[All-in-One Shop Management System](https://github.com/AbyeadZarifHassan/All-in-One-Restaurant-App)**
Inventory and orders in Python, object-oriented, sitting on a relational database. Replaced a paper ledger. `Python`, `SQL`

---

## Tools

- **Languages:** Python, C, C++, MATLAB
- **Machine learning and vision:** PyTorch, OpenCV, monocular depth estimation, vision-language models, prompt design
- **Robotics and control:** ROS 2 Humble, MAVLink, Pixhawk and PX4, Mission Planner, Arduino, PID control, inverse kinematics, PLC
- **Simulation and modelling:** Unity (URP, URDF), SolidWorks, AutoCAD, Simulink, Proteus, COMSOL
- **Development:** Git, GitHub, LaTeX

---

## Contact

[hassanabyeadzarif@gmail.com](mailto:hassanabyeadzarif@gmail.com), [LinkedIn](https://www.linkedin.com/in/abyead-zarif-hassan-1744b4211/)

Computer vision, vision-language reasoning and autonomous navigation, carried end to end onto real hardware. Get in touch if that's your area.
