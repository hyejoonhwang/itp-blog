
# **FairFit VR: From UX Concept to Interactive VR Prototype**

**Imagine walking into a gym — you’re motivated, ready to train — but every bench, squat rack, and treadmill is full.**  
You end up waiting, pacing, or simply giving up.

My project, **FairFit VR**, reimagines this experience through an **interactive virtual gym** designed to demonstrate what a _smart gym recommendation system_ could feel like.

---

##  Concept Overview

**FairFit** began as a UX project that explores how to make shared gym spaces more efficient and fair.  The system aims to reduce frustration and wasted time caused by long waits for popular equipment.
It uses **smart occupancy tracking** and **AI-based workout recommendations** to:
- show which machines are available,
- estimate waiting times, and
- suggest alternative exercises that target the same muscles.

The goal is to make gym time more efficient and engaging — transforming **waiting into training**.  
Users can track progress, earn small rewards for flexible decisions, and feel more in control of their workouts.

---

##  Transition to VR

For my **VR prototype**, I’m bringing the FairFit concept into a virtual environment where users can _see_, _choose_, and _move_ just as they would in a real gym.

In the **FairFit VR demo**, users can:
- **Look around** a 3D gym space.
- **Select machines** through simple, interactive UI panels.
- **Instantly see** how many people are waiting, the estimated wait time, and recommended alternative machines.
- **Choose to “wait”**, which displays a visual queue or timer.
- Or **“try alternative”**, where glowing arrows guide the user toward another available machine.

This prototype simulates the _logic_ of FairFit’s real-world service — showing how VR interfaces can connect digital UI with physical interaction.  
The idea is to make users experience the system, not just imagine it.

---

##  Midterm Goals

For my **midterm**, I plan to build:
1. A **simple gym environment** with about six machines (bench, squat, treadmill, etc.).
2. **Interactive UI panels** that display simulated, dynamic data (occupancy and wait time).
3. Two core user actions:
    - **“Wait here”** → shows queue feedback or timer.
    - **“Try alternative”** → guides the user to another machine via a glowing path.

The main goal for this stage is to explore how **VR interfaces** can merge **digital feedback** and **spatial interaction**, building a foundation for a more dynamic and responsive gym experience in the final project.

---

##  Next Steps (for Final Project)

In the final phase, I plan to expand FairFit VR with:
- **Full-body workout interactions** (push-ups, squats, lunges, twists, etc.)
- **Motion tracking** using colliders or sensors to detect user movement.
- **Dynamic feedback loops** that respond to real-time player actions.

Ultimately, the project aims to connect **UX design, VR technology, and physical engagement** — creating a smarter, more immersive gym experience for the future.


10/9
Michelle comment:
	Try Supernatural VR in meta horizon - 이거 일햇던 사람들 소개 가능
		collider을 machine part 에 둬서 user movement 를 track 할수 있을지 creative thinking 해보는것도 좋을듯. 
			yogamat (push up, 윗몸일으키기), 스쿼트, 런지, 덤벨잡고 twist, step up, burpees, benchpress, plank
	근데 이건 파이널 과제..! 믿텀부터 생각하자.ㅎㅎㅎ		




# **The logic behind the build of the scene**
### 1. **Walk up to a workoutZone**
- When user approaches to one of the workout machine, the designated panel appears.
- Interact via **ray pointer** (controller) or **poke** (hand) — from Meta Interaction SDK.
- When you point at a machine:
    - The panel lights up / hovers slightly (visual feedback).
    - Text Shows:
        -  Current people in line (e.g., “3 users waiting”)
        -  Estimated wait time (“≈ 5 min”)
        -  Recommendation (“Try Cable Row instead”)
	- Button Suggests:
		- 1. **Get in line**
		- 2. **Try alternative routine.**
### 2. **Press “Get in line”**
- A small **holographic queue circle** appears on the floor saying “You’re #4 in line.”
- Play music when waiting.
- Optional: Timer animation or progress bar starts (“~6 minutes” countdown simulation).
### 3. **Press “TRY ALTERNATIVE”**
- button appears with all the options of alternative routines.
- When the user selects one of the options then panel closes → mark on the chosen alternative machine appears (e.g., either an arrow or any kind of mark) guiding you to the suggested machine.
- Optional narration or floating text:
    > “Try Dumbbell Row — similar muscle group, 4 minutes faster.”

When you arrive:
- The target machine UI auto-opens (“Available Now!”).
- Soft sound cue (or haptic vibration) = confirmation feedback.
### 4. (optional) **Global Information Board**

At the gym entrance or near spawn:

- Large display that shows all machines + wait times updating every few seconds.
- You can interact (scroll or filter by muscle group).
- Demo button: “Peak Hour / Off-Peak” toggle (simulated data refresh).





