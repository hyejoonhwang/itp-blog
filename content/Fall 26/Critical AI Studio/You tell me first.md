
Most AI tools are designed to answer as fast as possible. You type a question, and a confident answer appears before you’ve had a chance to think about it yourself. In class we talked about the risk this creates: cognitive offloading. When thinking is always handed to the machine, our own judgment gets weaker, and we start trusting whatever sounds sure of itself. Louise points out in the book "Designing AI Interfaces" that users already tend to assume AI output is factual, even though models hallucinate. _You tell me first_ is a small, playful pushback against that habit.

The project is a little creature that answers your questions, but only after you feed it your own thoughts. Under the question field there is a second box, “Feed me your thoughts,” where you write a guess, a doubt, something the question reminds you of, or why you’re asking. There’s no required format. Any real thought counts.

The friction sits at the input, and the method follows the input, computation, output framework. First, an evaluator model reads your thoughts and sorts them into kinds: a take, a connection, a doubt, or a reason. Filler like “idk” or “just tell me” counts as junk food. This computation is made visible instead of hidden behind a spinner: the cloud eats each thought like a snack and spits out the junk. If it was fed, a second model gives a real answer that builds on your thinking, telling you where you were right, where you were off, and what to verify. If you gave it nothing, the cloud daydreams. It writes a confident but made up answer, and then the bubble pops and it admits it invented everything because it was hungry.

That reveal is the core of the experience. I didn’t want to simply block lazy questions, because a wall teaches nothing. Showing a convincing fake answer and then exposing it lets users feel how easy it is to believe a hallucination. The tone stays funny, never shaming.

The project is built with React and Vite, animated with Motion, and deployed on Vercel. Both model calls run through OpenRouter, with a fast, cheap model as the evaluator and a stronger model for the answer. I chose a web app so anyone can try it instantly, and I animated the character because its states are the computation made visible.

One limitation I want to name: a model can’t truly judge whether a thought is original. It can only check whether it’s specific and comes from the user’s own perspective. Still, even that small pause before asking changes the relationship. The user thinks first, and then AI answers.

https://you-tell-me-first.vercel.app
