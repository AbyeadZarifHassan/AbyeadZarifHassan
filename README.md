# Abyead Zarif Hassan

Robotics and Mechatronics Engineering graduate from the University of Dhaka (**CGPA: 3.76/4.00**), interested in building intelligent systems that can perceive, reason and act in the physical world.

My work sits at the intersection of **computer vision, deep learning, multimodal AI, vision-language models, embodied AI, autonomous navigation, perception and machine learning**. My undergraduate thesis explored whether vision-language models could make obstacle-avoidance decisions fast and reliably enough to operate inside a real UAV control loop.

I am seeking **MSc, MASc and PhD opportunities for Fall 2027**.

![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)
![PyTorch](https://img.shields.io/badge/PyTorch-EE4C2C?style=flat&logo=pytorch&logoColor=white)
![OpenCV](https://img.shields.io/badge/OpenCV-5C3EE8?style=flat&logo=opencv&logoColor=white)
![ROS 2](https://img.shields.io/badge/ROS%202-22314E?style=flat&logo=ros&logoColor=white)
![Unity](https://img.shields.io/badge/Unity-000000?style=flat&logo=unity&logoColor=white)
![C++](https://img.shields.io/badge/C++-00599C?style=flat&logo=cplusplus&logoColor=white)

[![Email](https://img.shields.io/badge/Email-hassanabyeadzarif%40gmail.com-EA4335?style=flat&logo=gmail&logoColor=white)](mailto:hassanabyeadzarif@gmail.com)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Abyead%20Zarif%20Hassan-0A66C2?style=flat&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/abyead-zarif-hassan-1744b4211/)

---

## Research interests

- **Multimodal and vision-language AI:** combining visual observations, depth and robot state for spatial reasoning and decision-making.
- **Embodied AI and autonomous navigation:** developing perception-to-action systems that operate safely on real robots in dynamic environments.
- **Computer vision and deep learning:** monocular depth estimation, scene understanding and efficient foundation-model inference under strict latency constraints.
- **Machine learning and reinforcement learning:** learning navigation policies from multimodal observations, using foundation models as priors, teachers or reward-shaping signals.

---

## Featured research

### VLM-guided UAV obstacle avoidance

**Undergraduate thesis:** *Advancing UAV Flight and Obstacle Avoidance with Cross-Modal Deep Learning Integration*  
**Co-author:** Samiha Tarannum Noor  
**Supervisor:** Dr. Md Mehedi Hasan, University of Dhaka · 

I developed an end-to-end autonomous UAV system in which a vision-language model reasons over an RGB frame and a monocular depth map, selects one of eleven navigation commands, and sends it to a Pixhawk 6C flight controller through MAVLink. Computation runs offboard, allowing a small airframe to use models that cannot be deployed on its onboard hardware.

I benchmarked six vision-language models using the same task, prompt and model-agnostic command parser:

- **Cloud:** GPT-5, GPT-4o and Gemini 2.0 Flash
- **Local:** LLaVA-7B, Llama 3.2-Vision and Qwen2.5-VL-3B

The simulation benchmark covered **265 episodes, 478 shared decision points and nine factorially designed scenes**, varying obstacle density, lighting, weather and obstacle motion. Models were evaluated using obstacle-avoidance accuracy, false-positive rate, path efficiency, safe-action alignment, collision rate, decision latency and trajectory complexity, with a geometric controller as the baseline.


### System highlights

- Unity simulation environment with lighting, fog, rain and dynamic obstacles
- Identical message interfaces for simulated and physical sensors
- DepthAnything-V2 monocular depth estimation with nine-region spatial analysis
- Model-agnostic parsing of free-form VLM responses into executable commands
- Finite-state safety layer with tiered proximity thresholds, altitude limits, timeout fallback, manoeuvre escalation and operator override
- Mission and waypoint handling through the MAVLink mission protocol
- Multi-threaded ground station for video, depth, telemetry and decision monitoring
- Simulation-based model screening followed by real flights on the University of Dhaka campus

The hardware trials used deliberately simpler obstacle arrangements than the simulation benchmark to limit the risk of damaging the aircraft. I therefore treat the simulation and hardware results as complementary evaluations rather than directly comparable measurements.

`Python` `PyTorch` `OpenCV` `ROS 2` `Unity` `MAVLink` `Pixhawk`

> The source code is currently private while I prepare a paper. I am happy to provide access or demonstrate the system upon request.

---

## What I want to investigate next

- **Temporal reasoning for dynamic environments:** helping multimodal models understand motion and state history instead of treating every frame independently.
- **Efficient local inference:** using distillation, quantisation and model cascades to reduce latency without losing spatial-reasoning ability.
- **Learning from foundation models:** using VLM outputs and confidence as priors or training signals for reinforcement-learning policies rather than directly obeying every model decision.

---

## Experience and leadership

- **Intern, Bangladesh Satellite Company Limited:** gathered data-processing and machine-learning experience from the national satellite network's TRP segment; supported SOCC, NOCC and RF-system operations; configured and diagnosed multiple Starlink terminal types.
- **Vice President, RMEDU Student Club:** led strategy and technical operations and mentored teams entering inter-university robotics competitions.
- **Secretary, IEEE Robotics and Automation Society Student Branch Chapter:** coordinated student members, faculty advisers and IEEE leadership.

---

## Selected projects

### [Automatic License Plate Recognition](https://github.com/AbyeadZarifHassan/Number-Plate-Recognition)

Built a classical computer-vision pipeline for plate localisation, character segmentation and OCR without a pretrained detector. Evaluated its limitations under low light, noise and motion blur.  
`Python` `OpenCV`

### 3-DOF pick-and-place manipulator

Modelled a robotic arm in SolidWorks, derived its inverse kinematics and implemented PID-based joint control using an Arduino Nano.  
`SolidWorks` `Arduino` `C++`

### [All-in-One Shop Management System](https://github.com/AbyeadZarifHassan/All-in-One-Restaurant-App)

Developed an object-oriented inventory and order-management application backed by a relational database.  
`Python` `SQL`

---

## Technical toolkit

| Area | Tools and technologies |
| --- | --- |
| Programming | Python, C, C++, MATLAB, SQL |
| AI and perception | PyTorch, OpenCV, vision-language models, monocular depth estimation, prompt design |
| Robotics and control | ROS 2, MAVLink, Pixhawk, PX4, Mission Planner, Arduino, PID control, inverse kinematics |
| Simulation and modelling | Unity, SolidWorks, AutoCAD, Simulink, Proteus, COMSOL |
| Research and development | Git, GitHub, LaTeX, experimental design and performance evaluation |

---

## Contact

I am open to research opportunities and collaborations in computer vision, multimodal learning, embodied AI and autonomous navigation.

- **Email:** [hassanabyeadzarif@gmail.com](mailto:hassanabyeadzarif@gmail.com)
- **LinkedIn:** [Abyead Zarif Hassan](https://www.linkedin.com/in/abyead-zarif-hassan-1744b4211/)
