---
title: "Gas Town: Why I'm Building a UI"
date: 2026-01-03T14:00:00Z
draft: false
tags: ["gas-town", "claude-code", "ai-agents", "building-in-public"]
description: "I discovered Gas Town this morning and immediately knew I had to build a frontend for it. Here's why."
---

This morning I was scrolling through X, thinking about the same problem I've been obsessing over for days: how do you actually orchestrate multiple Claude Code agents without losing your mind?

Then I saw it. [Gas Town](https://github.com/steveyegge/gastown) by Steve Yegge.

If you don't know Steve Yegge - he's a legendary developer (Amazon, Google, Sourcegraph) whose blog posts are considered required reading by many programmers.

I read his entire [blog post](https://steve-yegge.medium.com/welcome-to-gas-town-4f25ee16dd04). And by the end, I knew I had to build a UI for this thing.

## The Problem I've Been Trying to Solve

If you're using Claude Code for real work, you've probably wondered: how do people run 10+ agents in parallel? I've seen screenshots of people with dozens of sessions going. How does that even work?

I've tried a few things:

**Conductor** was one of the first UIs I found. It creates separate worktrees so agents don't step on each other's changes. I still use it when I want manual control over my agents. It works.

**Vibe Kanban** looked promising, but it didn't feel autonomous enough for what I wanted. I couldn't figure out how to make it truly hands-off.

I briefly looked at **Amp Code** and **Droid by Factory.ai**, but since I already have a Claude Code subscription, I didn't go deep there.

None of these solved the core problem: how do you delegate work to agents, have them coordinate, and actually trust that things are happening?

## Then I Found Gas Town

Gas Town isn't just another wrapper around Claude Code. It's a complete orchestration layer with a vision I hadn't seen before:

- **A Mayor** that coordinates everything and delegates tasks
- **Polecats** (ephemeral workers) that execute tasks and shut down
- **A Refinery** that handles merging so agents don't conflict
- **Beads** - a git-backed issue tracker where work persists across sessions
- **Convoys** to batch and track work in flight

And the wildest part? Agents can send "mails" to the Mayor. There's actual structured communication between agents.

### Beads: The Secret Weapon

Beads deserves special mention because it solves one of the most frustrating problems with AI agents: **amnesia**.

Every time Claude Code's context resets, you lose everything. All that understanding of your codebase? Gone. The task it was working on? Forgotten. With Beads, work is stored as JSONL in your git repo. It survives context resets. It branches with your code. And because it uses hash-based IDs (like `bd-a1b2`), you never get merge conflicts even when multiple agents are working in parallel.

```bash
# See what's ready to work on
bd ready

# Create a new task
bd create "Fix authentication bug" -p 0

# Check dependencies
bd dep tree bd-a1b2
```

It's like giving your AI agents a persistent memory that lives in version control.

The whole thing is built on one principle that Steve calls "The Propulsion Principle":

> **If you find something on your hook, YOU RUN IT.**

No waiting for confirmation. No "should I do this?" The hook is the assignment. Execute immediately. Gas Town is a steam engine and agents are pistons.

## What I Learned in My First Day

I set it up this morning. A few things surprised me:

**The orchestration is incredibly thoughtful.** This isn't "spin up 10 Claude sessions and hope for the best." There's real structure here - roles, lifecycles, accountability.

**Guardrails still matter.** The Mayor is supposed to delegate, not implement. But sometimes Claude being Claude, it tries to solve things itself instead of delegating. You need to set up Claude Code permissions carefully. The project is still very new (~960 stars as I write this), so these edges are expected.

**tmux is... tmux.** The default interface is tmux-based. If you're comfortable in tmux, great. But I want to SEE my agents. I want a dashboard. I want to glance at a screen and know what's happening.

## Why I'm Building a UI

That's the gap I want to fill.

Gas Town's architecture is solid. The CLI and tmux interface work. But I think there's room for something more visual - something where you can:

- See all your agents side by side in real-time
- Watch tasks flow through the system
- Know at a glance what's stuck and what's landed

I'm calling it **Imperator** (working title). I've already mocked it up with two [Lovable](https://lovable.dev) prompts - starting simple with side-by-side terminal views. If it takes off, maybe task graphs and convoy dashboards later.

I'm learning as I go. This is building in public in the truest sense.

## What's Next

I'm going to document this journey here. The wins, the walls, the "what was I thinking" moments.

**Resources:**

- [Steve's blog post on Gas Town](https://steve-yegge.medium.com/welcome-to-gas-town-4f25ee16dd04)
- [Gas Town repo](https://github.com/steveyegge/gastown)
- [Beads repo](https://github.com/steveyegge/beads)

And if you're building something similar or want to follow along with Imperator - stick around.

Back to building.

---

*Next up: Setting up Gas Town from scratch and my first real multi-agent workflow.*
