# TIER 3: ANALYSIS

**Agents:** project-status-reporter + schedule-planner (parallel)  
**Purpose:** Generate status reports and project schedules

---

## TIER 3A: PROJECT STATUS REPORTING

### Agent Delegation
```
DELEGATE to project-status-reporter agent (MUST BE USED for status report generation):

Task: Generate project status reports based on {Month} meeting notes and action items

Requirements:
1. Identify active projects
2. Generate weekly status updates (Long Email with Bullets format)
3. Create monthly summary report for {Month}
4. Include health indicators (🟢🟡🔴)
5. Save to Status/weekly-updates/ and Status/monthly-reports/

Success Criteria:
- One status per project
- Monthly summary complete
- Health indicators accurate
- Stakeholder-ready

Estimated time: 20-25 minutes
```

---

## TIER 3B: SCHEDULE PLANNING

### Agent Delegation (parallel with 3A)
```
DELEGATE to schedule-planner agent (MUST BE USED for schedule creation/updates):

Task: Create or update project schedules based on {Month} activities

Requirements:
1. Identify scheduling needs
2. Create schedules (Agile or Waterfall)
3. Update existing schedules
4. Map dependencies
5. Save to Projects/{Project}/schedule.md

Success Criteria:
- All projects have schedules
- Dependencies mapped
- Estimates include buffer

Estimated time: 15-20 minutes
```

---

## Interactive Mode Pause
```
╔══════════════════════════════════════╗
║   TIER 3 COMPLETE                    ║
╚══════════════════════════════════════╝

📊 Summary:
  • Status reports: {X}
  • Schedules created/updated: {Y}

Options:
  [C] Continue to Tier 4 (optional)
  [F] Finish workflow now
  [A] Switch to Autonomous mode
  [X] Abort workflow

Your choice:
```
