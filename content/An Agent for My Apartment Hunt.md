## Why I chose this problem

I'm currently apartment hunting in NYC, and I've realized that finding apartments isn't actually the most frustrating part. It's everything that happens after I find one I like.

I usually browse StreetEasy, Zillow, and other rental sites, find a few apartments I'm interested in, and reach out to the brokers. Depending on the platform, that could mean filling out a contact form, sending a message, or texting someone directly.

Then I wait for a response. Some brokers never get back to me, but when they do, they almost always ask the same questions:

- What's your credit score?
- What's your annual income?
- When are you planning to move in?

I've probably answered these questions dozens of times by now.

What's even more annoying is that when a broker replies, I often don't remember which apartment they're talking about. I have to go back to StreetEasy or Zillow, find the listing again, look through the photos, check the price and location, and remind myself why I was interested in it in the first place.

And since all these conversations happen across different platforms, I don't really have a good way to keep track of the apartments I'm considering or compare them side by side.

So for this assignment, I wanted to explore how an AI agent could handle this entire process for me, using the agentic design patterns from Chapter 6 of _Designing AI Interfaces_.

## My idea: organize everything around the apartment

The biggest thing I want to change is how information is organized.

Right now, everything is organized around messages. I have an email from one broker, a text from another, and a conversation somewhere inside StreetEasy. I'm the one who has to figure out which message belongs to which apartment.

Instead, I want the apartment itself to be the center of everything.

Each apartment would have its own card with photos, address, floor plan, rent, neighborhood, and a link to the original listing. All the conversations related to that apartment would live inside the same card, regardless of whether they came from email, text, or a rental platform.

If the same apartment is listed on multiple websites, the agent would recognize that and combine them rather than creating duplicates.

I also don't want to manually search for apartments every day. Ideally, the agent would keep an eye on new listings based on my preferences and recommend ones I might like.

If I'm interested, I approve the listing, and the agent takes care of reaching out to the broker. Once the broker responds, the agent can answer common questions using information I've already provided and even schedule a viewing based on my calendar.

My involvement should be limited to things that actually need my decision, rather than answering the same questions and coordinating schedules over and over.

## Breaking down the agent

Chapter 6 talks about four patterns for agentic systems: decomposition, sequencing, delegation, and clarification. I tried applying each of them to this workflow.

### Clarification: figuring out what I want first

Before the agent starts searching, it needs to know what I'm looking for.

I'd give it my budget, preferred neighborhoods, apartment size, move in date, and other preferences. I'd also provide the information brokers commonly ask for, like my income and credit score.

Another important part is deciding what information the agent is allowed to share without asking me first.

For example, I'm fine with it automatically telling a broker my preferred move in date. But I might want to approve sharing my exact income or credit score, especially with someone I haven't spoken to before.

I'd rather set these preferences once at the beginning than have the agent interrupt me every time it needs something.

### Decomposition and sequencing: turning one big task into smaller ones

Apartment hunting sounds like one task, but it's actually a bunch of smaller tasks that need to happen in a certain order.

Here's the workflow I imagine:

1. Search for new listings that match my preferences.
2. Recommend apartments I might be interested in.
3. Let me approve or reject each recommendation.
4. Reach out to the broker for approved listings.
5. Read the broker's response and answer routine questions.
6. Coordinate and schedule a viewing.
7. Bring me in when a decision or additional information is needed.

The order matters because each step depends on what happened before. For example, there's no point trying to schedule a viewing before the broker has responded.

### Delegation: different agents handling different jobs

Rather than having one agent do everything, I'd divide the work between three agents.

**Scout agent:** Finds new apartments, checks them against my preferences, and removes duplicate listings.

**Outreach agent:** Contacts brokers, keeps track of conversations across different channels, and responds to questions it already knows how to answer.

**Scheduling agent:** Checks my calendar, coordinates viewing times with brokers, and adds confirmed appointments.

As a user, I wouldn't necessarily need to know which agent is doing what. I'd just see the results in one interface.

### ReAct: dealing with unpredictable conversations

One thing that makes this workflow interesting is that broker conversations aren't always predictable.

Some brokers immediately offer viewing times. Others ask for additional documents. Sometimes an apartment is already rented, or the broker suggests a time that doesn't work for me.

This is where the ReAct pattern comes in.

Instead of following a fixed script, the agent needs to interpret each new message, decide what to do next, take an action, and then respond to whatever happens afterward.

For example, if a broker suggests a viewing on Wednesday at 3 PM, the agent checks my calendar. If I'm free, it can confirm. If I'm busy, it can suggest another time. If the broker asks for something outside the agent's permissions, it brings the conversation back to me.

## The three logics: what deserves my attention?

One of the things I liked from the chapter was the idea that an agent shouldn't just automate tasks. It should also decide what information is worth bringing to the user's attention.

For my apartment hunting agent, I would use three different logics to decide what happens to incoming information.

### 1. Elevate what needs me

These are situations where the agent can't or shouldn't make the decision for me.

For example:

- A broker asks a question that I haven't answered before.
- A viewing time requires me to rearrange my schedule.
- Someone asks about application fees, deposits, lease terms, or negotiation.
- There's an important update about an apartment I've favorited.

I'd want these to appear in a separate "Needs you" section.

The goal is that when I open the app, I don't have to read through every conversation to figure out what I need to do. I can just go to this section and see the few things that actually require my attention.

### 2. Divert the repetitive stuff

This is where I think the agent would save me the most time.

It should automatically handle things like answering the same income and credit score questions, recording confirmation messages, organizing conversations, and merging duplicate listings.

I'd also want it to move listings with no response into a separate category after a few days.

Importantly, I don't want these messages to disappear completely. I should still be able to open an apartment card and see exactly what the agent said or did.

I just don't need to be notified every time a broker says, "Thanks, I'll get back to you."

### 3. Elevate what's time sensitive

Apartment hunting can move pretty quickly in NYC. Sometimes a listing is gone within a day, and missing a message can mean missing an opportunity.

So I'd want the agent to notify me when something is time sensitive, even if I haven't opened the app.

For example, if a viewing is scheduled for tomorrow but hasn't been confirmed, I should know. Same if a broker tells me there are other applicants or that I need to respond by a certain deadline.

I'd also want to know if an apartment I've favorited suddenly drops in price or gets marked as rented.

For these situations, I'd rather get an extra notification than miss something important.

 I also think there's a difference between a regular update and something that needs immediate attention. If a broker confirms a viewing for next week, a normal notification is enough. But if a broker tells me I need to submit something within the next two hours, I'd want the agent to make sure I actually see it. Maybe it could send another notification if I haven't responded, rather than assuming I've read the first one.

## Showing the plan before taking action

Another principle from Chapter 6 that stood out to me was revealing the agent's plan before it acts.

I think this is especially important for an agent that communicates with other people on my behalf.

For example, when I approve an apartment recommendation, I don't want the agent to immediately start sending messages without showing me what it's about to do.

Instead, it could say something like:

> I'll reach out to Jane at Corcoran through StreetEasy, introduce you, mention your preferred move in date, and ask about viewings on Tuesday evening or Saturday morning. Should I send it?

That gives me a chance to check what it's planning to share before the message goes out.

But I also don't want to approve every tiny action forever, because that would defeat the whole purpose of having an agent.

The chapter discusses how Claude Code uses different permission levels depending on how risky an action is. I think a similar approach makes sense here.

|Action|Permission|
|---|---|
|Search and save listings|Automatic|
|Send the first message to a broker|Requires my approval of the listing|
|Share my preferred move in date|Automatic|
|Share income or credit score|Ask first, or share an approved range|
|Schedule a viewing during my available hours|Automatic, with a notification|
|Submit applications, pay fees, or negotiate lease terms|Always requires my approval|

I also like the idea of adjusting these permissions over time.

At first, I might want to review every outgoing message because I don't fully trust the agent yet. But after seeing that it consistently handles routine conversations correctly, I'd probably be comfortable giving it more freedom.

The important thing is that I should always know what it's allowed to do, and be able to change those permissions whenever I want.

## What the interface would look like

I don't think this needs to look like a traditional chatbot.

Actually, I'd prefer not to have to chat with the agent much at all. The whole point is to reduce the amount of work I have to do.

I imagine the interface having a few different layers, depending on how much detail I want to see.

### Notifications

This is the lightest layer.

If a viewing gets confirmed, a deadline is approaching, or the agent needs me to answer something, I get a notification.

Otherwise, I shouldn't have to think about the app.

### The apartment board

This would be the main screen and probably the part I'd use most.

I'd see all my apartments organized by status:

**Suggested → Contacted → Replied → Viewing Scheduled → Applied**

There would also be a separate category for listings that never received a response.

Each apartment would have a card showing its main photo, address, rent, and current status. If something needs my attention, there would be a small indicator on the card.

This would make it much easier to compare apartments, which is something I currently struggle with because everything is scattered across different sites.

### Individual apartment cards

If I want more information, I can open an apartment card.

This is where I'd see all the details about that apartment, including photos, floor plan, location, price, and the original listing.

More importantly, I'd see the entire conversation history with the broker in one place.

Even if the conversation started on StreetEasy and continued over text or email, it would all appear together.

I'd also want messages clearly labeled so I can tell which ones were sent by the agent and which ones I sent myself.

### Activity log

I'd want a place where I can see everything the agent has done.

Every message it sent, every piece of information it shared, and every appointment it scheduled should be recorded.

Realistically, I probably wouldn't check this very often. But knowing I can go back and see exactly what happened would make me much more comfortable letting the agent act on my behalf.

### The "Needs you" section

This would basically replace my current habit of checking multiple inboxes.

Instead of opening Gmail, StreetEasy, Zillow, and my texts to see whether anyone replied, I could open one section and immediately see what requires my input.

Ideally, this section would be pretty empty most of the time.

## One last thing: learning what I actually like

I'd also want the scout agent to get better at recommending apartments over time.

At the beginning, it would mostly rely on the criteria I explicitly give it, like price, neighborhood, and size.

But those criteria don't capture everything.

For example, I might keep rejecting apartments that technically fit my budget because they have tiny windows or awkward layouts. Or I might consistently save apartments with lots of natural light, even if they're a little smaller than I originally wanted.

The agent should be able to pick up on those patterns and adjust its recommendations.

I imagine getting a small batch of new listings each day, where I can quickly say yes or no. Over time, those decisions would help it understand my preferences better.

The chapter mentions Clippy as an example of an assistant that often interrupted users without being particularly helpful. I think the difference here is that the agent should learn when to help, what to recommend, and when to leave me alone.

## Final thoughts

What interests me most about this idea isn't necessarily having AI find apartments for me. I can already do that myself.

It's the possibility of having an agent manage all the small, repetitive tasks that happen between finding an apartment and actually going to see it.

I don't want to spend my time figuring out which broker emailed me, answering the same questions again, or manually coordinating viewing times.

I want to spend my time comparing apartments, visiting the ones I like, and deciding where I actually want to live.

I think that's what makes this a good use case for an agentic system. The agent can take care of the repetitive work, while I stay involved in the decisions that matter.