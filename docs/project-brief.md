# Project Brief

Internal tracking metadata for mentors. Not read by the resource building the project.

- **Tier:** Senior
- **Categories and subcategories targeted:** AI and LLM Fundamentals (Agentic Patterns, Tool Use, Multi-Agent Coordination, Prompt Engineering); Programming and Stack Fundamentals (API Construction, Data Persistence); SDLC and Engineering Practices (Test Driven Development, Development Lifecycle, Version Control); Communication and Client Readiness (Technical Explanation)
- **Role, seniority, and technical tags:** multi-agent systems, LangGraph, CrewAI, AutoGen, Paperclip, agent orchestration, system design, TDD
- **Start date:** [date]
- **End date:** [date]

## What This Project Teaches

Hands-on depth across four different multi-agent orchestration models. The resource builds four small agent teams (Sales on LangGraph, Development and Delivery on CrewAI, Marketing on AutoGen, Executive on Paperclip) inside one fictional software house, makes each team's internal coordination observable in a shared dashboard, and writes up a capability-mapping comparison grounded in the actual builds. It is a practice and reference build, not a placement gate.

## Relationship to the library

Decimal variant under the still-unbuilt whole-number Project 8 (The Swarm, Senior, multi-agent coordination), same pattern as Project 4.1 under Project 4. Generalized and forkable by any resource, not built specifically for whoever is assigned first.

## Assessment Criteria

On successful completion, the resource can build and explain each of the four frameworks' coordination models, justify which framework fits which kind of business problem using evidence from their own build rather than reputation or marketing, make an agent system's internal state visible in a UI, and defend a framework choice to a non-technical stakeholder in plain language.

## Definition of Done

- [ ] Clean clone runs from the README alone, with a live URL
- [ ] All four teams complete: Sales (LangGraph), Development and Delivery (CrewAI), Marketing (AutoGen), Executive (Paperclip)
- [ ] Every team's internal coordination (state and routing, role handoffs, or conversation trail) is visible in the dashboard
- [ ] Tests for each team pass against the provided seed data
- [ ] CI is green
- [ ] `docs/srs.md` capability-mapping write-up completed
- [ ] Light lifecycle docs completed, one section per team
- [ ] Presentation checkpoint completed
- [ ] Assessment criteria checked against actual performance

## Showcase

Live URL plus a short Loom showing all four teams working, explaining what is structurally different about how each one coordinates its agents. The capability-mapping write-up in `docs/srs.md` is the source material for LinkedIn content and portfolio positioning aimed at a C-suite / non-technical audience, not just other engineers.

## Depth of Learning

Depth, not surface. The resource implements each framework's coordination model themselves, on a genuinely different job per team (pipeline routing, role delegation with a review loop, conversational critique, executive synthesis), so the frameworks' differences show up structurally rather than being asserted. Paperclip specifically requires independent research since it is less documented than the other three; how the resource handles that gap is itself worth evaluating.

## Intended Use Case

A generalized, reusable lab for comparing multi-agent orchestration frameworks against a realistic small-business scenario (a software house with Sales, Development, Marketing, and Executive functions), so results are grounded in a working build rather than a reading of each framework's marketing.
