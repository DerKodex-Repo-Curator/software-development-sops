# Software Development SOPs

<p align="center">
  <strong>A turnkey SOP framework that turns software delivery into a repeatable, AI-assisted workflow—so teams ship faster with fewer mistakes.</strong>
</p>

<p align="center">
  <a href="#-quick-start">Quick Start</a> •
  <a href="#-whats-included">What's Included</a> •
  <a href="#-using-with-ai-agents">AI Integration</a> •
  <a href="#-contributing">Contributing</a>
</p>

---

## 🎯 Why This Exists

Building software consistently across a team is hard. This framework provides:

- **📋 29 Standard Operating Procedures** covering the entire development lifecycle
- **🤖 AI-Ready Prompts** for agentic coding assistants (Copilot, Cursor, Claude, etc.)
- **✅ Checklists & Templates** to eliminate guesswork
- **🚀 Junior-Friendly** guidance that helps new developers contribute quickly

## 📦 What's Included

```
├── .prompts/                    # AI session management
│   ├── AI-GUIDE.md              # How to use SOPs with AI agents
│   └── AI-SESSION.md            # Session state tracker
│
└── .sops/                       # Standard Operating Procedures
    ├── README.md                # Complete SOP index
    ├── templates/               # Reusable templates
    ├── phase-0-initialization/  # Project setup (7 SOPs)
    ├── phase-1-database/        # Database design (3 SOPs)
    ├── phase-2-api-backend/     # API development (7 SOPs)
    ├── phase-3-frontend/        # Frontend development (7 SOPs)
    ├── phase-4-ai-integration/  # AI/LLM features (4 SOPs)
    ├── phase-5-quality/         # Testing & review (5 SOPs)
    └── phase-6-deployment/      # Deployment & ops (4 SOPs)
```

### SOP Overview

| Phase | Focus | SOPs |
|-------|-------|------|
| 0 | **Initialization** | Requirements, Tech Stack, Repo Setup, Project Structure, Environment, Design Patterns, Code Style |
| 1 | **Database** | Database Selection, Schema Design, Seed Data |
| 2 | **Backend** | API Design, Authentication, Authorization, Error Handling, Validation |
| 3 | **Frontend** | Component Architecture, Styling, API Integration, Form Handling |
| 4 | **AI Integration** | Feasibility, LLM Integration, AI Testing, Cost Monitoring |
| 5 | **Quality** | Unit Testing, Integration Testing, Code Review, Documentation |
| 6 | **Deployment** | Environment Config, CI/CD, Monitoring, Maintenance |

## 🚀 Quick Start

### Option 1: Copy to New Project

```bash
# Create your project
mkdir my-awesome-project && cd my-awesome-project

# Copy the framework
cp -r /path/to/this-repo/.prompts .
cp -r /path/to/this-repo/.sops .

# Initialize git
git init
```

### Option 2: Use as Template (Recommended)

1. Click **"Use this template"** on GitHub
2. Create your new repository
3. Clone and start building!

## 🤖 Using with AI Agents

This framework is optimized for AI-assisted development. Each SOP includes a prompt template you can use directly with AI coding assistants.

### Starting a Session

Paste this into your AI assistant:

```markdown
I'm starting a new project: {PROJECT_NAME}.
{Brief description of what you're building}

Read `.prompts/AI-GUIDE.md` to understand the workflow.
Then initialize `.prompts/AI-SESSION.md` and begin with SOP-000.
```

The AI will:
1. Track progress in `AI-SESSION.md`
2. Guide you through each relevant SOP
3. Generate code following the procedures
4. Update session state as you complete phases

### Manual Usage

1. Open [.sops/README.md](.sops/README.md) for the complete SOP index
2. Navigate to the relevant phase folder
3. Follow the procedure step-by-step
4. Use the checklist to verify completion

## 🛠️ Tech Stack Focus

These SOPs are optimized for modern web development:

- **Frontend:** Next.js, React, TypeScript, Tailwind CSS
- **Backend:** Node.js, Prisma, PostgreSQL
- **Testing:** Vitest, Playwright
- **CI/CD:** GitHub Actions
- **Monitoring:** Sentry, structured logging

> 💡 The principles apply to other stacks—adapt the specific tools as needed.

## 📖 SOP Document Structure

Each SOP follows a consistent format:

```markdown
# SOP-XXX: Title

## Purpose           → Why this procedure exists
## Scope             → What it covers (and doesn't)
## Prerequisites     → Required completions before starting
## Procedure         → Step-by-step instructions with code examples
## Review Checklist  → Verification items
## AI Agent Prompt   → Ready-to-use prompt template
## Outputs           → Expected deliverables
## Related SOPs      → Connected procedures
```

## 🤝 Contributing

We welcome contributions! See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

### Quick Contribution Ideas

- 🐛 Found an unclear procedure? Open an issue
- 💡 Have a better approach? Submit a PR
- 📝 Missing a use case? Propose a new SOP
- 🌍 Want to adapt for another stack? Fork and customize

## 📄 License

This project is licensed under the MIT License - see [LICENSE](LICENSE) for details.

---

<p align="center">
  <strong>Made with ❤️ by Enactus PortSaid - Tech Committee</strong>
</p>

<p align="center">
  <em>"Consistency is the foundation of mastery."</em>
</p>
