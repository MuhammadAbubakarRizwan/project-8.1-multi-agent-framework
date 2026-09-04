# Popular Multi-Agent Frameworks and Tools

Background reading. This project uses four of these, LangGraph, CrewAI, AutoGen, and Paperclip. The rest are here so you know what else exists, what the wider landscape looks like, and what to reach for on a real client problem later.

You are not required to learn the frameworks outside your four. Read this once for orientation, then go deep on the four the project assigns.

Star counts are approximate and move quickly. Treat them as a rough signal of adoption, not a ranking of quality.

---

## Comparative Overview

| Framework / Tool | GitHub Stars | Core Paradigm | Key Value Proposition |
| :--- | :--- | :--- | :--- |
| **LangGraph** | ~134k+ (ecosystem) | Directed Graphs / State Machines | Fine-grained state control, cycle handling, and deterministic execution |
| **Paperclip** | ~75k+ | Zero-Human Org Management | Goal-based company orchestration with budget caps and management UI |
| **CrewAI** | ~50k | Role-Based Agents and Tasks | Clean Python abstractions (Roles, Goals, Backstories) for quick setup |
| **AutoGen** | ~45k to 50k | Multi-Party Conversational Chat | Dynamic agent-to-agent dialogues, code execution, and consensus building |
| **Agno** (Phidata) | ~41k+ | Full-Stack Multi-Agent Teams | Integrated agent engine with built-in database memory and UI interfaces |
| **MetaGPT** | ~40k+ | SOPs and Role-Play Mechanics | Enforces structured document handoffs (PRDs, UML) to reduce loops |
| **LlamaIndex Workflows** | ~38k+ (ecosystem) | Event-Driven Pub/Sub | Ideal for heavy search, RAG pipelines, and document-driven workflows |
| **ChatDev** | ~25k | Simulated Dev Company | Phase-based software engineering (Design, Code, Test, Document) |
| **OpenAI Agents SDK** | ~22k | Handoffs and Routines | Minimalist, low-overhead SDK for routing tasks between specialized agents |
| **smolagents** | ~12k to 15k | Code-Executing Agents | Lightweight (~1k lines of code), agents write Python snippets instead of JSON |

---

## Detailed Breakdown by Architecture

### 1. Enterprise and Deterministic State Management

#### LangGraph

- **Paradigm:** Directed graphs and state machines.
- **Best for:** Complex, mission-critical production systems requiring precise flow control.
- **Key features:**
  - Supports cyclical graph topologies, conditional branching, and explicit state schemas.
  - Native persistence layer for human-in-the-loop approvals, time travel, and checkpointing.
  - Designed for developers who need strict control over every execution step.

#### Paperclip (`paperclipai/paperclip`)

- **Paradigm:** AI company management and task delegation.
- **Best for:** Managing teams of autonomous agents like an organizational workforce.
- **Key features:**
  - Higher-level control plane featuring org charts, goal decomposition, and task routing.
  - Enforces token budget caps and scheduled heartbeat executions per agent.
  - Includes a central React dashboard for real-time tracking of active workers and goals.

---

### 2. High-Ergonomics and Role-Based Workflows

#### CrewAI

- **Paradigm:** Role-based autonomous crews.
- **Best for:** Rapid prototyping of collaborative agent teams.
- **Key features:**
  - Intuitive Python syntax centered around `Agent`, `Task`, and `Crew` abstractions.
  - Built-in support for sequential or hierarchical task execution and delegation.
  - Native tool-calling integration with low boilerplate requirements.

#### MetaGPT

- **Paradigm:** Standard Operating Procedures (SOPs).
- **Best for:** Document-driven software development and business spec workflows.
- **Key features:**
  - Assigns roles (Product Manager, Architect, Engineer) governed by structured SOPs.
  - Generates intermediate artifacts (PRDs, architecture diagrams, code files) during handoffs.
  - Minimizes infinite conversational loops by requiring formal deliverable outputs.

---

### 3. Conversational and Simulation Systems

#### AutoGen (Microsoft)

- **Paradigm:** Multi-agent conversation chains.
- **Best for:** Complex agent-to-agent negotiations, research debates, and automated coding loops.
- **Key features:**
  - Native patterns for multi-party group chats and dynamic speaker selection.
  - Flexible code-execution environments (Docker or local sandbox) for agent-written code.
  - Highly extensible for custom conversation management logic.

#### ChatDev

- **Paradigm:** Virtual software company.
- **Best for:** Autonomous end-to-end software prototyping.
- **Key features:**
  - Structures agent communication across distinct phases (Designing, Coding, Testing, Documenting).
  - Uses specialized agent pairs in focused dialogs to complete phase requirements.

---

### 4. Full-Stack and Lightweight Developer Frameworks

#### Agno (formerly Phidata)

- **Paradigm:** Full-stack agent applications.
- **Best for:** User-facing applications requiring built-in state, memory, and database layers.
- **Key features:**
  - Out-of-the-box support for vector database storage, session state, and multi-modal handling.
  - Pre-built UI components and control planes for viewing agent operations.

#### smolagents (Hugging Face)

- **Paradigm:** Minimalist code-executing agents.
- **Best for:** Lightweight Python scripts and minimal library overhead.
- **Key features:**
  - Very small codebase footprint, around 1,000 lines of code.
  - Prompts models to produce executable Python snippets directly, bypassing fragile JSON schemas.

#### OpenAI Agents SDK (formerly Swarm)

- **Paradigm:** Explicit handoffs and lightweight routines.
- **Best for:** Lightweight task delegation and triage routing.
- **Key features:**
  - Built on two basic primitives, `Agent` and `Handoff`.
  - Simple, unopinionated routing mechanism for passing conversation state between agents.

---

## Decision Matrix

Start from the problem, not the framework. The question that matters is how work needs to move between agents.

| If the problem looks like this | Reach for | Because |
| :--- | :--- | :--- |
| A process with defined stages, branches, and loops back to earlier stages | **LangGraph** | Explicit state and conditional edges make the flow auditable and repeatable |
| A team of specialists where one plans and others execute | **CrewAI** | Roles, goals, and delegation map directly onto how the team already works |
| Work that improves through critique and revision rather than one pass | **AutoGen** | Conversation and speaker selection are the native primitives |
| An ongoing organization of agents with budgets and standing goals | **Paperclip** | Org charts, goal decomposition, and spend caps are built in |
| Structured deliverables must be produced at each handoff | **MetaGPT** | SOPs force formal artifacts and cut down aimless looping |
| Retrieval and document pipelines are the bulk of the work | **LlamaIndex Workflows** | Event-driven design suits search and RAG-heavy flows |
| A user-facing product needing memory, storage, and a UI on day one | **Agno** | Ships the full stack rather than orchestration alone |
| Simple routing between a few specialists, nothing more | **OpenAI Agents SDK** | Two primitives, minimal overhead, little to learn |
| The agent's real job is computation, and you want minimal machinery | **smolagents** | Agents emit Python directly instead of brittle JSON tool calls |
| End-to-end software prototyping as a demonstration | **ChatDev** | Phase-based dev company simulation out of the box |

### Questions to ask before choosing

1. Does the work loop back on itself, or does it only move forward? Loops push you toward LangGraph or AutoGen.
2. Where does state live, and who is allowed to change it? Explicit state pushes you toward LangGraph.
3. Does a human need to approve something mid-flow? Check that the framework has a real human-in-the-loop story rather than a workaround.
4. What happens when an agent fails or produces something wrong? Retry and recovery behaviour differs sharply between these.
5. How will you know what it cost? Some frameworks expose tokens and spend natively, others leave you to instrument it yourself.
6. Can you explain the control flow to a client in one diagram? If not, the framework is probably wrong for that problem.

The last question is the one that matters most in the room.
