 ## **Final Motion Capture Project — Capturing Movement in Gym Workouts**

### **Project Context**

Our final project — **“Capturing Movement in Gym Workouts”** — explores the nuances of athletic motion through motion capture.  
The idea was to record realistic gym movements (push-ups, squats, yoga poses, balance ball work, etc.) and bring them into a stylized virtual environment. Our focus was both **technical** (recording, streaming, rigging, and retargeting workflows) and **aesthetic** (expressing workout through body motion and visualize with metahuman).

---

### **Streaming to Unreal Engine: Our Week 6 Focus**

During Week 6, our team (Michael, Ryan, Galt, and I) concentrated on one major technical goal — **live streaming full-body, hand, and facial motion data directly into Unreal Engine**.

We wanted to bypass the traditional MotionBuilder workflow and instead test **real-time retargeting** through **OptiTrack → Unreal via Live Link**, combining:

- **OptiTrack Motive** for body motion capture
- **Manus Quantum Metagloves** for high-fidelity hand and finger tracking
- **Unreal Live Link Face** for facial motion streaming
- **MetaHuman** as the target character for visualization
    

In theory, this setup would allow us to see our captured performer instantly drive a MetaHuman inside Unreal — no post-baking or manual retargeting required.

---

### **Pipeline Overview: Real-Time Route vs Traditional Pipeline**

To understand the difference, here’s how the two pipelines compare:

**MotionBuilder Workflow**

1. Record body data in **OptiTrack Motive**
2. Import to **MotionBuilder**
3.  **Rigging & Retargeting** onto a skeleton
4. Import baked animation to **Unreal Engine** and apply it to our MetaHuman
    

**Real-Time Streaming Workflow (Our Goal)**

1. Stream body data from **OptiTrack** to **Unreal** using **Live Link**
2. Pair the live skeleton to a **MetaHuman Control Rig**
3. Add **Manus Glove data** and **facial Live Link** feeds
4. Visualize the performance live within Unreal
    

Essentially, the second approach collapses the pipeline into a single step — ideal for virtual production, where you can preview or even record in real time.

---

### **Week 6 Session – October 4**

For this session, I invited a friend to perform as our motion-capture actor.  
After calibrating the capture volume, we attached the **Manus Metagloves** and set up **Live Link Face** on an iPhone. The Manus gloves connected successfully to Motive through the **Manus Plugin**, and we could see clean hand and finger tracking data.

However, the body skeleton from OptiTrack did **not align correctly** with the MetaHuman rig inside Unreal. The naming conventions and bone hierarchy between Motive and MetaHuman skeletons differed too much for automatic mapping.  
We spent roughly an hour troubleshooting — checking retargeting setups, bone axes, and Live Link sources — but the data still didn’t sync cleanly. Eventually, we decided to **pause the live-streaming route** and focus instead on perfecting the **body + hand capture** in Motive itself.

So, for the remainder of the week, we captured clean body and hand data to use later in MotionBuilder and Unreal.

---

### **Week 7 – Final Session (October 10)**

By our last session, we had already captured enough motion assets for the final video collage — multiple gym actions from different performers.  
Our main focus became **retargeting** and **polishing**.

Since the **live streaming pipeline didn’t work reliably**, we returned to the standard workflow:

1. Exporting OptiTrack data to **MotionBuilder**
2. Performing **rigging and retargeting** onto our MetaHuman skeleton
3. Baking and exporting animations back into **Unreal Engine**

This gave us full control over cleanup and keyframe accuracy. It also ensured the motion quality stayed intact for editing and rendering.

---

### **Reflection**

This project taught us how complex real-time motion pipelines can be.  
While the **Manus Gloves** and **OptiTrack Motive** integrated beautifully on their own, combining them with **Live Link Face** and **MetaHumans** introduced synchronization challenges that go beyond simple data streaming — skeleton naming, bone mapping, and rig structure all need tight compatibility.

Still, the attempt was valuable. We now understand **both** workflows:  
the **traditional retarget-and-bake approach** for reliability and the **real-time streaming route** for future virtual-production flexibility.

Next step — we plan to compile our captured motions into a **“Gym Workout Collage”** video piece that visually compares captured, retargeted, and rendered outputs, showcasing how motion capture can translate physical effort into digital expressiveness.

---

### **Credits**

**Team:** Michael Katz, Ryan Burke, Galt Almogy, Summer Hwang  
**Capture Date:** September 22 & 26, October 4 & 10, 2025  
**Tools:** OptiTrack Motive + MotionBuilder + Manus Metagloves + Unreal Engine + MetaHuman + LiveLink
**Project:** _Capturing Movement in Gym Workouts_




--------------------------------------------------------------------------
This is our final submission for MoCap for Archive class highlighting our captured body motions. However, our team plans to continue refining the visual presentation in Unreal Engine — polishing lighting, camera work, and rendering to better communicate the energy and realism of the gym movements.

![[WhatsApp Video 2025-10-15 at 13.50.07_a4bde9b2.mp4]]