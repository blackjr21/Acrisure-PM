# Claritev PM

Project management repository for tracking programs, projects, meetings, and todos.

---

## Repository Structure

```
Claritev/
├── MASTER-TODOS.md              # Central list of all work across programs/projects
├── Background-Context/          # Project context and definitions
├── Programs/                    # Programs with nested projects
│   └── [Program-Name]/
│       ├── program-overview.md
│       ├── todos.md
│       ├── Projects/
│       ├── Meetings/
│       └── Status/
├── Projects/                    # Standalone projects
│   └── [Project-Name]/
│       ├── plan.md
│       ├── status.md
│       ├── todos.md
│       └── Meetings/
├── Meetings/                    # All meetings organized by type
│   ├── 1-on-1/
│   ├── Team/
│   └── General/
├── Status/                      # Status reports and updates
│   ├── weekly-updates/
│   └── monthly-reports/
└── Resources/                   # Templates and references
    ├── templates/
    └── references/
```

---

## Quick Start

### Creating a New Project
1. Navigate to `Projects/` (standalone) or `Programs/[Program-Name]/Projects/` (program project)
2. Create a new folder: `[Project-Name]/`
3. Copy templates from `Resources/templates/`:
   - `plan-template.md` → `plan.md`
   - `status-template.md` → `status.md`
   - `todos-template.md` → `todos.md`

### Adding a Meeting
1. Navigate to appropriate meeting folder:
   - `Meetings/1-on-1/` for 1-on-1 meetings
   - `Meetings/Team/` for team meetings
   - `Meetings/General/` for other meetings
2. Create folder: `YYYY-MM-DD-Topic/`
3. Add `transcription.md` and `notes.md`

### Tracking Work
- **Project-level tasks**: Update `todos.md` in the project folder
- **Program-level tasks**: Update `todos.md` in the program folder
- **All tasks**: Roll up to `MASTER-TODOS.md` for central tracking

---

## Meeting Naming Convention

Format: `YYYY-MM-DD-[Participants/Team]-[Topic]`

Examples:
- `2025-11-17-John-Smith-Performance-Review/`
- `2025-11-18-Engineering-Sprint-Planning/`
- `2025-11-19-Stakeholder-Alignment/`

---

## Todo Format

All todos use structured format with:
- **Due:** Date
- **Priority:** High/Medium/Low
- **Status:** Not Started/In Progress/Blocked/Completed
- **Notes:** Additional context

See `Resources/templates/todos-template.md` for full template.

---

## Resources

Templates and guides available in `Resources/` folder.
