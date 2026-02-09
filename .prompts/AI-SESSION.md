# AI Development Session

> **AI Agent Responsibility:** This file is managed by the AI agent. The human only provides the initial project description—the AI fills in and updates everything else.

---

## 🤖 AI Agent Instructions

**On first session:**
1. Fill in the Project Overview table (name, description, root, date)
2. Document the Project Goals from the user's description
3. Begin with SOP-000 and update the tracker as you progress

**On each SOP completion:**
1. Update the SOP's status in the Progress Tracker (⬚ → ✅)
2. Record the actual output locations
3. Add any notes about deviations or decisions
4. Update the "Current Session" section for the next SOP
5. Update the "Session Prompt Template" with current state
6. Add an entry to the Session Log

**On session resume:**
1. Read this file to recover context
2. Continue from the next incomplete SOP
3. Update all sections as you progress

---

## 📋 Project Overview

| Field | Value |
|-------|-------|
| **Project Name** | {PROJECT_NAME} |
| **Description** | {One-line description of what you're building} |
| **Project Root** | `{/absolute/path/to/project}` |
| **Started** | {YYYY-MM-DD} |

---

## Project Goals

<!-- Briefly describe the core functionality (2-5 bullet points) -->

- {Goal 1}
- {Goal 2}
- {Goal 3}

---

## SOP Progress Tracker

### Phase 0: Initialization

| SOP | Title | Status | Output Location | Notes |
|-----|-------|--------|-----------------|-------|
| 000 | Requirements Gathering | ⬚ | `/docs/requirements.md` | |
| 001 | Tech Stack Selection | ⬚ | `/docs/tech-stack.md` | |
| 002 | Repository Setup | ⬚ | `README.md`, `.gitignore` | |
| 003 | Project Structure | ⬚ | Folder structure | |
| 004 | Environment Setup | ⬚ | `.env.example`, `/docs/setup.md` | |
| 005 | Design Patterns | ⬚ | `/docs/architecture/design-patterns.md` | |
| 006 | Code Style Standards | ⬚ | Linter/formatter configs | |

### Phase 1: Database

| SOP | Title | Status | Output Location | Notes |
|-----|-------|--------|-----------------|-------|
| 100 | Database Selection | ⬚ | `/docs/tech-stack.md` | |
| 101 | Schema Design | ⬚ | `/docs/database/erd.md`, migrations | |
| 103 | Seed Data | ⬚ | `/seeds/` or `/fixtures/` | |

### Phase 2: Backend

| SOP | Title | Status | Output Location | Notes |
|-----|-------|--------|-----------------|-------|
| 200 | API Design | ⬚ | `/docs/api/openapi.yaml` | |
| 201 | Authentication | ⬚ | Auth module/routes | |
| 202 | Authorization | ⬚ | `/docs/authorization.md`, middleware | |
| 203 | Error Handling | ⬚ | Error handler module | |
| 204 | Validation | ⬚ | Validation schemas | |

### Phase 3: Frontend

| SOP | Title | Status | Output Location | Notes |
|-----|-------|--------|-----------------|-------|
| 300 | Component Architecture | ⬚ | `/src/components/` structure | |
| 301 | Styling Standards | ⬚ | Style configs, design tokens | |
| 302 | API Integration | ⬚ | API client module | |
| 303 | Form Handling | ⬚ | Form components/hooks | |

### Phase 4: AI Integration (If Applicable)

| SOP | Title | Status | Output Location | Notes |
|-----|-------|--------|-----------------|-------|
| 400 | AI Feasibility | ⬚ | `/docs/ai-feasibility.md` | |
| 401 | LLM Integration | ⬚ | AI service module | |
| 404 | AI Testing | ⬚ | AI test suite | |
| 405 | Cost Monitoring | ⬚ | Cost tracking setup | |

### Phase 5: Quality

| SOP | Title | Status | Output Location | Notes |
|-----|-------|--------|-----------------|-------|
| 500 | Unit Testing | ⬚ | Test configuration, `/tests/unit/` | |
| 501 | Integration Testing | ⬚ | `/tests/integration/` | |
| 503 | Code Review | ⬚ | PR template, review checklist | |
| 504 | Security Audit | ⬚ | Security scan configs | |

### Phase 6: Deployment

| SOP | Title | Status | Output Location | Notes |
|-----|-------|--------|-----------------|-------|
| 602 | Container Standards | ⬚ | `Dockerfile`, `docker-compose.yml` | |
| 603 | Deployment Strategy | ⬚ | `/docs/deployment.md` | |
| 604 | Monitoring & Alerting | ⬚ | Monitoring configs | |
| 605 | Incident Response | ⬚ | `/docs/incident-response.md` | |

**Status Legend:**
- ⬚ Not Started
- 🔄 In Progress  
- ✅ Complete
- ⏭️ Skipped (not applicable)

---

## 🔄 Current Session

### Active SOP

**SOP:** {Current SOP number, e.g., SOP-000}  
**Title:** {Current SOP title}  
**Status:** 🔄 In Progress

### Context Files to Read

```
.sops/phase-0-initialization/SOP-000-requirements-gathering.md    # The procedure
# No prerequisites for SOP-000 (it's the first SOP)
```

### Expected Outputs

- [ ] `/docs/requirements.md`

---

## 📝 Session Prompt Template

```markdown
## Context

I'm working on {PROJECT_NAME}: {one-line description}.

**Project location:** This workspace root
**Session tracker:** `.prompts/AI-SESSION.md`
**SOPs location:** `.sops/`

## Completed Work

The following SOPs have been completed {List completed SOPs and their outputs, e.g.:}
- SOP-000: Requirements → `/docs/requirements.md`
- SOP-001: Tech Stack → `/docs/tech-stack.md`

## Current Task

Execute **SOP-{XXX}** ({Title}).

**Read these files:**
1. `.sops/phase-X-{phase}/SOP-XXX-{name}.md` — The procedure
2. {List prerequisite output files}

**Refer to `AI-GUIDE.md` to attend to your responsibilities and for guidance on best practices.**
**Follow the SOP's Procedure section step by step.**
**Create all outputs listed in the SOP's Outputs section.**
**Update `.prompts/AI-SESSION.md` when complete.**
```

---

## 📓 Session Log

### Session 1 — {YYYY-MM-DD}

**SOPs Completed:** {e.g., SOP-000, SOP-001}  
**Files Created:**
- {List files created}
**Notes:** {Any observations, decisions, or deviations}

---

## 🔗 Quick Reference

### Directory Structure

```
{project-root}/
├── .prompts/
│   ├── AI-GUIDE.md       # How to use SOPs with AI agents
│   └── AI-SESSION.md     # This file (active session tracker)
├── .sops/
│   ├── README.md         # SOP index
│   ├── templates/        # Reusable templates
│   ├── phase-0-initialization/
│   ├── phase-1-database/
│   ├── phase-2-backend/
│   ├── phase-3-frontend/
│   ├── phase-4-ai-integration/
│   ├── phase-5-quality/
│   └── phase-6-deployment/
├── docs/                 # Generated documentation
├── src/                  # Source code
└── ...
```

### Recommended SOP Order

```
Phase 0 (Sequential)
────────────────────
000 → 001 → 002 → 003 → 004 → 005 → 006

Phase 1-3 (Can parallelize frontend/backend after DB)
──────────────────────────────────────────────────────
100 → 101 → 103
            ↓
      ┌─────┴─────┐
      ↓           ↓
    Phase 2    Phase 3
    (Backend)  (Frontend)
      ↓           ↓
      └─────┬─────┘
            ↓
    Phase 4 (Optional)
            ↓
    Phase 5 (Quality)
            ↓
    Phase 6 (Deploy)
```

---

## ⚠️ Important Notes

1. **AI manages this file** — Human only provides initial project description
2. **Always read SOPs first** — Don't assume; follow the documented procedure
3. **Check prerequisites** — Ensure previous SOP outputs exist before starting
4. **Update after each SOP** — Keep this tracker current
5. **Document deviations** — Note any departures from SOPs in the session log
