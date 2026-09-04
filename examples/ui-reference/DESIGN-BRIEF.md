# UI Reference: Claude Design Brief

Paste this into Claude Design to generate the working demo for Project 8.1. The output is a reference showing a resource exactly what "done" looks like, and it must be presentable to a client as it stands.

---

## 1. The one rule

**The subject of this interface is the agent frameworks, not the business.**

This is an agent observability and control tool that happens to run on a software house scenario. It is not a business dashboard that happens to use agents. Think LangSmith, Langfuse, or Arize Phoenix, not Jira, HubSpot, or Trello.

Every screen answers "which framework is this, how did it coordinate its agents, what did it decide, and what did it cost" first. What happened to the lead or the ticket is the consequence, shown at the end.

### Anti-goals, do not build these

- Do not make a sales funnel, a Kanban board, or a ticket board the main navigation.
- Do not open the app on a business overview with pipeline totals or revenue figures.
- Do not put business metrics in the hero position. The headline numbers are about agent execution: steps, tokens, cost, latency, retries.
- The user must never click through a business object to reach the agent detail. The agent detail is the default view.

The business layer exists and is real, but it is downstream. It answers "so what did this actually do," which is the last question, not the first.

---

## 2. Critical constraint

**All agent activity is pre-recorded and hardcoded. Do not call any LLM API. Do not integrate LangGraph, CrewAI, AutoGen, or Paperclip.** Every run is scripted data played back on a timer.

Write the scripted runs so they read like genuine agent output: specific, a little imperfect, with real reasoning. The scripted failures in Section 8 are required, not optional. A happy-path demo is useless as a reference.

---

## 3. The core loop

This is the spine of the entire design. Every framework tab runs the same five stages, in the same layout, in the same order. **Only the visual in Stage 2 changes between frameworks.**

Build the shell once and reuse it four times. This is what keeps the project achievable: one layout, four topologies.

> **Input → Live execution → Decision → Human review → Committed action**

### Stage 1: Input

Every run starts here, and the user sees exactly what is handed to the agents before anything happens.

A panel with a scenario picker (choose a lead, a ticket, or a content brief from the seed data) and the selected record shown as a structured, readable card. Fields are editable, so a resource can change a value and watch the agents behave differently. One obvious **Run** button starts it.

Nothing is hidden here. The user should be able to point at the screen and say "this is the input, watch what the team does with it."

### Stage 2: Live execution

Once running, a **Now Playing** banner sits above everything and is the most prominent thing on screen:

- The active agent, by name and role, for example "Quinn, Qualify Node"
- What it is doing, in one plain sentence, for example "Scoring Owen Castellan against qualification criteria"
- A status pill: thinking, calling model, using tool, routing, waiting, retrying, done
- Step counter, for example "step 7 of 14", and elapsed time

Below it, everything updates together in real time: the topology animates, the waterfall grows, the trace tree expands, the log appends. The framework-specific topology visual (Section 5) lives here.

The user can pause at any moment and click any agent to see what it is thinking right now.

### Stage 3: Decision

When the team reaches a decision point, the run visibly pauses and a decision panel takes focus. A distinct, deliberate moment, not just another log row.

It shows which agent made the call, the options available, which was taken, the full reasoning, and what prior agent output it relied on. Decision points are also marked on the timeline with a distinct marker, so a completed run can be scrubbed by decision.

### Stage 4: Human review

**The agents do not get the last word. This stage is mandatory and is a headline feature of the product.**

When the agent team finishes its work, it does not execute. It submits a recommendation to a human and waits. The interface enters a clearly marked **Awaiting human review** state, visually distinct from both running and completed.

The review panel shows:

- **The recommendation**, stated plainly in one sentence, for example "Move Owen Castellan to Propose and generate a proposal" or "Approve the SSO launch copy for publication"
- **The team's confidence**, and any caveat the agents themselves raised
- **Contribution by agent.** One row per agent that participated: name, role, what it contributed, and the single most important line from its output
- **The reasoning chain**, collapsed by default, expandable in full
- **Cost so far**: tokens, spend, duration
- A link into the full trace for anyone who wants to audit it

Four actions are available to the human:

1. **Approve and execute.** Accept the recommendation as it stands. Proceeds to Stage 5.
2. **Edit and execute.** Adjust the proposed action inline, then execute the amended version. The amendment is recorded as a human edit, distinct from what the agents proposed.
3. **Retry with guidance.** A free-text box where the human explains what they want done differently, for example "the discount is too aggressive, keep it under 10 percent" or "this lead is not ready, the budget was never confirmed." The run restarts with that guidance injected.
4. **Reject.** Stop, record the reason, take no action.

**On retry, the human guidance must appear as a visible input inside the new trace,** shown as a distinct human-authored span at the top, styled differently from agent spans. The user should be able to watch the agents read the human's note and change their behaviour because of it. Show the two runs side by side or in sequence so the difference is directly comparable.

This retry loop is the single most compelling thing to demo to a client. Give it real weight in the design.

### Stage 5: Committed action

Only after human approval does anything actually happen.

An **Action Receipt** appears showing:

- The action taken, stated plainly, for example "Lead moved from Nurture to Propose"
- A before and after of the business record, changed fields highlighted
- Contribution by agent, one row each
- **Who approved it**, which of the four actions they chose, and any guidance or edit they supplied
- Total tokens, cost, and duration across all attempts including retries
- A permanent link back to the full trace

The receipt is then written into the business layer. It is a permanent record, not a popup that disappears.

---

## 4. Framework identity

**This is what makes the four screens feel genuinely different rather than one screen with a changed title.** Every framework tab carries a persistent identity header, always visible, never scrolled away.

The header states, in this order:

1. **Framework name and version**, with its logo mark or a distinct colored glyph
2. **Team name**, the business team this framework is running
3. **What the team is for**, one sentence
4. **The roster**, every agent as a small avatar chip with name and role, the active one highlighted during a run
5. **Currently working on**, the specific record being processed right now
6. **Framework vocabulary**, a small strip naming what this framework calls its parts, so the differences are on screen rather than assumed

Each framework also owns an accent color used consistently across its topology, spans, chips, and header. Switching tabs should feel like walking into a different room.

### Team 1

- **Framework:** LangGraph
- **Team:** Revenue Desk
- **Purpose:** Move inbound leads from first contact to a closed decision
- **Accent:** teal
- **Vocabulary strip:** Nodes, Edges, Conditional Edges, State Schema, Checkpoints
- **Roster:**
  - **Ingrid**, Intake Node, validates and logs the incoming lead
  - **Quinn**, Qualify Node, scores against criteria and routes the lead
  - **Nadia**, Nurture Node, builds follow-up sequences for leads that are not ready
  - **Pablo**, Propose Node, drafts the proposal for qualified leads
  - **Cleo**, Close Node, records the final outcome

The agent initials deliberately spell the node sequence, I Q N P C, so the framework's own structure is learnable from the roster.

### Team 2

- **Framework:** CrewAI
- **Team:** Delivery Pod
- **Purpose:** Take a ticket from request to signed-off delivery
- **Accent:** amber
- **Vocabulary strip:** Agents, Roles, Goals, Backstories, Tasks, Hierarchical Process, Delegation
- **Roster:**
  - **Priya Menon**, Product Manager, breaks the ticket down, sets acceptance criteria, and signs off
  - **Dev Kapoor**, Senior Developer, implements the task
  - **Quentin Ash**, QA Engineer, reviews the implementation against acceptance criteria

Priya appears twice in a run, once to plan and once to sign off. Show that clearly, because a manager returning for a second turn is characteristic of CrewAI's hierarchical process.

### Team 3

- **Framework:** AutoGen
- **Team:** Content Studio
- **Purpose:** Draft and revise marketing content until it clears brand and compliance
- **Accent:** indigo
- **Vocabulary strip:** Conversable Agents, Group Chat, Speaker Selection, Termination Condition, Max Rounds
- **Roster:**
  - **Wren**, Copywriter, drafts and revises the content
  - **Cass**, Brand and Compliance Critic, reviews against the brief's constraints
  - **Mod**, Chat Manager, selects the next speaker and enforces the termination condition

Mod is not doing the work, it is running the conversation. Make that visible, because a dedicated orchestrator agent is characteristic of AutoGen and has no equivalent in the other three.

### Team 4

- **Framework:** Paperclip
- **Team:** Executive Brief
- **Purpose:** Synthesize the other three teams into one strategic recommendation
- **Accent:** violet
- **Vocabulary strip:** to be confirmed by the resource during research
- **Roster:**
  - **Sol**, Synthesis Agent, consumes the three team feeds and produces a recommendation

Paperclip operates at a different level from the other three. It is a control plane for running agents as an organization, with org charts, goal decomposition, task routing, and token budget caps. Reflect that in this header by showing the org structure and a budget indicator alongside the roster, so the difference in level is visible rather than stated.

---

## 5. Screens

Primary navigation, by framework, never by business team:

`LangGraph | CrewAI | AutoGen | Paperclip | Compare | Board`

A persistent top bar shows, for the loaded run: run ID, status, steps, total tokens, total cost, duration, and model. Playback controls live here and apply everywhere: play, pause, step forward, step back, reset, speed.

### Screen A: Run Inspector

**The hero screen and the default landing view.** Build this first and give it the most attention. The five stages of the core loop play out here. The Stage 1 input panel collapses once a run starts, the Now Playing banner replaces it, the decision panel takes focus at a decision, the human review panel takes focus at the end, and the receipt appears after approval. The three panes below stay available throughout.

**Left pane, run list and trace tree.** A list of runs for this framework, each row showing run ID, scenario, steps, tokens, cost, duration, and a status chip (completed, awaiting review, completed with retry, rejected, terminated at max rounds). Below it, the trace tree: a collapsible hierarchy of every span, agent turns, model calls, tool calls, routing decisions, state writes, and human interventions, nested to show what called what. Auto-expands to follow playback.

**Center pane, execution timeline and topology.** The framework's live topology on top (see below), a horizontal waterfall of spans underneath. Bars positioned by start time, sized by duration, colored by span type. Human spans styled distinctly from agent spans.

The topology per framework, and this is what makes the four visibly different:

- **LangGraph:** the node graph, active node pulsing, traversed edges lit. Conditional branches show which way they went and why. The state object sits alongside, updating live.
- **CrewAI:** the role hierarchy, Priya above Dev and Quentin, with an animated handoff arrow when a task is delegated and a visible return arrow when it comes back.
- **AutoGen:** conversation topology, speaker turns arcing between Wren, Cass, and Mod, with the round counter and termination condition on display.
- **Paperclip:** three input streams converging into Sol.

**Right pane, span inspector.** Everything about the selected span:

- **Reasoning**, the agent's thinking in full. The most important panel in the application. Never truncate it.
- **Prompt**, exactly what was sent, system prompt collapsible above
- **Completion**, the raw output
- **Tokens and cost**, prompt tokens, completion tokens, total, model name, call cost, share of run total
- **Timing**, start offset and duration
- **State diff**, before and after with changed fields highlighted. Central for LangGraph, so make it prominent there.
- **Decision**, options available, option taken, stated reason

**Bottom strip, run log.** Persistent, timestamped, scrollable, appended live, filterable by span type, human entries visually distinct.

### Screen B: Framework Anatomy

One per framework. A static explainer of that framework's coordination model beside a live example from a real run. Uses the framework's own vocabulary throughout. Ends with a short note on what the model is good at and where it strains.

### Screen C: Compare

The screen that carries the point of the project.

Top: the same conceptual task through all four frameworks, four columns, each with its topology in miniature and its headline numbers (steps, tokens, cost, duration, retries, human interventions).

Middle: a comparison table across coordination model, how control flows, where state lives, how failure and retry are handled, how a run terminates, where the human gate sits, observability out of the box, and token efficiency.

Bottom: the honest verdict, one short paragraph per framework on the kind of business problem it fits, backed by what is visible above.

### Screen D: Board

The downstream business layer. The funnel, ticket board, and content pipeline. Keep it simple and small.

**Every card carries its agent history:**

- A badge showing how many agents touched it, its token cost, and whether a human approved, edited, or retried it
- Clicking the card opens the Action Receipt, not a business detail form
- Cards worked more than once hold one receipt per attempt, in order, so retry history is visible on the card
- Human-approved, human-edited, and human-rejected cards are visually distinguishable at a glance

A card an agent team moved should never look like a card a human moved. Agent provenance is the point. When a run is approved on Screen A, the matching card visibly updates here.

Scenario data: eight leads for Revenue Desk (including Dana Whitfield at Northbridge Logistics, Renata Ibori at Kestrel Financial Services, Owen Castellan at Brackenfield Insurance), six tickets for Delivery Pod (CSV export, invoices pagination bug, SAML single sign-on, admin audit log), four content briefs for Content Studio (SSO launch announcement, plain-language SOC2 explainer), and one synthesis for Executive Brief.

---

## 6. Design system

A soft, tactile, **neumorphic** interface. Calm and physical rather than flat and clinical, but restrained. This gets shown to clients, so it must look considered without looking like a design experiment.

### The critical rule

**Neumorphism is for chrome, never for content.**

Panels, cards, buttons, toggles, chips, headers, and controls are neumorphic, soft extruded shapes with dual shadows. Anything the user actually reads closely, prompts, completions, reasoning text, state objects, log lines, sits in a **flat inset well with high-contrast text**. Soft low-contrast styling on dense monospace text is unreadable on a projector and will ruin the demo.

### Palette

Light neumorphic base:

- Surface base: `#E6EAF0`
- Raised surfaces: same base, lifted by shadows only, never by a different fill
- Content wells: `#F7F9FC` inset, or `#1E2430` inset with light text where a code or log feel is wanted
- Primary text: `#1B2330`
- Secondary text: `#5C6880`
- Hairlines: `#D2D9E4`

Framework accents, used for topology, span bars, active chips, and the identity header:

- LangGraph teal `#0E9F8E`
- CrewAI amber `#D98324`
- AutoGen indigo `#4B5BD7`
- Paperclip violet `#8250C4`

Status colors, consistent everywhere:

- Running `#4B5BD7`
- Awaiting human review `#D98324`, this state should feel like it is asking for attention
- Approved `#0E9F8E`
- Retried `#7A5AF8`
- Rejected `#C0453B`
- Human action `#111827`, human spans always read darker and heavier than agent spans

### Neumorphic treatment

- Raised: light shadow top-left `rgba(255,255,255,0.9)`, dark shadow bottom-right `rgba(163,177,198,0.55)`, offset 6px, blur 12px
- Inset, for wells and pressed states: the same two shadows inverted
- Corner radius 16px on panels, 12px on cards, 10px on buttons and chips
- Generous internal padding, 20px to 24px in panels
- No borders anywhere. Shadow does all the separation work.
- Elevation carries meaning: the active panel in the current stage sits highest, inactive panels sit flatter

### Type

- Interface: Inter or the system sans stack
- Prompts, completions, state, logs, run IDs: JetBrains Mono or the system mono stack
- Sizes: 12px labels, 14px body, 16px section headings, 22px screen titles
- Numbers in the metrics bar are tabular, so they do not jitter during playback

### Motion

- Panel and stage transitions 200ms ease-out
- Topology node activation pulses gently, roughly 900ms
- Waterfall bars draw in as they occur rather than appearing instantly
- Handoff and conversation arrows animate along their path
- All motion is subtle. Nothing bounces. Everything must be pausable.

### Restraint

- No gradients beyond the neumorphic shadows themselves
- No glassmorphism, no glow, no drop shadows on text
- Illustration only where it carries information, meaning the topologies
- Empty states are plain sentences, not artwork

Laptop width and above. Mobile is not required. The three-pane inspector must never collapse below laptop width, that layout is the whole point.

---

## 7. What makes this simple to build

Say this plainly so the scope reads as achievable:

- One shell, one identity header, one five-stage loop, reused across all four framework tabs
- Only the Stage 2 topology component differs per framework, four small components
- All data is a static fixture file per run, no backend logic, no state machine
- Playback is a timer walking an array of pre-written spans
- The human review gate is one panel with four buttons and a text box
- A retry is simply loading a second scripted run and marking the first span as human-authored

Four frameworks, but one application with four plug-ins.

---

## 8. Required scripted failures

Each must be fully inspectable in the Run Inspector with reasoning visible at every step. These are what make the reference honest.

- **LangGraph.** Owen Castellan scores below threshold at Qualify, the conditional edge routes to Nurture, and after a simulated response the run loops back to Qualify and routes to Propose. The state diff across the loop must be clearly visible, and the routing decision must show both branches and why one was taken.
- **CrewAI.** On the audit log ticket, Quentin fails Dev's first attempt because the implementation logs the action but not the actor, so the trail cannot answer who made a change. The task is delegated back and passes on the second round. Delegation and retry must appear as distinct spans.
- **AutoGen.** On the SSO launch brief, Wren's first draft claims accounts are "completely secure." Cass catches it against that brief's own stated constraint, quotes the constraint back, and Wren revises. The round counter advances visibly.
- **Paperclip.** Sol surfaces a genuine tension between its three inputs rather than averaging them into a bland summary.
- **Human retry, required.** At least one run must be sent back by the human with written guidance, re-run with that guidance visible as a human span, and produce a materially different recommendation the second time. Show both runs so the difference is directly comparable. This is the demo moment.
