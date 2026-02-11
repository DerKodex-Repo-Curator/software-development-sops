# SOP-302: UI/UX Design & Planning

## Purpose

Plan and design the user interface before implementation. This SOP bridges requirements (user stories) and code (components/pages), ensuring the UI is intentional, user-centered, and systematically designed. The AI adapts its approach based on the developer's design experience and input level.

---

## Scope

- **Applies to:** All user-facing features and pages
- **Covers:** Wireframes, user flows, component planning, design tokens
- **Does not cover:** Visual design tools (Figma), component implementation (SOP-300), page implementation (SOP-305)

---

## Prerequisites

- [ ] SOP-000 (Requirements) — User stories defined
- [ ] SOP-300 (Component Architecture) — Component patterns understood
- [ ] SOP-301 (Styling Standards) — Design tokens/theme defined

---

## Procedure

### 1. Assess Developer Design Input Level

The AI adapts based on developer preference:

| Input Level | Developer Provides | AI Generates |
|-------------|-------------------|--------------|
| **Minimal** | User stories only | Full wireframes, flows, component breakdown |
| **Moderate** | Rough sketches or descriptions | Refined wireframes, component suggestions |
| **Detailed** | Figma designs or detailed specs | Implementation plan, code structure |

**AI Prompt to Developer:**
> "For this feature, would you like me to:
> A) Design the UI from scratch based on requirements
> B) Refine rough ideas you describe
> C) Create an implementation plan from your existing designs
> Select A, B, or C (or describe your preference):"

### 2. Analyze User Stories for UI Implications

Create `/docs/frontend/ui-analysis.md`:

```markdown
# UI Analysis

## Feature: [Feature Name]

### User Stories → UI Requirements

| Story | UI Implications | Components Needed | Interactions |
|-------|-----------------|-------------------|--------------|
| US-XXX: [User can perform action] | [UI elements needed] | [Component1], [Component2] | [User interaction flow] |
| US-XXX: [User can view data] | [Display requirements] | [DataDisplay], [Container] | [Load → Display → Interact] |
| US-XXX: [User can modify data] | [Edit interface needed] | [Form], [Input], [Button] | [Click → Edit → Submit → Feedback] |

### Derived UI Requirements
- [Requirement 1 based on user stories]
- [Requirement 2 based on user stories]
- [Accessibility consideration]
- [Responsive behavior needed]
```

### 3. Create User Flows

Document key user journeys:

```markdown
## User Flows

### Flow 1: [Primary Action]

┌─────────────┐     ┌──────────────┐     ┌─────────────┐     ┌──────────────┐
│  [Step 1]   │ ──► │   [Step 2]   │ ──► │  [Step 3]   │ ──► │   [Step 4]   │
└─────────────┘     └──────────────┘     └─────────────┘     └──────────────┘
                                                                     │
                                                                     ▼
                                              ┌──────────────────────────────┐
                                              │  [Success state with:        │
                                              │  - Feedback element 1]       │
                                              │  - Feedback element 2]       │
                                              └──────────────────────────────┘

### Flow 2: [Secondary Action]

┌─────────────┐     ┌──────────────┐     ┌─────────────────┐
│  [Step 1]   │ ──► │   [Step 2]   │ ──► │  [Modal/Dialog] │
└─────────────┘     └──────────────┘     └─────────────────┘
                                                  │
                    ┌─────────────────────────────┴───────────────────────────┐
                    │                                                         │
                    ▼                                                         ▼
          ┌─────────────────┐                                      ┌───────────────────┐
          │  [Option A]     │                                      │  [Option B]       │
          │  → [Result A]   │                                      │  → [Result B]     │
          └─────────────────┘                                      └───────────────────┘
```

### 4. Design Wireframes (ASCII/Text-Based)

For AI-driven design, use text-based wireframes:

```markdown
## Wireframes

### Screen: [Main View] (Mobile)

┌───────────────────────────────────────┐
│ ◄ Back    [Title]                 ⋮   │  ← Header with actions
├───────────────────────────────────────┤
│ ┌───────────────────────────────────┐ │
│ │ 🔍 [Search/Input]             [+] │ │  ← Primary input
│ └───────────────────────────────────┘ │
├───────────────────────────────────────┤
│                                       │
│ 📁 [SECTION 1] (N)              [▼]   │  ← Section header
│ ┌───────────────────────────────────┐ │
│ │ ○ [Item 1]                 [meta] │ │  ← List item
│ │ ○ [Item 2]                 [meta] │ │
│ │ ○ [Item 3]                 [meta] │ │
│ └───────────────────────────────────┘ │
│                                       │
│ 📁 [SECTION 2] (N)              [▼]   │
│ ┌───────────────────────────────────┐ │
│ │ ○ [Item 1]                 [meta] │ │
│ │ ○ [Item 2]                 [meta] │ │
│ └───────────────────────────────────┘ │
│                                       │
├───────────────────────────────────────┤
│    [🏠 Tab1]  [📊 Tab2]  [⚙️ Tab3]    │  ← Navigation
└───────────────────────────────────────┘

### Screen: [Modal/Dialog]

┌────────────────────────────────────┐
│          [Modal Title]             │
├────────────────────────────────────┤
│                                    │
│  [Label]:                          │
│  ┌──────────────────────────────┐  │
│  │ [Input field]                │  │
│  └──────────────────────────────┘  │
│                                    │
│  [ Primary Action ]                │
│                                    │
│  ─────── OR ───────                │
│                                    │
│  [ Alternative Action ]            │
│                                    │
├────────────────────────────────────┤
│              [ Close ]             │
└────────────────────────────────────┘
```

### 5. Component Breakdown

Map wireframes to component hierarchy:

```markdown
## Component Hierarchy

### [Main View] Page

[Page]
├── Header
│   ├── BackButton
│   ├── Title (editable if needed)
│   └── MenuButton → DropdownMenu
│       ├── [Action 1]
│       ├── [Action 2]
│       └── [Action 3]
│
├── [Primary]Input
│   ├── SearchInput / TextInput
│   └── ActionButton
│
├── ContentArea
│   └── [Section] (repeated)
│       ├── SectionHeader
│       │   ├── Icon
│       │   ├── Title
│       │   ├── Count
│       │   └── CollapseToggle
│       └── [ItemCard] (repeated)
│           ├── SelectionControl
│           ├── Content
│           ├── Metadata
│           └── Actions (swipe/hover)
│
└── Navigation
    ├── NavItem ([Tab 1])
    ├── NavItem ([Tab 2])
    └── NavItem ([Tab 3])
```

### 6. Define Interaction Specifications

Document micro-interactions:

```markdown
## Interaction Specifications

### [Primary Action] Animation
- **Trigger:** [User action]
- **Duration:** [X]ms ease-out
- **Effects:**
  1. [Visual change 1]
  2. [Visual change 2]
  3. [Final state]

### [Secondary Action] Animation
- **Trigger:** [User action]
- **Duration:** [X]ms ease-in
- **Effects:**
  1. [Element behavior]
  2. [Feedback indication]

### [Gesture] Interaction
- **Trigger:** [Gesture type on element]
- **Threshold:** [X]px to reveal, [Y]px to confirm
- **Effects:**
  1. [Progressive reveal]
  2. [Confirmation behavior]
  3. [Undo option if applicable]
```

### 7. Responsive Breakpoints

Define layout changes:

```markdown
## Responsive Design

| Breakpoint | Layout Changes |
|------------|---------------|
| Mobile (<640px) | Single column, bottom nav, touch gestures |
| Tablet (640-1024px) | Two columns, side navigation |
| Desktop (>1024px) | Multi-pane layout, hover interactions |

### Mobile Specific
- Full-width content
- Bottom navigation
- Touch-optimized controls

### Desktop Specific
- Hover to reveal actions
- Keyboard shortcuts
- Drag-and-drop (if applicable)
```

### 8. Accessibility Considerations

Document a11y requirements:

```markdown
## Accessibility

### Keyboard Navigation
- Tab through interactive elements
- Space/Enter to activate
- Escape to dismiss dialogs
- Arrow keys for navigation within groups

### Screen Reader
- Elements announce: "[type] [name], [state], [context]"
- Live regions for dynamic updates
- Meaningful link/button labels

### Visual
- Minimum contrast: 4.5:1
- Focus indicators: 2px solid outline
- Touch targets: minimum 44x44px

### Motion
- Respect prefers-reduced-motion
- Alternative: instant state changes
```

---

## Review Checklist

- [ ] User stories analyzed for UI implications
- [ ] User flows documented for key journeys
- [ ] Wireframes created (text-based or linked Figma)
- [ ] Component hierarchy defined
- [ ] Interactions specified with timing
- [ ] Responsive breakpoints documented
- [ ] Accessibility requirements listed
- [ ] Developer approved design direction

---

## AI Agent Prompt Template

```markdown
Execute SOP-302 (UI/UX Design):

Read:
- `/docs/requirements.md` for user stories
- `/docs/frontend/components.md` for existing components
- `/docs/frontend/theme.md` for design tokens

**Developer input level:** [Minimal/Moderate/Detailed]

**Tasks (for Minimal input):**
1. Analyze user stories for UI implications
2. Create user flows for key journeys
3. Design text-based wireframes for each screen
4. Break down wireframes into component hierarchy
5. Specify key interactions and animations
6. Document responsive behavior
7. List accessibility requirements

**Output to:** `/docs/frontend/ui-design/[feature-name].md`

**Await human approval before proceeding to implementation.**
```

---

## Outputs

- [ ] `/docs/frontend/ui-analysis.md` — Story-to-UI mapping
- [ ] `/docs/frontend/ui-design/[feature].md` — Wireframes & specs per feature
- [ ] Component hierarchy for each screen
- [ ] Interaction specifications
- [ ] Responsive breakpoint documentation
- [ ] Accessibility requirements

---

## Human Approval Gate

⚠️ **CHECKPOINT:** Before proceeding to SOP-305 (Page Implementation):

```markdown
## Design Review Checklist

Please review the proposed UI design and confirm:

- [ ] Wireframes match your vision for the feature
- [ ] User flows cover all use cases
- [ ] Component breakdown is appropriate
- [ ] Interactions feel right for the app
- [ ] Accessibility requirements are adequate

**Approved:** [ ] Yes  [ ] No (provide feedback)
```

---

## Related SOPs

- **SOP-000:** Requirements (user stories)
- **SOP-300:** Component Architecture (building blocks)
- **SOP-301:** Styling Standards (design tokens)
- **SOP-305:** Page Implementation (uses this design)
