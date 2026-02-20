# AI Agent Guide for SOP-Driven Development

> **This document explains how to use the SOP framework with AI coding agents.**
>
> The SOPs are designed as a **single source of truth** for both human developers and AI agents, enabling consistent, efficient, and auditable software development.

---

## AI Agent Responsibilities

The AI agent is responsible for **managing the entire session lifecycle**:

| Responsibility         | When             | What to Update                                                      |
| ---------------------- | ---------------- | ------------------------------------------------------------------- |
| **Initialize session** | First prompt     | Fill Project Overview, Goals from user description                  |
| **Track progress**     | After each SOP   | Update Progress Tracker status and outputs                          |
| **Update checklist**   | After each SOP   | Check off completed items in `.sops/templates/project-checklist.md` |
| **Commit changes**     | After each SOP   | Git commit with conventional message (see Version Control)          |
| **Run checkpoint**     | After each phase | Verify alignment with requirements (see Checkpoint System)          |
| **Maintain context**   | After each SOP   | Update Current Session, Session Prompt Template                     |
| **Log sessions**       | End of session   | Add entry to Session Log                                            |
| **Resume context**     | New session      | Read AI-SESSION.md, continue from last SOP                          |

**The human only provides:**

- Initial project name and description
- Answers to clarifying questions
- Approval/feedback on outputs

---

## Version Control Guidelines

The AI agent is responsible for committing changes at appropriate checkpoints:

### When to Commit

| Trigger                                   | Commit Type  | Message Pattern                | Example                              |
| ----------------------------------------- | ------------ | ------------------------------ | ------------------------------------ |
| **After completing an SOP**               | Final        | `feat(sop-XXX): {description}` | `feat(sop-101): add database schema` |
| **Significant progress within large SOP** | Intermediate | `wip(sop-XXX): {description}`  | `wip(sop-200): add user endpoints`   |
| **Bug fix during development**            | Fix          | `fix: {description}`           | `fix: correct validation logic`      |
| **Documentation updates**                 | Docs         | `docs: {description}`          | `docs: update API reference`         |

### Key Rules

1. **Commit after each SOP** — One atomic commit per completed SOP (easy to track/revert)
2. **Large SOPs may have multiple commits** — Use `wip:` for intermediate, `feat:` for final
3. **Always commit before switching SOPs** — Never carry uncommitted changes across SOPs
4. **Commit before ending a session** — Ensure no work is lost between sessions

### Commit Message Format

Follow [Conventional Commits](https://www.conventionalcommits.org/):

```
<type>(scope): <description>

[optional body]
```

**Types:** `feat`, `fix`, `docs`, `chore`, `refactor`, `test`, `wip`  
**Scope:** Usually the SOP number (e.g., `sop-101`) or component name

---

## Checkpoint System (Drift Prevention)

AI agents can drift from original specifications over time. This **hierarchical validation system** catches drift at multiple levels.

### Validation Hierarchy

```
┌─────────────────────────────────────────────────────────────────┐
│  LEVEL 0: SOURCE OF TRUTH (Human-Approved)                      │
│  └── requirements.md, tech-stack.md                             │
│      Owned by: Project Lead | Changes require approval          │
├─────────────────────────────────────────────────────────────────┤
│  LEVEL 1: DESIGN DECISIONS (SOP Documentation Outputs)          │
│  └── schema.md, api-endpoints.md, component-architecture.md     │
│      Must conform to: Level 0                                   │
├─────────────────────────────────────────────────────────────────┤
│  LEVEL 2: IMPLEMENTATION (Actual Code)                          │
│  └── prisma/schema.prisma, src/app/api/**, components/**        │
│      Must conform to: Level 1 (which conforms to Level 0)       │
└─────────────────────────────────────────────────────────────────┘

Validation flows DOWNWARD:
  Level 0 → Level 1: Do design docs honor requirements?
  Level 1 → Level 2: Does code honor design docs?
```

### Phase-Specific Validation Layers

| Phase       | Level 1 (Design Docs)                                     | Level 2 (Code)                        |
| ----------- | --------------------------------------------------------- | ------------------------------------- |
| **Phase 1** | `/docs/database/schema.md`, `/docs/database/decisions.md` | `prisma/schema.prisma`, seed files    |
| **Phase 2** | `/docs/api/endpoints.md`, `/docs/api/openapi.yaml`        | `src/app/api/**`, route handlers      |
| **Phase 3** | `/docs/frontend/components.md`, `/docs/frontend/state.md` | `src/components/**`, pages            |
| **Phase 4** | `/docs/ai/feasibility.md`, `/docs/ai/prompts.md`          | AI integration code, prompt templates |
| **Phase 5** | Test strategy in docs                                     | `__tests__/**`, test files            |
| **Phase 6** | `/docs/deployment/`, runbooks                             | `.github/workflows/**`, Dockerfiles   |

### When to Run Checkpoints

| Checkpoint | Trigger       | Validation Focus                                  |
| ---------- | ------------- | ------------------------------------------------- |
| **CP-1**   | After Phase 1 | Requirements → Database design → Schema code      |
| **CP-2**   | After Phase 2 | Requirements → API design → Route implementations |
| **CP-3**   | After Phase 3 | User stories → Component design → UI code         |
| **CP-4**   | After Phase 5 | Full stack validation before deployment           |

### Checkpoint Prompt Template

Use this hierarchical validation prompt at each checkpoint.

> **Note:** Document locations and key decisions should already be filled in the **Checkpoint Tracker** section of `AI-SESSION.md`. Reference that section instead of searching for files.

```markdown
## Alignment Checkpoint — Phase {X} Complete

Perform a **hierarchical drift analysis** before proceeding.

**First:** Read the Checkpoint Tracker in `.prompts/AI-SESSION.md` for pre-filled document locations and key decisions for this phase.

---

### Layer 0 → Layer 1: Design Alignment

**Re-read Source of Truth:**

- Read locations from AI-SESSION.md → Checkpoint Tracker → Source of Truth table

**Audit Design Documents for this phase:**

- Read locations from AI-SESSION.md → Checkpoint Tracker → Phase {X} → Design Docs table

**Report: Do design decisions honor requirements?**

| Requirement/User Story | Design Doc                | Addressed? | Location in Doc | Drift Notes |
| ---------------------- | ------------------------- | ---------- | --------------- | ----------- |
| {from requirements.md} | {from checkpoint tracker} | ✅/⚠️/❌   | Section X       |             |

**Design Drift Findings:**

- [ ] Design decisions that don't trace to requirements (scope creep)
- [ ] Requirements not addressed in design docs (gaps)
- [ ] Tech stack deviations

---

### Layer 1 → Layer 2: Implementation Alignment

**Audit Code against Design Documents:**

- Read locations from AI-SESSION.md → Checkpoint Tracker → Phase {X} → Implementation table

**Report: Does code honor design decisions?**

| Design Decision   | Expected Implementation | Actual Code | Compliant? | File:Line  |
| ----------------- | ----------------------- | ----------- | ---------- | ---------- |
| {from design doc} | {expected}              | {actual}    | ✅/❌      | {location} |

**Implementation Drift Findings:**

- [ ] Code that doesn't match design docs
- [ ] Design docs not yet implemented (incomplete)
- [ ] Undocumented implementations (rogue code)

---

### Summary & Actions

**Overall Alignment Score:**

- Level 0 → 1: \_\_\_% compliant
- Level 1 → 2: \_\_\_% compliant

**Critical Issues (Block Proceeding):**

1. ...

**Warnings (Track in Backlog):**

1. ...

**Recommendations:**

- [ ] Fixes required before next phase
- [ ] Design docs to update
- [ ] Requirements clarifications needed from project lead

---

**Update AI-SESSION.md:**

1. Set Checkpoint Status to ✅ Passed or ⚠️ Issues Found
2. Record Last Run date
3. Document any issues found

⏸️ **STOP: Await human approval before proceeding to next phase.**
```

### Quick Checkpoint (Mid-SOP)

For lightweight validation during complex SOPs:

```markdown
Quick alignment check:

1. What did I just implement? (List last 3 items)
2. For each, trace back:
   - Which design doc specifies this? (Level 1)
   - Which requirement/user story needs this? (Level 0)
3. Flag anything that doesn't trace back to Level 0
4. Flag any Level 0 requirement that should have been addressed but wasn't
```

### Recovery from Drift

If checkpoint reveals drift:

```markdown
Drift detected. Recovery procedure:

1. **Stop current work** — Don't compound the drift
2. **Identify drift type:**
   - Scope creep (added unrequested features) → Remove or get approval to add to requirements
   - Deviation (built differently than specified) → Refactor to match spec OR update spec with approval
   - Gap (missed requirement) → Implement missing piece
3. **Update tracking:**
   - Log drift in AI-SESSION.md Session Notes
   - Update design docs if spec change is approved
4. **Re-run checkpoint** after fixes
5. **Get approval** before proceeding
```

---

## Core Philosophy

```
┌─────────────────────────────────────────────────────────────────────┐
│                                                                     │
│   SOPs define HOW (the method)                                      │
│   Your prompts define WHAT (the subject)                            │
│   AI executes the intersection                                      │
│                                                                     │
│     ┌─────────────┐     ┌─────────────┐     ┌─────────────┐         │
│     │    SOP      │  +  │   Context   │  =  │   Output    │         │
│     │ (Procedure) │     │  (Project)  │     │  (Artifact) │         │
│     └─────────────┘     └─────────────┘     └─────────────┘         │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

### Key Principles

1. **Reference, Don't Repeat** — Point AI to SOP files instead of rewriting instructions
2. **Chain Dependencies** — Each SOP's outputs become the next SOP's inputs
3. **Track Progress** — Use the session template to maintain state across conversations
4. **Verify Outputs** — Check that AI-generated files match SOP requirements
5. **Document Deviations** — If the AI deviates from an SOP, capture why

---

## SOP Adaptation (Project-Specific Execution)

Generic SOPs use `[Entity]` placeholders and one-size-fits-all examples. This works for teaching the pattern, but when an AI agent executes them, it must mentally translate to the actual project — and this is where context gets lost, entities get forgotten, and wrong patterns get applied.

**Solution:** After Phase 0 completes, the AI generates a **Project Execution Brief** — a single document that specializes all remaining SOPs to the actual project.

### When to Generate

The Execution Brief is a **mandatory deliverable of Phase 0 completion**. Generate it after SOP-006 (Code Style Standards) is done, before starting Phase 1.

### What the Execution Brief Contains

Create `/docs/execution-brief.md`:

```markdown
# Project Execution Brief: {Project Name}

> Auto-generated from Phase 0 outputs. This document specializes
> all remaining SOPs to this project's specific context.

## Entity Map

Derived from `/docs/requirements.md`:

| Entity | Relationships                        | Used in SOPs            | Notes       |
| ------ | ------------------------------------ | ----------------------- | ----------- |
| User   | has many Lists, has many PantryItems | 101, 200, 201, 202, 305 | Auth entity |
| List   | belongs to User, has many Items      | 101, 200, 201, 202, 305 | Core entity |
| Item   | belongs to List                      | 101, 200, 201, 202, 305 |             |
| ...    | ...                                  | ...                     | ...         |

## Tech Stack Adaptations

Derived from `/docs/tech-stack.md`:

| SOP Area | Generic SOP Says | This Project Uses | Adaptation                                      |
| -------- | ---------------- | ----------------- | ----------------------------------------------- |
| Database | Raw Prisma       | Supabase + Prisma | Use Supabase client for auth, Prisma for data   |
| Auth     | NextAuth.js      | Supabase Auth     | Skip SOP-203 JWT sections, use Supabase session |
| Hosting  | Vercel           | Vercel            | No change                                       |
| Styling  | Tailwind CSS     | Tailwind CSS      | No change                                       |
| ...      | ...              | ...               | ...                                             |

## Pattern Overrides

Derived from `/docs/architecture/patterns.md`:

| Pattern    | SOP Default | This Project          | Reason                 |
| ---------- | ----------- | --------------------- | ---------------------- |
| Repository | Class-based | Function-based        | Simpler for team size  |
| State Mgmt | React Query | React Query + Zustand | Offline support needed |
| ...        | ...         | ...                   | ...                    |

## Feature Scope (MVP)

Derived from `/docs/requirements.md` MVP section:

| Feature   | Priority | Relevant SOPs      | Pages                    | API Resources |
| --------- | -------- | ------------------ | ------------------------ | ------------- |
| User Auth | Must     | 203, 204, 305      | Login, Register, Profile | /api/auth/\*  |
| Lists     | Must     | 200, 201, 202, 305 | Lists Index, List Detail | /api/lists/\* |
| ...       | ...      | ...                | ...                      | ...           |

## Phase Relevance

| Phase/SOP                | Status     | Reason                       |
| ------------------------ | ---------- | ---------------------------- |
| Phase 4 (AI Integration) | ⏭️ Skip    | No AI features in MVP        |
| SOP-306 (PWA)            | ⏭️ Skip    | Not needed for this project  |
| SOP-204 (Authorization)  | ✅ Execute | Multiple user roles required |
| ...                      | ...        | ...                          |

## Project-Specific Constraints

- {e.g., "Must work offline — PWA with local-first sync"}
- {e.g., "Budget: free tier only for all services"}
- {e.g., "Target: mobile-first, Arabic RTL support"}

## Placeholder Resolution

> SOPs use generic placeholders like `[Entity]` and `[Related]` in their
> code examples. These mean **different things depending on which entity
> you are currently implementing.** Use the matching card below.

### Global Placeholders (all SOPs)

| Placeholder      | Value          |
| ---------------- | -------------- |
| `[App Name]`     | {Project Name} |
| `[PROJECT_NAME]` | {Project Name} |

### Entity: {EntityA} (e.g., List)

| Placeholder       | Value                       |
| ----------------- | --------------------------- |
| `[Entity]`        | {EntityA} (e.g., `List`)    |
| `[entity]`        | {entityA} (e.g., `list`)    |
| `[entities]`      | {entitiesA} (e.g., `lists`) |
| `[Related]`       | {RelatedToA} (e.g., `Item`) |
| `[RelatedEntity]` | {RelatedToA} (e.g., `Item`) |
| `[related]`       | {relatedToA} (e.g., `item`) |

### Entity: {EntityB} (e.g., Item)

| Placeholder       | Value                       |
| ----------------- | --------------------------- |
| `[Entity]`        | {EntityB} (e.g., `Item`)    |
| `[entity]`        | {entityB} (e.g., `item`)    |
| `[entities]`      | {entitiesB} (e.g., `items`) |
| `[Related]`       | {RelatedToB or — if none}   |
| `[RelatedEntity]` | {RelatedToB or — if none}   |
| `[related]`       | {relatedToB or — if none}   |

{Repeat for each entity in the Entity Map above.}
```

### How the Agent Uses the Execution Brief

1. **Before starting any SOP from Phase 1 onward**, read `/docs/execution-brief.md`
2. **Resolve placeholders per entity** — when implementing a work unit for entity X, look up the "Entity: X" card in the Placeholder Resolution section and substitute all placeholders accordingly
3. **Apply tech adaptations** — use the correct tools/patterns for this project
4. **Check phase relevance** — skip SOPs marked ⏭️
5. **Scope to MVP features** — don't implement features marked as post-MVP

> **AI Agent Rule:** If the Execution Brief contradicts a generic SOP example, the Brief takes precedence. The SOPs define the _method_; the Brief defines the _subject_.
>
> **Placeholder Rule:** Never guess what `[Entity]` or `[Related]` means. Always look up the matching entity card in the Placeholder Resolution section. If there is no card for the entity you are implementing, stop and ask the human to update the Execution Brief.

---

## Iterative SOP Execution (Complex SOPs)

Some SOPs involve implementing multiple similar work units (e.g., "implement all pages" or "create all API endpoints"). These **must not** be executed in a single prompt — agents lose context and miss units.

### Which SOPs Are Iterative

| SOP     | Work Unit      | Iterate Over                                               |
| ------- | -------------- | ---------------------------------------------------------- |
| SOP-200 | Service        | Each service class (e.g., ListService, ItemService)        |
| SOP-201 | Repository     | Each repository class                                      |
| SOP-202 | Resource Group | Each API resource group (e.g., /api/lists/_, /api/items/_) |
| SOP-305 | Page           | Each user-facing page                                      |

### The Iterative Execution Pattern

```
┌────────────────────────────────────────────────────────────────┐
│  Step 1: BUILD MANIFEST                                        │
│  Read the Execution Brief + upstream design docs.              │
│  List ALL work units that must be implemented.                 │
│  Present manifest to human for approval.                       │
├────────────────────────────────────────────────────────────────┤
│  Step 2: IMPLEMENT ONE UNIT                                    │
│  For the next unit in the manifest:                            │
│  - Complete the FULL vertical slice (not just one layer)       │
│  - Cross-reference the Execution Brief for correct patterns    │
│  - Verify: does this unit match its design doc / requirement?  │
├────────────────────────────────────────────────────────────────┤
│  Step 3: CHECKPOINT                                            │
│  ⏸️ Stop and present the completed unit to the human:          │
│  - What was implemented                                        │
│  - Files created/modified                                      │
│  - How it maps to requirements                                 │
│  - Any deviations or decisions made                            │
│  Wait for approval before proceeding to the next unit.         │
├────────────────────────────────────────────────────────────────┤
│  Step 4: REPEAT Steps 2-3 for each remaining unit              │
├────────────────────────────────────────────────────────────────┤
│  Step 5: COVERAGE VERIFICATION                                 │
│  After all units are done:                                     │
│  - Cross-reference manifest against actual outputs             │
│  - Flag any units that were missed or partially done           │
│  - Report coverage: X/Y units complete                         │
│  ⏸️ Final approval before marking SOP complete.                │
└────────────────────────────────────────────────────────────────┘
```

### AI Agent Rules for Iterative SOPs

1. **Never skip the manifest step** — The manifest is what prevents forgetting units
2. **One unit at a time** — Do not start the next unit until the current one is checkpointed
3. **Full vertical slice** — Each unit must be complete (e.g., for a page: server component + client component + API wiring + loading states + error states)
4. **Use the Execution Brief** — Don't guess entity names or patterns; read the Brief
5. **Update AI-SESSION.md** after each unit — Track per-unit progress in the Iterative SOP Progress table
6. **Coverage check is mandatory** — Don't mark the SOP as complete until coverage is verified

## Understanding the SOP Structure

### Each SOP Contains

```markdown
# SOP-XXX: Title

## Purpose ← Why this SOP exists

## Scope ← What it covers / doesn't cover

## Prerequisites ← What must be done first (other SOPs, tools)

## Procedure ← Step-by-step instructions (AI follows this)

## Review Checklist ← Verification items

## AI Agent Prompt ← (Optional) Pre-written prompt template

## Outputs ← What files/artifacts this SOP produces

## Related SOPs ← Cross-references
```

### The AI Agent Should

1. **Read the full SOP** before starting
2. **Check prerequisites** are satisfied (read output files from previous SOPs)
3. **Follow the Procedure** section step-by-step
4. **Produce all Outputs** listed
5. **Verify against the Review Checklist**
6. **Update project-checklist.md** — check off completed items for the SOP

---

## Getting Started with a New Project

### Step 1: Set Up Project Structure

Copy the SOP framework into your new project:

```bash
# Create a new project directory
mkdir my-new-project && cd my-new-project

# Copy both folders as-is
cp -r /path/to/sops-repo/.prompts .
cp -r /path/to/sops-repo/.sops .
```

Resulting structure:

```
my-new-project/
├── .prompts/
│   ├── AI-GUIDE.md       # This guide
│   └── AI-SESSION.md     # Session tracker (AI manages this)
└── .sops/
    ├── README.md         # SOP index
    ├── templates/        # Project templates
    ├── phase-0-initialization/
    └── ...               # Other phases
```

### Step 2: Start Your First AI Session

Provide this simple prompt to your AI agent:

```markdown
I'm starting a new project: {PROJECT_NAME}.
{Brief 1-2 sentence description of what you're building}

Read `.prompts/AI-GUIDE.md` and `.prompts/AI-SESSION.md` to understand the workflow.
Then begin with SOP-000 (Requirements Gathering).
```

**The AI will automatically:**

1. Fill in the Project Overview in `AI-SESSION.md`
2. Extract goals from your description
3. Read SOP-000 and guide you through requirements gathering
4. Create the output files
5. Update the session tracker
6. Proceed to the next SOP

### Step 3: Continue Through SOPs

For subsequent sessions (or resuming in a new chat):

```markdown
Continuing work on this project.

Read `.prompts/AI-SESSION.md` to see progress and continue with the next SOP.
```

Or simply copy the **Session Prompt Template** from `AI-SESSION.md`—the AI keeps it updated with the current state.

---

## Prompt Patterns

### Pattern 1: Execute a Single SOP

```markdown
Execute SOP-{XXX}.

Read `.prompts/AI-SESSION.md` for context, then follow the SOP procedure.
Update the session tracker when complete.
```

### Pattern 2: Continue From Last Session

```markdown
Continuing this project.

Read `.prompts/AI-SESSION.md` and proceed with the next incomplete SOP.
```

### Pattern 3: Execute Multiple Related SOPs

```markdown
Execute these SOPs in order:

1. SOP-002 (Repository Setup)
2. SOP-003 (Project Structure)
3. SOP-004 (Environment Setup)

Update `.prompts/AI-SESSION.md` after each one.
```

### Pattern 4: Review and Verify

```markdown
## Context

Project: {name} at `{path}`
Just completed: SOP-{XXX}

## Task

Verify the outputs against SOP-{XXX}'s Review Checklist.

Read:

- `.sops/phase-X/{SOP-file}.md` (the checklist section)
- {files that were created}

Report:

- ✅ Items that pass
- ❌ Items that need attention
- Recommended fixes
```

### Pattern 5: Recover Context (New Session)

```markdown
I'm resuming work on this project.

Read `.prompts/AI-SESSION.md` and summarize:

- What's been completed
- What's next
- Any noted issues

Then await my instructions.
```

### Pattern 6: Execute Iterative SOP

```markdown
Execute SOP-{XXX} using the iterative pattern.

Read:

- `.prompts/AI-SESSION.md` for context
- `/docs/execution-brief.md` for project-specific adaptations
- The SOP file for the procedure

**Step 1:** Build the work unit manifest from the Execution Brief + upstream docs.
Present it to me for approval before implementing anything.

**Step 2:** After I approve, implement ONE unit at a time.
After each unit, checkpoint with me — show what was built, what files changed,
and how it maps to requirements. Wait for my approval before the next unit.

**Step 3:** After all units, run coverage verification against the manifest.
```

---

## 📊 SOP Dependencies Map

Understanding what each SOP needs and produces:

```
SOP-000 (Requirements)
    └── outputs: /docs/requirements.md
            │
            ▼
SOP-001 (Tech Stack) ◄── reads requirements
    └── outputs: /docs/tech-stack.md
            │
            ▼
SOP-002 (Repository) ◄── reads tech stack (for .gitignore)
    └── outputs: README.md, .gitignore, CONTRIBUTING.md
            │
            ▼
SOP-003 (Structure) ◄── reads tech stack (for folder conventions)
    └── outputs: folder structure
            │
            ▼
SOP-004 (Environment) ◄── reads structure
    └── outputs: .env.example, setup docs
            │
            ▼
SOP-005 (Patterns) ◄── reads requirements + tech stack
    └── outputs: /docs/architecture/design-patterns.md
            │
            ▼
SOP-006 (Code Style) ◄── reads tech stack
    └── outputs: linter/formatter configs
            │
            ▼
        [Phase 1+]
```

---

## ✅ Best Practices

### For Effective AI Sessions

| Do                               | Don't                               |
| -------------------------------- | ----------------------------------- |
| Reference SOP files by path      | Copy-paste SOP content into prompts |
| Let AI read prerequisite outputs | Summarize previous work yourself    |
| Use the session tracker          | Rely on chat memory across sessions |
| Verify outputs match SOP specs   | Assume AI followed everything       |
| Update tracker after each SOP    | Forget to record progress           |

### For SOP Maintenance

| Do                                    | Don't                       |
| ------------------------------------- | --------------------------- |
| Keep Outputs section accurate         | Let outputs become outdated |
| Update Prerequisites when adding SOPs | Create orphan SOPs          |
| Include concrete examples in SOPs     | Be vague about expectations |
| Version control your SOPs             | Make undocumented changes   |

### For Multi-Session Projects

| Do                                 | Don't                            |
| ---------------------------------- | -------------------------------- |
| Start with "read the session file" | Assume context carries over      |
| Record session logs                | Lose track of what was done      |
| Note deviations and why            | Silently ignore SOP requirements |
| Checkpoint after each phase        | Do too much in one session       |

---

## 🔧 Customizing SOPs for AI

### Adding AI-Friendly Metadata

Consider adding a front-matter block to SOPs:

```markdown
---
sop_id: "SOP-101"
phase: 1
title: "Schema Design"
prerequisites:
  - sop: "SOP-100"
    output: "/docs/tech-stack.md"
outputs:
  - "/docs/database/erd.md"
  - "/migrations/001_initial.sql"
estimated_tokens: 2000
---
```

### Structuring Procedures for AI

Write procedures as numbered steps with clear actions:

```markdown
## Procedure

### 1. Read Prerequisites

- Read `/docs/requirements.md` for data entities
- Read `/docs/tech-stack.md` for database choice

### 2. Identify Entities

- List all nouns from requirements that need persistence
- Group related entities

### 3. Design Tables

For each entity:

- Create table with snake_case name (plural)
- Add required columns: id, created_at, updated_at
- Define relationships (foreign keys)

### 4. Create Outputs

- Write ERD in Mermaid format to `/docs/database/erd.md`
- Generate SQL migration to `/migrations/001_initial.sql`
```

---

## 🧪 Testing AI Adherence

After an AI completes an SOP, verify with:

```markdown
## Verification Task

Check if the outputs from SOP-{XXX} meet requirements:

Read:

- `.sops/phase-X/{SOP-file}.md` (focus on Review Checklist and Outputs)
- {generated files}

For each checklist item, report:

- ✅ Requirement met (with evidence)
- ❌ Requirement not met (with specific gap)

Suggest fixes for any failures.
```

---

## 📚 Quick Reference

### Project Structure

```
{project-root}/
├── .prompts/
│   ├── AI-GUIDE.md           # This guide
│   └── AI-SESSION.md         # Session tracker (AI manages this)
├── .sops/
│   ├── README.md             # SOP index
│   ├── templates/            # Reusable templates
│   ├── phase-0-initialization/
│   ├── phase-1-database/
│   ├── phase-2-backend/
│   ├── phase-3-frontend/
│   ├── phase-4-ai-integration/
│   ├── phase-5-quality/
│   └── phase-6-deployment/
├── docs/                     # Generated documentation
├── src/                      # Source code
└── ...
```

### Session Lifecycle

```
┌─────────────────────────────────────────────────────────────┐
│ 1. SETUP                                                    │
│    Copy .prompts/ and .sops/ to new project                 │
│    User provides project name + description                 │
├─────────────────────────────────────────────────────────────┤
│ 2. INITIALIZE (AI)                                          │
│    AI fills in AI-SESSION.md from user's description        │
│    AI begins SOP-000                                        │
├─────────────────────────────────────────────────────────────┤
│ 3. EXECUTE (AI)                                             │
│    For each SOP:                                            │
│    - Read SOP + prerequisites                               │
│    - Follow Procedure                                       │
│    - Create Outputs                                         │
│    - Update AI-SESSION.md                                   │
├─────────────────────────────────────────────────────────────┤
│ 4. RESUME (new session)                                     │
│    AI reads .prompts/AI-SESSION.md                          │
│    Continues from last incomplete SOP                       │
├─────────────────────────────────────────────────────────────┤
│ 5. COMPLETE                                                 │
│    All SOPs marked ✅                                       │
│    Project is ready for production                          │
└─────────────────────────────────────────────────────────────┘
```

### Key Files

| File                     | Purpose                             |
| ------------------------ | ----------------------------------- |
| `.prompts/AI-GUIDE.md`   | This guide                          |
| `.prompts/AI-SESSION.md` | Active session tracker (AI-managed) |
| `.sops/README.md`        | SOP index and overview              |
| `.sops/templates/`       | Reusable project templates          |

### Starting Prompt (Copy This)

```markdown
I'm starting a new project: {NAME}.
{One or two sentences describing what you're building}

Read `.prompts/AI-GUIDE.md` to understand the workflow.
Then initialize `.prompts/AI-SESSION.md` and begin with SOP-000.
```
