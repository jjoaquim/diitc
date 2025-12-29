---
title: "Critical Thinking in the Age of AI"
date: 2025-12-22T15:43:03+01:00
images: []
tags: ["AI", "Development", "Critical Thinking", "Product"]
categories: ["AI", "Development", "Critical Thinking", "Product"]
draft: false
---

We're entering a weird phase of software.

AI can generate code.
AI can generate UI ideas.
AI can generate *confidence*.

That last one is the dangerous part.

The failure mode isn't "AI is useless." The failure mode is "AI is *plausible*" — so teams stop doing the boring checks that keep production from catching fire.

Engineering teams across the industry are shipping AI-generated database migrations without understanding the schema changes. Marketing teams deploy AI-written copy that confidently claims features the product doesn't have. Support teams paste chatbot responses that solve the wrong problem entirely.

The code compiles. The tests pass. The metrics look green.

Until they don't.

Addy Osmani published something that cuts through the noise: **"[Critical Thinking during the age of AI](https://addyo.substack.com/p/critical-thinking-during-the-age)."** It's a simple framework — almost embarrassingly obvious. Which is exactly why it works.

## The Framework That Actually Matters

Addy structures critical thinking around six questions: **Who / What / Where / When / Why / How**

Not as philosophy. As an operating system for teams building with AI.

Here's what that looks like when you're actually shipping product.

---

## Who: Authority Is Not Optional

When an LLM generates code that looks authoritative, your brain wants to merge and move on.

That's the trap.

I've seen production incidents caused by AI-suggested changes that nobody actually understood — not because the team was careless, but because the process didn't catch it. The code was syntactically correct. It even passed tests. But when it hit production at 2 AM, nobody knew how to debug it because nobody had *owned* the decision to ship it.

Treat AI output like it came from an eager intern: useful draft, zero authority.

Before you ship, answer:
- Who is accountable when this breaks?
- Who understands the domain enough to validate this?
- Who reviewed the security implications?
- Who will debug this at 3 AM?

Critical thinking is a team sport. "Who's in the room?" determines whether you ship a feature or a time bomb.

---

## What: Define the Problem Before You Code the Solution

Most AI workflows skip straight to implementation.

You type: "How do I implement X?"

And now you're building a solution to a problem you never validated. I've watched teams spend weeks optimizing AI-generated database queries when the real problem was that nobody had explained to the AI that the data model was wrong.

The better sequence:
- What is actually failing? (Not what users say. What the data shows.)
- What does success look like? (Specific, measurable.)
- What evidence supports this being the right problem?

"Users say it's slow" is not a problem statement. It's a symptom.

Is it slow for all users or a segment? Slow on mobile or desktop? Slow because of render time, network latency, or database queries?

Define the problem with specificity or you'll optimize the wrong thing.

---

## Where: Context Is King (Sandboxes Lie)

A fix that works in a local development environment can be a production catastrophe.

Real example: AI suggests using a synchronous API call because that's what worked in the documentation example. Ships to production. Blocks the main thread. App freezes. User churn spikes 40%.

Before you ship, map the context:
- Where does this run? (Mobile device with 2GB RAM? Edge worker with 50ms timeout? Server with unlimited resources?)
- Where in the user journey does this fail? (New users? Power users? Specific workflows?)
- Where are the downstream dependencies? (What breaks if this service goes down?)

AI is great at local solutions. Real engineering is understanding the system it lands in.

---

## When: Triage vs Root Cause

Under pressure, teams default to speed: restart the service, roll back the deploy, "just ship it."

Sometimes that's correct. But label it honestly: **This is a band-aid. We're buying time.**

The critical skill is knowing when speed matters and when rigor is non-negotiable.

Triage is for production fires. Root cause analysis is for everything else.

And if you do a quick fix, schedule the follow-up immediately. Otherwise, the band-aid becomes "the architecture", and you're debugging it three years later, wondering why nobody can remember why it was built that way.

---

## Why: Stop Shipping Vibes

"Why are we doing this?" is the most underrated product question in 2025.

If the answer is:
- "Because it's trendy."
- "Because our competitor did it."
- "Because the AI suggested it."

That's not a why. That's a vibe.

Real example: A team ships an AI chatbot because "everyone's doing AI now." Usage is 2%. Support tickets increase 30% because the bot can't escalate to humans. Six months later, they shut it down.

Nobody asked: Why do users need this? What problem does it solve that humans or docs can't?

Use the **5 Whys** framework when debugging failures:
- Why did the model fail? (Data quality dropped)
- Why did data quality drop? (Pipeline dependency broke)
- Why wasn't it caught? (No monitoring on that metric)
- Why is monitoring missing? (Team assumed it was covered)
- Why did we assume? (No ownership clarity)

Stop fixing symptoms. Fix causes.

---

## How: Show Your Work

The final trap: skipping justification.

You paste the AI-generated solution. It compiles. Tests pass. Ship it.

Six months later, someone asks: "Why did we implement it this way?"

Nobody remembers.

Critical thinking shows up in *how* you communicate:
- What's the proposal?
- What's the rationale?
- What evidence supports it?
- What are the trade-offs?
- What alternatives did we consider?

If you can't explain it clearly in a pull request description, you don't understand it yet. And if you don't understand it, you can't maintain it.

AI doesn't change that. It amplifies it.

---

## The Pre-Ship Checklist That Keeps Production Stable

Before you merge AI-assisted changes:

- **Who** is accountable? Who reviewed it? Who owns debugging it?
- **What** problem is this solving? Show the evidence.
- **Where** could it break? Map the environments and dependencies.
- **When** is this a triage fix vs root-cause work?
- **Why** is this the right approach? What alternatives exist?
- **How** will we validate it? What's the rollback plan?

This isn't bureaucracy. It's how you maintain velocity *and* quality when your draft engine is infinite.

---

## The Real Competitive Advantage

AI makes it easy to *look* productive.

Critical thinking is what keeps you *effective*.

The teams that win in the next few years won't be the ones that "use AI the most." They'll be the ones who keep their judgment intact while using it.

Because when everyone can generate infinite code, the differentiator is knowing what *not* to ship.

---

**Further Reading**
- Addy Osmani — "[Critical Thinking during the age of AI](https://addyo.substack.com/p/critical-thinking-during-the-age)"
