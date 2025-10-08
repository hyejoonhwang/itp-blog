Most VR gym experiences are static — you walk into a fully built space and just observe.  
I wanted to explore what happens when we give the user control — when they can _design, build, and arrange their own gym environment_ directly in VR.
For my midterm, I’m creating a **customizable VR gym** experience.  
The scene starts as an empty gym environment built from Unity Asset Store assets.

In front of the player is a **UI panel made with the Interaction SDK**, showing several buttons — each representing a different piece of gym equipment like a treadmill, dumbbells, or a yoga mat.

When the player clicks a button, the corresponding equipment appears in the gym.  
Then, using **hand or ray grab interactions**, the player can _pick up and place_ the machines anywhere they want — creating their own layout.
This project focuses on implementing **ISDK UI and object interactions** together.  
Each button triggers a prefab spawn event, and the spawned object can be grabbed, moved, and dropped into position using **ISDK grab interactions**.

The main goal is to show how VR interfaces can connect digital UI and physical interaction — building an environment that responds to the player’s input.
This midterm is a foundation for my final project, where I plan to expand into **dynamic gym interactions** — like equipment responding to player movement, or even collaborative multiplayer workouts.

But for now, the focus is on building the core system that lets players customize and physically arrange their own VR gym.