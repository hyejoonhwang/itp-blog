
For my midterm, I’m making a blank die that _talks_.  

It’s inspired by the Magic 8 Ball and decision dice — but with a twist. Unlike normal dice covered with numbers or dots which are made for people who can see, this one has **nothing on its faces**.

The idea is that **whether you have vision or not, everyone gets the result together**. You ask a question, roll the die, and when it settles, it responds with **sound** instead of visuals.

The idea came from thinking about **accessibility in play and interaction design** — how to make something as simple as rolling a die feel fair, shared, and a little bit magical for everyone in the room.

#### Conversation with David Rios

I talked with David Rios to get a sense of how to start my project — what to research first, what tools to learn, and how to plan the overall workflow. I wasn’t sure how to approach this project from start to finish, so I asked him to help me map out a general pipeline.

#### What to Do

**1. Start with the Code (Electronics)**
- Learn how the **IMU module** works. Try simple Arduino examples to read acceleration and orientation data.
- Test whether you can detect which side of the die is facing upward.
- Focus on making this part work before worrying about the physical design.
    

**2. Prototype the Form**

- Use **cardboard boxes or Styrofoam** to create quick, lightweight prototypes.

- Check whether rolling the die actually gives readable IMU data and stable orientation detection.
    
- This helps confirm that the sensing logic is reliable before moving into fabrication.
    

**3. Move on to Fabrication**  
Once the electronics and sensing logic are proven:

- **Mounting and structure:**
    - Figure out how to fix the Arduino, speaker, and battery securely inside the die.
    - Look up **“standoffs”** for mounting, and check **Arduino hole size** (M2.5 or M3 screws).
        
- **Power:**
    - Decide on the power source — **LiPo battery** or **9V battery**.
    - The battery connects to the **VIN pin** on the Arduino.
        
- **Wiring:**
    - Plan how to connect everything neatly. Consider using a **solderable breadboard (perfboard)** for stability.


**Things to buy
1. Bread board mini
2. Solderable breadboard
3. Batteries
4. battery adapter
5. Speakers
6. standoffs