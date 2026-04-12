---
title: "ServePoint: a ColdFusion demo, and what I learned building it AI-first"
date: 2026-04-12
# layout: post
# tags: [coldfusion, ai, coldbox, postgres]
---

# ServePoint: a ColdFusion / ColdBox demo, and what I learned building it AI-first

**Live demo:** If you want to click around before reading further, ServePoint runs on Render at **[servepoint.onrender.com](https://servepoint.onrender.com/)** (cold starts on the free tier are real—first load may take a moment). 

Repo lives [here](https://github.com/mattburnett-repo/servepoint).

I spent real time on **ServePoint**, a work-in-progress **social services case management** demo, and the interesting part—at least for this blog—isn’t only the stack. It’s that I put the codebase together **almost entirely with AI coding assistants** (e.g. Cursor-style agent workflows), while I stayed responsible for **direction, integration, security posture, and review**.

## Why this stack (and not one of the Shiny New Things)

Part of the point, for me, was to **work seriously with a stack that sits outside the industry hype machine**. The loudest channels are full of the same short list of frameworks; that’s fine for many products, but it’s not the whole profession. **ColdFusion** and **ColdBox** are unfashionable in conference keynotes and recruiter keyword lists—which is exactly why they’re interesting here: **real systems still run on them**, the constraints are concrete, and you can’t pretend you’re building Hello World. ServePoint is my way of **staying grounded in an enterprise CF ecosystem** while everyone else rabbits on about the same three JavaScript meta-frameworks. I’m not claiming CF is “better”; I’m claiming **it’s a deliberate lane** away from default hype.

## What ServePoint is

**ServePoint** is an enterprise-oriented **ColdFusion** demo: **ColdBox (HMVC)**, **Adobe ColdFusion 2025**, **PostgreSQL**, **CF ORM (Hibernate)** with **cborm**, **cfmigrations** at startup, **TestBox** for tests, **Docker** for local dev, and deployment notes that include real-world constraints (for example **Render**-style hosting). The README frames it toward **clients who care about security, privacy, and disciplined engineering**—think public sector and mission-driven agencies—not a consumer splash page.

Functionally, it’s a **case-management-shaped** exercise: cases, communications, reporting/audit-style views, document flows, admin areas behind **RBAC**, and **compliance-oriented docs** that are explicitly **engineering transparency** (checklists, data inventory), not a claim of certification. It’s still a **demo**: the value is in **how it’s built**, not in pretending it’s a finished product.

## The part I want to emphasize: AI-first development

I did not hand-type every line to see how fast I could type. I used AI assistants to **generate and refactor** large amounts of **handlers, services, migrations, tests, views, and ops glue**, and I used the project as a **lab** for how far you can push that workflow on a **specific, opinionated stack** (ColdBox conventions, Adobe CF vs Lucee footguns, ORM + Postgres, etc.).

The headline I’m comfortable defending is this: **the bulk of the code was produced by assistants under constraints I set**—repo rules, error messages, “smallest fix,” and “don’t paper over wrong schema with clever wrappers.” The headline I avoid is “the computer did everything”: **I still owned architecture, scope, merges, and correctness.**

## What I actually learned about using AI coding assistants

**1. The repo is part of the prompt.**  
Long-lived projects need **explicit rules**: stack (Adobe CF, not Lucee), framework boundaries, naming, testing patterns. The assistant stops guessing when **the repository** answers first.

**2. Debug upstream before you refactor.**  
A lot of pain that looks like “bad code” is **config, schema, env, or migration drift**. Teaching the workflow to **verify assumptions** saved more time than teaching it to **rewrite modules**.

**3. AI is fast at volume; humans are still the filter.**  
Boilerplate, CRUD-shaped code, test scaffolding, and repetitive refactors are where assistants shine. **Integration points, security rules, and “what are we actually shipping”** are where you still pay attention.

**4. Review the diff like a very fast junior teammate.**  
The failure mode isn’t “wrong syntax”—it’s **plausible-looking** code that **doesn’t match your conventions** or **hides scope creep**. The skill that leveled up for me wasn’t prompting tricks alone; it was **disciplined review**.

**5. The meta-skill is project leadership, not magic words.**  
Putting ServePoint together taught me less about one perfect prompt and more about **running a build**: narrow tasks, reproduce failures, keep changes small, stop when fixed.

## Closing

If you’re a ColdFusion or ColdBox developer, maybe the stack details are useful. If you’re anyone trying **AI-first development** on a **real application** rather than a tutorial todo list, maybe the process details are useful. Either way, this blog stays a **notes-to-self** place—ServePoint is just the latest notebook, **off the hype treadmill on purpose**.

---

*If you want the technical specifics—routing, handlers, migrations, Docker, Render—those live in the project’s README and design docs; this post is about the build and what I learned using AI assistants to do it.*
