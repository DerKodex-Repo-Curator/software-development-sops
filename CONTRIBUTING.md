# Contributing to Software Development SOPs

Thank you for your interest in contributing! This document provides guidelines for contributing to our SOP framework.

## 📋 Table of Contents

- [Code of Conduct](#code-of-conduct)
- [How Can I Contribute?](#how-can-i-contribute)
- [SOP Guidelines](#sop-guidelines)
- [Pull Request Process](#pull-request-process)
- [Style Guide](#style-guide)

---

## Code of Conduct

This project follows the [Enactus values](https://enactus.org/) of integrity, collaboration, and positive impact. Please:

- Be respectful and inclusive
- Provide constructive feedback
- Focus on what's best for the team and community
- Welcome newcomers and help them learn

## How Can I Contribute?

### 🐛 Reporting Issues

If you find a problem with an SOP:

1. Check if the issue already exists
2. Open a new issue with:
   - Which SOP is affected (e.g., SOP-201)
   - What's unclear or incorrect
   - Your suggested improvement (if any)

### 💡 Suggesting Enhancements

Have an idea for improvement?

1. Open an issue labeled `enhancement`
2. Describe the use case
3. Explain how it benefits the team

### 📝 Adding or Updating SOPs

1. Fork the repository
2. Create a feature branch (`git checkout -b sop/add-new-procedure`)
3. Make your changes following the [SOP Guidelines](#sop-guidelines)
4. Submit a Pull Request

## SOP Guidelines

### Creating a New SOP

1. **Determine the phase** (0-6) where the SOP belongs
2. **Use the next sequential number** in that phase
3. **Follow the standard structure:**

```markdown
# SOP-XXX: Title

## Purpose
[One paragraph explaining why this SOP exists]

## Scope
- **Covers:** [What this SOP addresses]
- **Does not cover:** [What's out of scope, with references to other SOPs]

## Prerequisites
- [ ] SOP-XXX (Name) — [why it's needed]
- [ ] [Any tools or access required]

## Procedure

### Step 1: [Action]
[Detailed instructions with code examples]

### Step 2: [Action]
[Continue with steps...]

## Review Checklist
- [ ] [Verification item 1]
- [ ] [Verification item 2]

## AI Agent Prompt Template
\```markdown
Execute SOP-XXX (Name):

**Context:**
- [Relevant context]

**Requirements:**
1. [Requirement 1]
2. [Requirement 2]

**Deliverables:**
- [ ] [Output 1]
- [ ] [Output 2]
\```

## Outputs
- [File or artifact 1]
- [File or artifact 2]

## Related SOPs
- **SOP-XXX:** [Name] ([relationship])
```

### Updating Existing SOPs

1. **Preserve backward compatibility** when possible
2. **Update cross-references** if SOP numbers change
3. **Update the README index** if titles change
4. **Test with an AI agent** to ensure prompts still work

## Pull Request Process

### Before Submitting

- [ ] SOP follows the standard structure
- [ ] Cross-references are correct
- [ ] README.md index is updated (if needed)
- [ ] No spelling/grammar errors
- [ ] Code examples are tested and work

### PR Template

```markdown
## Summary
[Brief description of changes]

## Type of Change
- [ ] New SOP
- [ ] SOP update/improvement
- [ ] Bug fix (incorrect information)
- [ ] Template update
- [ ] Documentation improvement

## SOPs Affected
- SOP-XXX: [Brief description of change]

## Checklist
- [ ] I followed the SOP structure guidelines
- [ ] I updated cross-references where needed
- [ ] I tested any code examples
```

### Review Process

1. A maintainer will review within 3 business days
2. Address any feedback
3. Once approved, a maintainer will merge

## Style Guide

### Markdown

- Use ATX-style headers (`#`, `##`, `###`)
- Use fenced code blocks with language identifiers
- Use reference-style links for repeated URLs
- Keep lines under 100 characters when possible

### Code Examples

- **TypeScript/JavaScript:** Follow the style in SOP-006
- **Include imports** when they're not obvious
- **Add comments** explaining non-obvious logic
- **Use realistic variable names** (not `foo`, `bar`)

### Language

- Use **active voice** ("Create a file" not "A file should be created")
- Be **concise** but complete
- Use **second person** ("You should..." or imperative "Create...")
- Avoid jargon without explanation

---

## Questions?

If you're unsure about anything, open an issue with the `question` label. We're happy to help!

---

Thank you for helping improve our development practices! 🙌
