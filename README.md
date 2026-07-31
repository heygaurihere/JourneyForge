# JourneyForge
![Status](https://img.shields.io/badge/status-in%20development-yellow)
![License](https://img.shields.io/badge/license-MIT-green)
![Python](https://img.shields.io/badge/python-3.12-blue)

# JourneyForge

A goal-driven browser agent that explores websites like a real user, tracking its click path and decision points to detect UX friction — producing a scored report with a recorded session for review.

## The Problem

You ask a friend to try your website: "Buy a phone" or "Create an account."
They come back and say "I couldn't find the search bar" or "signing up took forever."

That kind of honest, first-time-user feedback is gold — but you can't ask a real
person to test every page, every flow, every time you ship a change.

JourneyForge is that friend, automated.

## What It Does

Give it a URL and a goal in plain English:

URL: https://example.com
Goal: Find Pricing


The agent opens the site and tries to complete the goal like a real, slightly
confused first-time user would — deciding which links or buttons to click at
each step, going back when it hits a dead end, and trying again.

When it's done (or gives up), you get a report:

Goal: Find Pricing
Status: Completed
Time Taken: 2 minutes
Clicks: 11
Wrong Turns: 3
Dead Ends: 2

Insight: Users are likely confused because Pricing isn't in the
main navigation bar.


Plus a recorded video of the entire attempt, so you can watch exactly
where it struggled.

## Example Scenarios

*(Illustrative examples of intended behavior — these describe the target 
experience this project is being built toward.)*

**Goal: Create Account**
Agent tries Login → Sign In → Forgot Password → Back → Signup → Email →
OTP → Captcha → Done. Took 5 minutes.
> ⚠ Registration flow is too complicated.

**Goal: Delete Account**
Agent searches Settings → Privacy → Help → finds nothing. Eventually gives up
and tries Google search instead.
> ⚠ This action is effectively impossible to find.

## How It Works (Architecture)

1. **API** (FastAPI) — accepts a job request (URL + goal), returns a job ID
2. **Queue** (Redis) — holds pending jobs for workers to pick up
3. **Worker** (Playwright + LLM) — opens an isolated browser session, looks at
   available links/buttons on each page, decides the most promising next click
   toward the goal, and repeats — tracking every step, wrong turn, and dead end
4. **Report** — compiles the click path, friction score, and a recorded video
   of the session

User → API → Redis Queue → Worker (Playwright + LLM) → Report + Recording


## Roadmap

- [x] Phase 0 — Project scaffolding, architecture design
- [ ] Phase 1 — Core pipeline: API + Redis queue + worker skeleton (dummy data)
- [ ] Phase 1 — Real browser automation with Playwright
- [ ] Phase 1 — Goal-directed click decisions via LLM
- [ ] Phase 1 — Friction scoring + report generation + video recording
- [ ] Phase 2 — Accessibility analysis layered onto the same browser session
- [ ] Future — SEO checks, performance analysis, conversion funnel insights

## Status

🚧 Early development — currently building the core job pipeline (API + Redis
+ worker skeleton) before adding the real browsing agent.

## Tech Stack

- **Backend:** FastAPI
- **Queue:** Redis
- **Browser Automation:** Playwright
- **Agent Decisions:** LLM (via Groq API)
- **Frontend:** React (planned)
- **Database:** PostgreSQL (planned, for storing report history)

## License

MIT
