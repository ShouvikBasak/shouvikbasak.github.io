---
layout: post
title:  "Human‑in‑the‑loop isn't a compromise - it’s an architectural control"
description: "As AI agents become better at writing large amounts of code, the Architect’s role shifts to defining constraints, intent and control. Human‑in‑the‑loop is not about slowing agents down, it’s the architectural control that preserves intent, security, and understanding. It prevents scope creep, code sprawl, and loss of architectural clarity."
categories: [AI]
tags: [Agentic AI, Google Antigravity]
date: 2026-05-04 06:28:14 +0530
---
![Sample App created using Google Antigravity]({{ "/assets/app-screenshot-elected-india.png" }})

**TL;DR:** As AI agents become better at writing large amounts of code, the Architect’s role shifts to defining constraints, intent and control. Human‑in‑the‑loop is not about slowing agents down, it’s the architectural control that preserves intent, security, and understanding. It prevents scope creep, code sprawl, and loss of architectural clarity.

---

While building _ElectEd India_, an educational app for first‑time voters, as part of the _PromptWars Google Antigravity Hackathon_, I walked away with one strong realisation:

> As agents become more capable, software development begins to feel conversational. But that is precisely the moment an Architect’s role becomes critical.

When an agent can write code fluently, quickly, and across multiple files, the primary risk is no longer whether something works. The real risk is whether the system still reflects the architect’s intent.

Interacting with agents through prompts is best understood as working with a junior development team. We don’t outsource the idea, wait for a big reveal, and accept whatever comes back. We guide execution through **vision, specifications, constraints, and continuous review**.

Progress is achieved in **small, controlled steps**, through deliberate iteration and not a single splash outcome.

Lose that control, and two things happen quietly: **scope creep sets in, and code sprawl follows**. Humans can see the output, but no longer truly understand what’s inside.

My approach was to treat agents exactly like team members with strong coding skills:

- they were guided through **clear guidelines and architectural artifacts**
- they were explicitly told **what to do and what not to do**
- every meaningful outcome was **reviewed, approved, adjusted, or rejected**
- security was embedded **by design**, never “left to the agent”

The prompts weren’t creative writing exercises. They were **contracts**, human‑agent contracts, explicitly reviewed and signed off before execution.

This is where _human‑in‑the‑loop_ stops being just about “review” and starts becoming the **control plane**.

## The Architect’s role in an agentic world

Architects don’t write most of the code. Architects design the **conditions under which code is allowed to exist**.

That means:

- defining **scope, goals, non‑goals, and invariants**
- turning “this should never happen” into **hard constraints**
- externalising intent through **specs and artifacts**
- deciding **security, trust boundaries, and failure modes** _before_ implementation

Agents, in this model, are not thinkers. They are very capable implementers.

They execute. They don’t decide.

The artifacts - architecture notes, rules, and test cases became the shared source of truth that kept agent behaviour stable even as implementation evolved.

## Why this matters even more when agents can code

Agentic development doesn’t reduce the need for software engineering discipline.

It **raises the bar**.

Without architectural control:

- helpful” agents create code sprawl
- refactors drift beyond intent
- security decisions get made implicitly
- systems become harder to explain, review, and trust

With architectural control:

- agents reflect human thinking instead of improvising
- intent survives beyond a single session
- systems remain reviewable and governable

Architectural design, planning, and review are the real heavy lifting. Agents doing the coding is the efficiency gain.

One observation I’ll leave for another post. Within Google Antigravity, different models excel at different kinds of work, and agents should be made to use them selectively. But that’s a write‑up for another day.

GitHub Repo link here for those curious: https://github.com/ShouvikBasak/promptwars-c2

#BuildwithAI #GoogleAntigravity