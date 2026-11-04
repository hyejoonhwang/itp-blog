
* Interviewed five people who use LLMs as personal companions, not just for work tasks: Jueun, Michael, Jihee, Doyoon, and Niki

**Jueun**

* Represents the most common usage pattern seen across interviews  
* Most personal topics stay surface-level: how to text someone more politely, interpreting physical symptoms, seeking advice  
* Not offloading emotional weight; asking for answers, not support  
* Useful framing: may represent an entry point into more emotionally engaged AI use over time, not a disqualifying case

**Michael**

* Highly technical; built a personal agentic logging system  
  * Logs diet, workout routines, behavioral notes (e.g., an argument with a roommate)  
  * All data fed into AI to detect patterns he cannot see himself  
  * AI advises him on what to do differently based on detected patterns  
* Does vent occasionally within the log, but rarely reads the AI’s emotional response  
* Wants pattern detection and objective insight, not comfort or validation  
* Key tension: wants AI to give him facts, not agree with everything he says

**Jihee**

* In weekly psychiatry for deep past trauma; uses chat separately for everyday concerns  
* Chat topics: mostly relationship stress with her boyfriend, lightweight daily worries  
* Keeps the two completely separate by choice:  
  * Therapy topics feel too serious and structured for chat  
  * Chat fills the gap for things that feel “too silly” to bring to therapy, or that arise late at night when her therapist is inaccessible  
* Strong analogy she offered: purple diary (deep, heavy, sometimes afraid to open) vs. yellow diary (light, casual, anything goes)  
  * Wants any AI companion app to function as the yellow diary, not the purple one  
  * Would not want push notifications if the app carries emotionally heavy content: notifications would surface unwanted feelings in daily life  
* Based in Korea, so cannot currently access T (US-only, English-only)

**Doyoon**

* PhD researcher in mental health; former psychiatric nurse in Korea for three years  
* Uses chat for daily decompression, similar to Jihee, mostly relationship and everyday stress  
* Also uses a looser version of Michael’s pattern-detection approach: shares broad context about herself and asks AI to identify patterns she is not noticing  
* Very interested in AI helping her understand herself better  
* Suggested in-app psychology assessments as a feature idea:  
  * Quick, engaging tests (e.g., MBTI-style) that users can take to learn about themselves  
  * Data from assessments could enable more personalized AI interactions  
* Flagged a concrete usability problem: as chat sessions get longer, AI becomes increasingly sycophantic, agreeing with whatever she says  
  * Her workaround: wipe the session and start fresh to restore neutrality  
  * Implication: session length and AI stance calibration matter for trust  
* Skeptical of the TikTok-scroll engagement concept: concerned it could drive unhealthy dependency and addiction, coming from her clinical background

**Niki**

* Most emotionally engaged and heaviest user across all five interviews  
* Uses ChatGPT, Claude, and Gemini, and has settled on ChatGPT most often  
* Differentiates between them by purpose:  
  * ChatGPT: emotional support, warmth, comfort  
  * Claude: objective, direct, neutral advice  
  * Gemini: feels like it sits in between the two  
* Uses AI at almost every point in her day  
  * Example: asks what to do with a two-hour gap in her schedule and follows whatever the AI recommends, including attending a nearby pop-up event  
* Uses voice input while walking home from the subway, talking freely about whatever comes to mind  
  * Suggested that the access point to voice input should be as frictionless as possible  
* Responded positively to the TikTok-scroll concept (contrast with Doyoon’s concern)

# **Design Implications and Feature Concepts**

* Lightweight framing is consistent across interviews  
  * Jihee’s yellow diary analogy is a strong anchor: the app should feel casual, low-stakes, and non-burdensome  
  * Notifications should be carefully considered: if the app carries emotional weight, notifications become intrusive  
  * Jihee and Doyoon both suggest the app should not try to replicate or compete with therapy  
* Engagement through self-knowledge, not passive scrolling  
  * In-app self-assessment tests (MBTI-style or similar) as a standalone, optional section  
  * Framed as a fun activity, not a clinical tool  
  * Assessment data could feed into more personalized AI responses over time  
  * Aligns with what both Michael and Doyoon are already doing informally  
* Habit-building through lightweight tracking  
  * Mood tracker or daily check-in as a way to create a return habit  
  * Inspired by Michael’s daily logging behavior  
  * Keeps engagement healthy rather than compulsive  
  * Consistent with the lightweight, non-burdensome tone  
* Voice input accessibility  
  * Niki’s use case suggests voice should be a low-friction entry point, not buried in the interface  
* AI stance and session design  
  * Doyoon and Michael both flagged sycophancy as a real problem  
  * Both want objectivity and pattern-based insight, not agreement  
  * Session length may need to be managed, or the AI’s stance recalibrated over time, to maintain trust

# **Geo-Social AI Companion Concept**

* New concept that emerged from connecting two separate projects  
  * Last semester: built an interactive story map where users could drop location-tagged stories in New York City  
  * Core motivation: curiosity about the hidden histories and stories embedded in physical spaces  
  * User feedback on that project: people wanted to experience those stories in the actual physical space, not just on a map  
* Synthesis idea: combine the location-aware story layer with an AI companion app  
  * When a user feels stressed or low, the app suggests going outside and engaging with the physical world  
  * Could surface nearby points of interest, stories, or activities tied to the user’s location  
  * Grounds the AI interaction in real-world connection rather than screen-only engagement  
* Why this matters for the user research:  
  * Addresses the dependency concern Doyoon raised about AI-only interaction  
  * Broadens the potential user base to people who are skeptical of AI as a sole source of comfort  
  * Reframes AI as a bridge to the real world, not a substitute for it  
  * Aligns with the lightweight, go-outside, touch-grass approach to decompression that came up across interviews  
* Still early-stage; needs more structured brainstorming and written development before next session

# **Next Research and Design Steps**

* Write up the geo-social AI companion concept in the shared doc for next session  
* Continue refining the feature set based on interview findings:  
  * Lightweight daily tracking or mood check-in  
  * In-app self-assessment module  
  * Frictionless voice input entry point  
* Scope the project more tightly around a specific use case before expanding further