# TIER 2: CONSOLIDATION

**Agents:** todo-tracker + stakeholder-management-copilot (parallel)  
**Purpose:** Extract action items and stakeholder information

---

## TIER 2A: TODO TRACKING

### Agent Delegation
```
DELEGATE to todo-tracker agent (MUST BE USED for action item consolidation):

Task: Scan all meeting notes created in Tier 1 and consolidate action items

Requirements:
1. Scan: Meetings/**/notes.md files (modified today)
2. Extract all [ACTION] items from "Tasks & Assignments" sections
3. Add to MASTER-TODOS.md with 5 required fields
4. Add [TRACKED] markers to source files
5. Organize by due date sections
6. Generate 6-section report

Success Criteria:
- MASTER-TODOS.md updated
- [TRACKED] markers added
- No duplicates
- Source links work

Estimated time: 20-30 minutes
```

---

## TIER 2B: STAKEHOLDER MANAGEMENT

### Agent Delegation (parallel with 2A)
```
DELEGATE to stakeholder-management-copilot agent (MUST BE USED for stakeholder extraction):

Task: Extract stakeholder information from all meeting notes created in Tier 1

Requirements:
1. Analyze meeting notes (modified today)
2. Create/update People/{Name}.md profiles
3. Update People/stakeholder-register.md
4. Assign Influence/Interest levels
5. Document communication preferences

Success Criteria:
- Every stakeholder has profile
- No duplicates
- Stakeholder-register.md current

Estimated time: 15-20 minutes
```

---

## Interactive Mode Pause
```
╔══════════════════════════════════════╗
║   TIER 2 COMPLETE                    ║
╚══════════════════════════════════════╝

📊 Summary - Action Items:
  • Action items tracked: {X}
  • [TRACKED] markers added: {Y}

📊 Summary - Stakeholders:
  • Stakeholders identified: {Z}
  • Profiles created/updated: {A}

Options:
  [C] Continue to Tier 3
  [S] Skip to Tier 4
  [A] Switch to Autonomous mode
  [X] Abort workflow

Your choice:
```
