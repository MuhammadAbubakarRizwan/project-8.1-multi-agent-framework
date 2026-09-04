# Project 8.1: Multi Agent Framework

| Field | Detail |
|---|---|
| Project Number | 8.1 |
| Project Name | Multi Agent Framework |
| Tier | Senior |
| Deadline | 10 working days (2 weeks) from your start date |
| Status | Active |
| Tags | Multi-Agent Systems, LangGraph, CrewAI, AutoGen, Paperclip, Agent Orchestration, System Design, Test Driven Development, Prompt Engineering, Git Workflow |

---

## What This Is

One dashboard that simulates a small software house, run by four different multi-agent orchestration frameworks, one framework per team.

| Team | Framework | Job |
|---|---|---|
| Sales | LangGraph | Move leads through a qualify, nurture, propose, close pipeline |
| Development and Delivery | CrewAI | PM, developer, and QA working tickets with a sign-off loop |
| Marketing | AutoGen | Writer and critic agents drafting and revising content |
| Executive | Paperclip | Synthesize the other three teams into a strategic recommendation |

The point is not to decide which framework is best. The point is to build the same kind of business problem four different ways, once each, so you understand what each framework is actually good at, where it struggles, and which real problems it fits. By the end you should be able to sit in front of a client and say, with a working example behind you, "here is what I would reach for and why."

Everything runs against a fixed, provided set of seed data in `seed-data/`, so every build is deterministic and testable. Keep it light and move fast. Use AI heavily to accelerate the build, once you understand what you are building.

---

## Start Here

**Read [`docs/brief.md`](docs/brief.md) in full before writing any code.** It has the step by step you follow, the specification for each of the four teams, the definition of done, and the questions you should be able to answer at the end.

Before you start, add the two provided skills in `skills/` to your Claude project:

- **Learning agent** (`skills/teach-me/SKILL.md`). Say "teach me" and a topic, for example "teach me LangGraph state", and it walks you through it and asks for a confidence score at the end. Save that in `LEARNING_LOG.md`.
- **Troubleshooting agent** (`skills/troubleshoot/SKILL.md`). Describe what broke and it guides you to the cause instead of handing you the fix.

Then fork this repository and work inside your fork. Commit as you finish each team, save proof into `proof/`, and fill in `PRESENTATION.md` as you go.

---

## Reference Material

**The sample UI: [`examples/agent-framework-observatory.html`](examples/agent-framework-observatory.html)**

Download it and open it in your browser. This is the standard your build should match in shape and depth. It shows the input, the agents working, the decision, the human review step, and the action that follows. Match the intent, not the pixels. You do not have to copy its styling.

The design brief it was generated from is at [`examples/ui-reference/DESIGN-BRIEF.md`](examples/ui-reference/DESIGN-BRIEF.md), which spells out the five-stage loop, the per-framework identity, and the design system in full. Read it alongside the sample.

**The wider landscape: [`docs/multi-agent-frameworks-reference.md`](docs/multi-agent-frameworks-reference.md)**

A comparison of the main multi-agent frameworks in use, including the four this project assigns and six others, with a decision matrix for picking one on a real problem. Read it once for orientation. You are only required to build with the four assigned here.

---

## Prerequisites

You should already be comfortable with the following. If one is rusty, ask your AI Instructor for a fast revision and a few check questions, then move on.

- Basic programming in at least one language
- Building and calling a simple web API
- Basic Git: clone, commit, push, branch
- What an LLM API call is, and what a single-agent tool-calling loop looks like

---

## Covered Areas

| Category | Area |
|---|---|
| AI and LLM Fundamentals | Agentic Patterns |
| AI and LLM Fundamentals | Tool Use |
| AI and LLM Fundamentals | Multi-Agent Coordination |
| AI and LLM Fundamentals | Prompt Engineering |
| Programming and Stack Fundamentals | API Construction |
| Programming and Stack Fundamentals | Data Persistence |
| SDLC and Engineering Practices | Test Driven Development |
| SDLC and Engineering Practices | Development Lifecycle |
| SDLC and Engineering Practices | Version Control |
| Communication and Client Readiness | Technical Explanation |

Communication and Client Readiness is trained through the review and presentation work in the brief, alongside the build, not as separate work.

---

## Recommended Stack

This project can be built in any stack or language you choose.

- **Models:** your choice of LLM provider. A free-tier model, for example Gemini Flash, is enough for all four teams.
- **Frameworks:** LangGraph, CrewAI, AutoGen, and Paperclip, one per team as assigned in the table above. These are fixed.
- **Frontend and backend:** your choice. Keep the frontend functional and clear rather than heavily designed. The point is to make each framework's internals visible, not to build a polished product.
