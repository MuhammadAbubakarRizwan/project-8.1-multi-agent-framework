# Project 8.1 Brief: Multi Agent Framework

This is the full brief. Read it in full before writing any code. See the top-level `README.md` for the quick overview.

---

## Step by Step

Work through these steps in order, once per team, before moving to the next team. Do not skip ahead to building before Step 2 is done for a framework.

1. **Learn the framework.** Skim that team's assigned framework's official documentation, then use your AI Instructor (teach-me) to explain the framework's core model back to you until you can describe it in your own words. Do this fresh, immediately before that team's stage, not all four frameworks up front.
2. **Plan.** Write a short requirements note for the team: what the team needs to do, and how the framework's model maps onto that job. See the team specification below.
3. **Design in Claude.** Design the team's dashboard screens and states in Claude before writing implementation code. Every screen and state should be settled before Step 4 starts. For the first team you build, this also sets the shared dashboard shell the other three teams will plug into.
4. **Build with Claude Code.** Implement the team's agent system and dashboard view against the finalized design.
5. **Test.** Write the tests after implementation, against that team's provided seed data, covering the behavior described below.
6. **Reflect.** A few lines on what this framework was good at, and what it struggled with, for this team's job.
7. **Repeat Steps 1 to 6 for the next team**, in order: Sales, Development and Delivery, Marketing, Executive.
8. **Deploy.** Push the finished dashboard to a live URL.
9. **Review and present.** Schedule your call, and rehearse your walkthrough with Gemini Live or OpenAI voice mode beforehand.

Light lifecycle artifacts grow one section per team as you go: `docs/requirements.md`, `docs/design.md`, `docs/test-plan.md`, `docs/reflection.md`, and `docs/system-diagram.drawio` for the evolving architecture.

---

## Team Specifications

Build one dashboard, over the provided seed data in `seed-data/`, covering four teams in a fictional software house. Each team's agent system must make its internal state visible in the dashboard, not just its final output.

### Team 1: Sales — LangGraph

Seed data: a fixed set of sample leads (`seed-data/leads.json`), each with name, company, source, and notes.

Build a LangGraph graph with explicit conditional routing between these nodes:
- **Intake** — validate and log an incoming lead into state.
- **Qualify** — score the lead against fixed criteria; a conditional edge routes to Nurture (not ready) or Propose (qualified).
- **Nurture** — generate a follow-up sequence for the lead; on a simulated response, routes back to Qualify.
- **Propose** — generate a proposal artifact for a qualified lead.
- **Close** — mark the lead won or lost, and log the outcome.

The lead's state object should carry its score, its current stage, and its full stage history as it moves through the graph.

**Dashboard view:** a lead-status board showing every lead's current stage and the path it took through the graph. This is what makes LangGraph's explicit state and routing model visible.

### Team 2: Development and Delivery — CrewAI

Seed data: a fixed backlog of feature or ticket requests (`seed-data/tickets.json`).

Build a CrewAI crew, using a manager or hierarchical process, with these roles:
- **PM agent** — breaks the ticket into tasks, assigns them, and sets acceptance criteria.
- **Developer agent** — produces an implementation (real code or a clearly structured pseudo-implementation) for the task.
- **QA agent** — reviews the developer's output against the PM's acceptance criteria and returns a pass or fail verdict with reasons.
- **PM agent (second pass)** — reviews the QA verdict, either signs the ticket off as done or sends it back to the developer. This loop continues until the ticket passes.

**Dashboard view:** a ticket-status board showing each ticket moving through PM, Developer, QA, and PM sign-off, with the artifact each role produced visible at each step.

### Team 3: Marketing — AutoGen

Seed data: a fixed set of content briefs (`seed-data/content-briefs.json`), for example blog post or campaign copy requests.

Build an AutoGen conversation loop with these roles:
- **Writer agent** — drafts the content for a brief.
- **Brand and compliance critic agent** — reviews the draft for tone, compliance, and accuracy, and returns specific feedback.
- The writer revises based on the critique. The loop continues until the critic approves, or a fixed maximum number of rounds is reached.

**Dashboard view:** the final approved content, plus the full back-and-forth conversation and critique trail. The trail itself is the demoable artifact here, since it is what is unique to AutoGen's conversational coordination model compared to the other three frameworks.

### Team 4: Executive — Paperclip

Input: a summarized snapshot of the other three teams' current state (the sales pipeline snapshot, the development ticket status, and the marketing campaign readiness).

Build a Paperclip-based synthesis agent that aggregates these three inputs into a single strategic recommendation, for example a quarterly roadmap call, a resourcing flag, or a risk note.

**Dashboard view:** a single executive briefing view showing the recommendation and the inputs it was based on.

**Note on Paperclip:** Paperclip works at a different level from the other three. It is a control plane for running agents like an organization, with org charts, goal decomposition, task routing, token budget caps, and its own management dashboard. That makes it a natural fit for the Executive team, but it also means your implementation will look structurally different from the other three rather than being another variation on the same idea. Lean into that difference and make it visible, since it is the most interesting comparison point in the project. See `docs/multi-agent-frameworks-reference.md` for background. If its actual capabilities mean the structure above needs to change, implement the closest reasonable equivalent and document what you changed and why in `docs/design.md`.

---

## Requirements and Scope

**In scope:**
- One dashboard, the four team simulations above, built in sequence against the provided seed data.
- Full internal-state visibility for every team: whichever of state, routing, role handoffs, or conversation trail applies to that framework.
- Test-first-after-implementation automated tests for each team, run against the provided seed data.
- A capability-mapping write-up in `docs/srs.md` comparing what each framework was actually good at, backed by your own build.
- Light lifecycle docs per team, and a live deployed URL once all four teams are built.

**Out of scope:**
- Deciding or building a "best" framework or a meta-agent that picks between them.
- A fifth team or framework, unless you finish early. OpenAI's Agents SDK is an optional stretch add-on only, not required.
- A heavily designed, production-polish frontend. Functional and observable is enough.
- Production-grade infrastructure beyond a simple live deployment.

`docs/srs.md` is a capability-mapping document, not a performance benchmark: for each of the four business functions, which framework was assigned, how its orchestration model maps onto that job, and where it was strong or weak in your own build. This is the document your comparison write-up and any client-facing content should be built from.

---

## Repository Structure

```
project-8.1-multi-agent-framework/
  README.md              Quick overview. Read it, do not edit it.
  docs/brief.md           This file: the full step by step, team specs, and definition of done.
  PRESENTATION.md        Your submission. All links, artifacts, and proof go here.
  LEARNING_LOG.md        One short wrap up per area you learn.
  seed-data/             Fixed leads, tickets, content briefs, and exec summary inputs.
  skills/
    teach-me/SKILL.md       Your learning agent (provided).
    troubleshoot/SKILL.md   Your troubleshooting agent (provided).
  proof/                  Screenshots and other evidence go here.
  docs/
    srs.md                Capability-mapping document: which framework fits which job, and why.
    requirements.md       Requirements notes, one short section per team.
    design.md             Design decisions, one short section per team, including any Paperclip deviation notes.
    system-diagram.drawio Simple evolving architecture diagram.
    test-plan.md          What each team's tests cover.
    reflection.md         A few lines of reflection per team.
  src/                    Your application code.
  tests/                  Your automated tests.
  .github/workflows/      Your CI workflow.
  Dockerfile
  .gitignore
  LICENSE
```

---

## Definition of Done

- [ ] Clean clone runs from the README alone, with a live URL
- [ ] Team 1 (Sales, LangGraph): full graph with conditional routing, state visible in the dashboard
- [ ] Team 2 (Development and Delivery, CrewAI): PM, Developer, QA roles with the sign-off loop, artifacts visible per step
- [ ] Team 3 (Marketing, AutoGen): writer and critic conversation loop, full trail visible
- [ ] Team 4 (Executive, Paperclip): synthesis agent producing a recommendation from the other three teams' state
- [ ] Every team's internal coordination (state, routing, role handoffs, or conversation) is visible in the dashboard, not just the final output
- [ ] Tests for each team pass against the provided seed data
- [ ] `docs/srs.md` capability-mapping write-up completed, covering all four frameworks
- [ ] Light lifecycle docs completed: requirements, design, diagram, test plan, reflection, one section per team
- [ ] `LEARNING_LOG.md` has an entry for every area in Covered Areas
- [ ] `PRESENTATION.md` completed with every link, artifact, and proof
- [ ] Loom video recorded and linked in `PRESENTATION.md`
- [ ] Mentor added as a collaborator, live presentation scheduled and completed

---

## Review and Presentation

When your Definition of Done checklist is complete, submit in three formats:

1. **Written.** `PRESENTATION.md`: the live URL, the repository link, the Loom link, and a short note on each area you learned.
2. **Video.** A short Loom walkthrough: show all four teams working, and explain what is different about how each one coordinates its agents.
3. **Live.** A call with your mentor. Walk through the dashboard live, and answer questions without notes.

Rehearse your walkthrough with Gemini Live or OpenAI voice mode before the live call.

---

## Bonus Practice Activities

Optional unless your mentor points you toward one.

- **Write a comparison article.** After all four teams are built, write a short post on which framework you would reach for, for which kind of business problem, using your own build as the evidence. Strong portfolio and LinkedIn material.
- **Build a fifth team with OpenAI's Agents SDK.** Only if you finish the required four early.

---

## Interview Gap-Check Questions

You should be able to answer all of these comfortably before considering the project finished.

1. Walk through what happens, end to end, when a lead moves from Intake to Close in your LangGraph build.
2. Why does LangGraph's explicit graph and state model fit a pipeline like Sales, and where would that same model become awkward?
3. Walk through the sign-off loop in your CrewAI build. What happens when QA fails a ticket?
4. Why does CrewAI's role-based model fit a delivery team, and what did it make harder than LangGraph would have?
5. What makes AutoGen's conversational loop different from CrewAI's role delegation? Point to something specific you saw in your own build.
6. What did you learn about Paperclip that isn't obvious from a quick look at its documentation?
7. If a client asked you to automate a real sales pipeline, would you choose LangGraph, or something else? Defend the choice using your own build, not the framework's marketing.
8. Pick one test. Why does it exist, and what does it actually verify about the agent's behavior?
9. Which of the four frameworks would you be least comfortable maintaining in production, and why?
10. What would you do differently if you started this lab again?

If you cannot answer these comfortably, you are assigned another practice variant in the same multi-agent cluster until the gap closes.
