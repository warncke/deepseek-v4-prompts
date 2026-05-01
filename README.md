# DeepSeek V4 Prompts — Automated Code Generation Workflow

This directory contains a set of **agent prompts** designed to work together in a 3-stage automated code generation pipeline. The workflow transforms a rough system idea into a fully implemented npm module with tests, linting, and coverage enforcement.

---

## Pipeline Overview

```
User's rough system idea
        │
        ▼
┌─────────────────────────────────────┐
│  system-design-agent.md             │  Stage 1: Design → Specification
│  (conversational refinement)        │
│  → generate technical specification │
└─────────────────────────────────────┘
        │
        ▼  technical-specification.md
        │
┌─────────────────────────────────────┐
│  technical-specification-review-    │  Stage 2: Review → Refinement
│  agent.md                           │
│  (7-expert audit panel)             │
│  → Consolidated Revisions           │
└─────────────────────────────────────┘
        │
        │ (feedback loop back to Stage 1 for refinement)
        ▼  refined technical-specification.md
        │
┌─────────────────────────────────────┐
│  instantiate.md                     │  Stage 3: Specification → Code
│  (TypeScript developer agent)       │
│  → Complete npm package             │
│    • src/*.ts + tests               │
│    • package.json, tsconfig.json    │
│    • ESLint + Prettier config       │
│    • Jest with 90% coverage         │
│    • README.md                      │
│  → npm run coverage (must exit 0)   │
└─────────────────────────────────────┘
        │
        ▼  Complete npm module
```

---

## Stage 1: Design → Specification

**Prompt file:** [`system-design-agent.md`](./system-design-agent.md)

A conversational **senior cryptographic systems architect** that:

1. **Accepts a rough system description** and asks clarifying questions (one at a time) about security, modularity, and design choices.
2. **Proposes concrete improvements** aligned with well-understood design principles (decoupling, separation of secrets, defense-in-depth).
3. **Produces artifacts on demand** via these commands:
   - `generate sequence diagram` — Mermaid `sequenceDiagram` of data/processing flow
   - `generate architecture diagram` — Mermaid `graph TB` C4 container/component diagram
   - `generate class specification` — Complete TypeScript interface specifications for every class
   - `generate manim animation` — Python/Manim visualization of the state machine
   - `generate d3 animation` — Self-contained HTML/D3.js browser animation
   - `generate testing plan` — Unit and E2E test matrix
   - `generate technical specification` — The full specification document
   - `revise technical paper` / `generate technical paper` — Incremental revision workflow

**Key design principles** enforced: independent secrets, no unnecessary coupling, all-or-nothing seed verification, portability to C/Rust/TypeScript, testing plan always included.

### Output: Technical Specification

The `generate technical specification` command produces a document following this structure:

1. **Overview** — purpose, components, data flow summary
2. **Component Specifications** — complete TypeScript class interfaces (no implementation)
3. **System Architecture** — C4 container diagram (Mermaid `graph TB`)
4. **Detailed Data Flow** — sequence diagram (Mermaid `sequenceDiagram`)
5. **Visualisation** — D3 animation (included if it exists)
6. **Testing Requirements** — unit tests and E2E test strategy
7. **CLI Entry Point** — environment variables and startup commands

**Example:** [`technical-specification.example.md`](./technical-specification.example.md) shows a complete specification for "LogsR" — a distributed append-only log server.

---

## Stage 2: Review → Refinement

**Prompt file:** [`technical-specification-review-agent.md`](./technical-specification-review-agent.md)

A **7-expert review panel** that critically audits a technical specification. Each expert translates findings into a unified computational modeling vocabulary (nodes, edges, flows, constraints) and assigns severity.

### Panel Members

| #   | Expert                            | Domain Focus                                                                |
| --- | --------------------------------- | --------------------------------------------------------------------------- |
| 1   | **CryptographyExpert**            | Primitives, randomness, side-channels, forward secrecy, algorithm selection |
| 2   | **DigitalPhysicalSecurityExpert** | Network threats, access control, TLS, rate limiting, incident response      |
| 3   | **DistributedSystemsExpert**      | Consistency, fault tolerance, partitions, idempotency, back-pressure        |
| 4   | **SoftwareEngineeringExpert**     | Correctness, invariants, testability, portability, decoupling               |
| 5   | **UserExperienceExpert**          | API clarity, error messages, terminology, documentation                     |
| 6   | **LegalComplianceExpert**         | GDPR, data retention, export controls, audit trails                         |
| 7   | **EnergyAnalysisExpert**          | Bottlenecks, I/O patterns, caching, algorithmic efficiency                  |

### Output Format

Two sections only:

1. **Part 1 — Consolidated Revisions**: Issues sorted by severity (Critical > Major > Minor), then by domain priority. Each revision references the specific section, quotes original text, proposes a change, and cites the originating expert.
2. **Part 2 — Debug Tallies**: Raw top-10 issues per expert for transparency.

### Feedback Loop

The consolidated revisions from Stage 2 can be fed back into the Stage 1 agent (`system-design-agent.md`) for refinement. Revision commands can be applied individually, and the full revised specification is regenerated only on explicit `generate technical paper` confirmation.

### Custom Review Panels

Two additional agents support more complex review setups:

- **[`expert-panel-creation-agent.md`](./expert-panel-creation-agent.md)** — A meta-agent that helps you design **bespoke multi-expert review panels** using the "weight-revelation method". Generates 10 domain-specific evaluation methods per expert. Supports panel diagrams, resolution logic (TypeScript), and full review prompt generation.

- **[`expert-panel-creation-from-technical-specification.md`](./expert-panel-creation-from-technical-specification.md)** — Creates **recursive, per-module audit panels** by parsing the C4 architecture from a specification. Enables impact analysis: "If we change this module, what breaks in its parents and children?" Produces bottom-up audit ordering and finding propagation rules.

---

## Stage 3: Specification → Code

**Prompt file:** [`instantiate.md`](./instantiate.md)

A **TypeScript developer agent** that reads the technical specification and produces a complete, production-ready npm package.

### What It Generates

| File                    | Purpose                                                                             |
| ----------------------- | ----------------------------------------------------------------------------------- |
| `package.json`          | ESM module (`"type": "module"`), exact devDependencies, build/test/coverage scripts |
| `tsconfig.json`         | Strict mode, ES2022 target, NodeNext module resolution, declarations                |
| `tsconfig.build.json`   | Extends tsconfig, excludes test files from production build                         |
| `.eslint.config.js`     | Flat config with `@eslint/js` and `@typescript-eslint`                              |
| `.prettierrc`           | Semi, single quotes, trailing commas                                                |
| `.prettierignore`       | Ignores `lib/`, `node_modules/`, `coverage/`                                        |
| `.vscode/settings.json` | Local TypeScript SDK for IntelliSense consistency                                   |
| `jest.config.ts`        | `ts-jest` with ESM, 90% coverage thresholds                                         |
| `.gitignore`            | `node_modules/`, `lib/`, `coverage/`, `.env`, `*.db`                                |
| `src/*.ts`              | One file per logical component, co-located tests                                    |
| `src/index.ts`          | Main entry point, exports all public API                                            |
| `README.md`             | Developer + end-user documentation                                                  |

### Non-Negotiable Rules

- **90% coverage threshold** for branches, functions, lines, and statements — enforced in `jest.config.ts`. The agent must add tests (not lower thresholds) if coverage is insufficient.
- **ESM throughout** — all relative imports end with `.js`.
- **TypeScript strict mode** with `NodeNext` module resolution.
- **No inheritance** in class design — flat, portable-to-C/Rust structures.
- **Post-generation verification loop**: `npm install` → `npm run build` → `npm run coverage` (must exit 0).

---

## How to Use This Workflow

### Full Pipeline (recommended)

1. **Prepare your system idea** — a rough description of what you want to build (e.g., "a distributed append-only log server with replication and access control").

2. **Run Stage 1** — Feed your idea into `system-design-agent.md`. Engage in the conversational loop until the design is settled, then invoke `generate technical specification` to produce the spec.

3. **Run Stage 2** — Feed the generated spec into `technical-specification-review-agent.md`. Apply the consolidated revisions back to the spec (via Stage 1's `revise technical paper` / `generate technical paper` commands).

4. **Iterate** — Repeat Stages 1 and 2 until the spec is solid.

5. **Run Stage 3** — Feed the final `technical-specification.md` into `instantiate.md`. The agent will produce the full npm package and verify it against the 90% coverage threshold.

### Targeted Use

- **Just the design agent**: Use `system-design-agent.md` alone for rapid prototyping and architectural exploration.
- **Just the review panel**: Use `technical-specification-review-agent.md` (or the custom panel agents) to audit any existing specification.
- **Just the instantiation agent**: Use `instantiate.md` with an already-written specification to generate the codebase.
- **Custom panels**: Use `expert-panel-creation-agent.md` to build domain-specific review panels for any artifact type.

---

## File Index

| File                                                                                                               | Lines | Role                                             |
| ------------------------------------------------------------------------------------------------------------------ | ----- | ------------------------------------------------ |
| [`system-design-agent.md`](./system-design-agent.md)                                                               | 235   | Stage 1: Interactive system design agent         |
| [`technical-specification.example.md`](./technical-specification.example.md)                                       | 1102  | Example output from Stage 1 (LogsR spec)         |
| [`technical-specification-review-agent.md`](./technical-specification-review-agent.md)                             | 213   | Stage 2: 7-expert specification review panel     |
| [`expert-panel-creation-agent.md`](./expert-panel-creation-agent.md)                                               | 211   | Custom panel builder (meta-agent)                |
| [`expert-panel-creation-from-technical-specification.md`](./expert-panel-creation-from-technical-specification.md) | 250   | Recursive per-module audit panel builder         |
| [`instantiate.md`](./instantiate.md)                                                                               | 257   | Stage 3: Spec-to-code TypeScript developer agent |
