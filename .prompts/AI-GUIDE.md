# AI Agent Guide for SOP-Driven Development

> **This document explains how to use the SOP framework with AI coding agents.**
>
> The SOPs are designed as a **single source of truth** for both human developers and AI agents, enabling consistent, efficient, and auditable software development.

---

## AI Agent Responsibilities

The AI agent is responsible for **managing the entire session lifecycle**:

| Responsibility | When | What to Update |
|----------------|------|----------------|
| **Initialize session** | First prompt | Fill Project Overview, Goals from user description |
| **Track progress** | After each SOP | Update Progress Tracker status and outputs |
| **Maintain context** | After each SOP | Update Current Session, Session Prompt Template |
| **Log sessions** | End of session | Add entry to Session Log |
| **Resume context** | New session | Read AI-SESSION.md, continue from last SOP |

**The human only provides:**
- Initial project name and description
- Answers to clarifying questions
- Approval/feedback on outputs

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

## Understanding the SOP Structure

### Each SOP Contains

```markdown
# SOP-XXX: Title

## Purpose           ← Why this SOP exists
## Scope             ← What it covers / doesn't cover
## Prerequisites     ← What must be done first (other SOPs, tools)
## Procedure         ← Step-by-step instructions (AI follows this)
## Review Checklist  ← Verification items
## AI Agent Prompt   ← (Optional) Pre-written prompt template
## Outputs           ← What files/artifacts this SOP produces
## Related SOPs      ← Cross-references
```

### The AI Agent Should

1. **Read the full SOP** before starting
2. **Check prerequisites** are satisfied (read output files from previous SOPs)
3. **Follow the Procedure** section step-by-step
4. **Produce all Outputs** listed
5. **Verify against the Review Checklist**

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

| Do | Don't |
|----|-------|
| Reference SOP files by path | Copy-paste SOP content into prompts |
| Let AI read prerequisite outputs | Summarize previous work yourself |
| Use the session tracker | Rely on chat memory across sessions |
| Verify outputs match SOP specs | Assume AI followed everything |
| Update tracker after each SOP | Forget to record progress |

### For SOP Maintenance

| Do | Don't |
|----|-------|
| Keep Outputs section accurate | Let outputs become outdated |
| Update Prerequisites when adding SOPs | Create orphan SOPs |
| Include concrete examples in SOPs | Be vague about expectations |
| Version control your SOPs | Make undocumented changes |

### For Multi-Session Projects

| Do | Don't |
|----|-------|
| Start with "read the session file" | Assume context carries over |
| Record session logs | Lose track of what was done |
| Note deviations and why | Silently ignore SOP requirements |
| Checkpoint after each phase | Do too much in one session |

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

| File | Purpose |
|------|---------|
| `.prompts/AI-GUIDE.md` | This guide |
| `.prompts/AI-SESSION.md` | Active session tracker (AI-managed) |
| `.sops/README.md` | SOP index and overview |
| `.sops/templates/` | Reusable project templates |

### Starting Prompt (Copy This)

```markdown
I'm starting a new project: {NAME}.
{One or two sentences describing what you're building}

Read `.prompts/AI-GUIDE.md` to understand the workflow.
Then initialize `.prompts/AI-SESSION.md` and begin with SOP-000.
```
