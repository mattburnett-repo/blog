---
layout: post
title: "Aptitude Search: resume to verified openings beyond keyword search"
date: 2026-07-12
---

# Aptitude Search: resume to verified openings beyond keyword search

Live app: [aptitude-search.vercel.app](https://aptitude-search.vercel.app/)  

I’ve been building **Aptitude Search** — a pipeline that takes a resume, infers what someone is actually good at, and returns currently open job postings that fit that profile. The goal is opportunity discovery driven by aptitude, not only by keyword overlap with a recent job title.

It’s still an MVP. The claim I’m comfortable with isn’t “AI finds you a job.” It’s that **keyword-only search is a limited default**, and a small, schema-strict pipeline can do better than relying only on your last title in a search box.

This post is a short walkthrough of what it is and how it works.

## The problem I kept hitting

Keyword job search works well when your last title maps cleanly onto the market. It gets harder when:

- Your strengths don’t match the job title on your resume
- You’re changing domains and don’t know the right search terms yet
- Useful openings sit under adjacent role families you might not think to type into a search box

A lot of tools optimize for matching strings in the posting to strings on the resume. That’s useful, but it isn’t the same as finding work that fits how someone actually works.

I wanted something closer to: **resume → capabilities → role families → live openings**, with constraints (location, remote, salary floor, industries) applied along the way.

## What it does

One pipeline run does three stages end-to-end. No manual handoff between steps.

1. **Stage 1 — Aptitude profile**  
   Resume text → structured JSON: skills, strengths, working-style signals, adjacent roles, confidence. Schema-validated. Prefer omission over invention.

2. **Stage 2 — Role family plan**  
   Profile → role families with search terms, work modes, and avoid terms. This is also where [O\*NET](https://www.onetcenter.org/database.html#individual-files) occupation matching comes in — standardized occupation signals as a bridge between “what you’re good at” and “what kinds of jobs exist,” not as a live job board.

3. **Stage 3 — Job search**  
   This is the actual search. Python builds queries from the role family plan, runs web discovery (search + scrape), ranks postings by aptitude/work-pattern fit, then a synthesis LLM call maps survivors into schema-strict `verified_matches`. Result URLs are filtered to links the discovery step actually observed.

So Stage 3 isn’t “ask the model to invent jobs.” Discovery is deterministic tooling; the model synthesizes and formats what was found.

Optional constraints ride along: location, remote preference, salary minimum, industries to include/exclude.

## Stack (short version)

- **Backend:** FastAPI / Python — stages 1–3, jsonschema validation, streaming UI path
- **Frontend:** React + Vite + TypeScript — paste resume, run pipeline, expand stage panels, optional PDF export
- **Models:** Hugging Face–backed chat for stages 1, 2, and Stage 3 synthesis (keys stay on the server)
- **Occupation data:** O\*NET (local DB / embeddings path for aptitude → occupation matching; CC BY 4.0 from USDOL/ETA)
- **Discovery:** profile-driven web search and scrape, not an LLM query planner

Local loop is the usual two-process setup: API on `:3001` (Swagger at `/docs`), UI on `:5173`. Production frontend is on Vercel; the browser talks to a running API for a full pipeline.

## Design choices that mattered

**Schema-strict stages.** Stages 1, 2, and Stage 3 synthesis return JSON only, validated against committed schemas. That keeps the pipeline composable and reduces soft failures that look like success.

**Keep discovery separate from synthesis.** Discovery is Python. Synthesis ranks and shapes observed postings. If a URL wasn’t in tool output, it doesn’t survive filtering. That constraint alone avoids a lot of invented openings.

**O\*NET for job *types*, web search for *openings*.** Mixing those two jobs in one model call is how you get hallucinated “open roles.” Keeping the taxonomy and the labor market as separate concerns has been more reliable.

**Evidence over assumption.** Prompts are biased toward leaving fields empty rather than padding. Confidence signals exist for a reason.

**One clear flow for the user.** Paste resume → run → see profile, role families / O\*NET matches, then verified openings. Stage panels expand; Raw JSON is available when you need it, not as the primary UI.

## What I learned building it

1. **The product is the pipeline contract, not the stage labels.** Once `verified_matches` is the search result and Stage 3 is explicitly job search, the rest of the system has somewhere to aim.

2. **Role families beat job titles as search seeds.** Titles can be brittle. Families plus `search_terms` / `avoid_terms` produce queries that surface less obvious paths — which is the main differentiator.

3. **Fit ranking before synthesis saves time and cost.** Scraped noise is cheap to score in Python. You don’t need a chat model as your first filter.

4. **Repo rules and schemas are part of the prompt.** Same lesson as [ServePoint](https://github.com/mattburnett-repo/servepoint): the assistant (or the pipeline) behaves better when the repository answers first — schemas, prompt files, config.

5. **Ops still matters after the demo works.** Streaming failures, security headers, optional Sentry — the quieter layer that keeps a deployed MVP honest.

## Closing

Aptitude Search is my attempt to treat job discovery like a systems problem: extract signal from a resume, map it to role families with real occupational structure, then search the open web for openings that survive fit checks and schema validation.

If you want the technical specifics — prompts, schemas, Swagger examples — they’re in the repo. This post is the why and the shape of the thing.

#hopeThisHelps
