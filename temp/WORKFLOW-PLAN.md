# Comprehensive Workflow Plan - Year of Conversations Processing

**Created:** 2025-11-18
**Purpose:** Process a year's worth of meeting transcripts using AI agent team
**Status:** Ready to Execute

---

## Executive Summary

This plan outlines the optimal workflow for processing ~400 meeting transcripts (12 months) using 6 specialized AI agents. The agents analyzed the current state (38 transcripts in Input/November/) and designed a sequential, quality-focused workflow.

**Key Metrics:**
- **Total Transcripts:** ~400 (36 per month average)
- **Processing Time:** 3-4 hours per month
- **Total Timeline:** 10-12 weeks (at 1 month/week)
- **Quality Target:** >95% accuracy with minimal manual fixes

---

## Table of Contents

1. [Current Situation](#current-situation)
2. [Agent Workflow Sequence](#agent-workflow-sequence)
3. [Recommended Processing Approach](#recommended-processing-approach)
4. [Step-by-Step Workflow](#step-by-step-workflow)
5. [Batch Size Recommendations](#batch-size-recommendations)
6. [Quality Controls](#quality-controls)
7. [Time Estimates](#time-estimates)
8. [What Each Agent Produces](#what-each-agent-produces)
9. [Next Steps](#next-steps)

---

## Current Situation

### What We Have

**Input/November/ Directory:**
- **38 meeting transcripts** (.txt format from Otter.ai)
- **File size:** Average 17.5KB per file (~700 lines)
- **Quality:** Complete transcripts with speaker labels
- **Date range:** November 2024

**Meeting Type Distribution:**
- Technical/Architecture: 34% (13 files)
- Daily Standups: 26% (10 files)
- Team Coordination: 16% (6 files)
- PMO/Status: 11% (4 files)
- Backlog Grooming: 8% (3 files)
- 1-on-1s: 5% (2 files)

**Key Projects Identified:**
- MuleSoft-Salesforce Integration
- Copado DevOps Setup
- Data Migration projects
- Policy field mapping work
- Service Center transition

**Key Stakeholders Identified:**
- Calvin Williams Jr. (Digital Program Manager)
- Jody/Jodie (Leadership)
- Alan (Technical Lead)
- Ella, Anna, Adriana (Team members)
- Nitin, Ramya, Ashish (Developers)

### Repository Current State

**Populated:**
- ✅ Templates in Resources/
- ✅ Agent prompts in .claude/agents/
- ✅ Folder structure (Meetings/, People/, Projects/, Programs/)

**Empty (Awaiting Processing):**
- ❌ Meetings/ folders (only README files)
- ❌ MASTER-TODOS.md (template only)
- ❌ People/ profiles (stakeholder-register.md is template)
- ❌ Projects/ and Programs/ folders

---

## Agent Workflow Sequence

### Dependency Map

```
RAW TRANSCRIPTS (.txt)
         ↓
    [TIER 1] - Foundation
  ai-pm-meeting-notes
         ↓
         +------------------+
         ↓                  ↓
    [TIER 2A]          [TIER 2B]
  todo-tracker    stakeholder-copilot
         ↓                  ↓
         +--------+---------+
                  ↓
         +--------+---------+
         ↓                  ↓
    [TIER 3A]          [TIER 3B]
project-status      schedule-planner
    reporter
         ↓                  ↓
         +--------+---------+
                  ↓
            [TIER 4]
    meeting-agenda-preparer
```

### Agent Execution Order

#### TIER 1: Foundation (Must run FIRST)
**Agent:** `ai-pm-meeting-notes`

**What it does:**
- Transforms raw transcripts into structured meeting notes
- Extracts attendees from conversation (not "Speaker 1/2")
- Creates 4-section format: TLDR, Tasks & Assignments, Risks, Notes
- Classifies meetings into 1-on-1, Team, or General

**Input:** Raw .txt files from Input/[Month]/
**Output:** `Meetings/[Type]/YYYY-MM-DD-Topic/notes.md`
**Time:** 60-90 minutes per month
**Dependencies:** None

**Example Output:**
```markdown
# Meeting Notes: MuleSoft Integration Planning

**Date:** November 15, 2024
**Time:** 2:00-3:00 PM PT
**Attendees:**
- Calvin Williams Jr. (Program Manager)
- Alan (Technical Lead)
- Anna (MuleSoft Developer)

---

## TLDR
- [DECISION] Approved MuleSoft architecture for Phase 1
- [RISK] API rate limits may impact automation frequency
- [VALUE] Expected 30% reduction in manual data entry
- [DEPENDENCY] Requires Copado setup completion by Nov 22

## Tasks & Assignments
- [ACTION] Complete API documentation review – Owner: Calvin | Due: 2025-11-20 | Status: Not started
- [ACTION] Set up development environment – Owner: Anna | Due: 2025-11-22 | Status: In progress

## Risks
- [RISK] API rate limits may delay automation testing
  - Mitigation: Exploring batch processing approach
- [DEPENDENCY] Copado deployment must complete before integration testing

## Notes
[Detailed context organized by subsections...]
```

---

#### TIER 2: Consolidation (Run SECOND - Can parallelize)

**TIER 2A: todo-tracker**

**What it does:**
- Scans all meeting notes for action items
- Consolidates to MASTER-TODOS.md
- Adds [TRACKED] markers to prevent duplicates
- Organizes by due date (This Week, Next Week, Later, Backlog)

**Input:** Structured notes from Tier 1
**Output:**
- Updated MASTER-TODOS.md
- [TRACKED] markers in source files
- 6-section processing report

**Time:** 20-30 minutes
**Dependencies:** Requires ai-pm-meeting-notes output

**Example Output (MASTER-TODOS.md):**
```markdown
## Due This Week

### [MuleSoft-Integration] Complete API documentation review
- **Due:** 2025-11-20
- **Priority:** High
- **Status:** Not Started
- **Notes:** Required for Phase 1 architecture approval
- **Source:** [Meetings/General/2025-11-15-MuleSoft-Planning.md](Meetings/General/2025-11-15-MuleSoft-Planning.md)
- **Created:** 2025-11-18
```

---

**TIER 2B: stakeholder-management-copilot**

**What it does:**
- Extracts stakeholder mentions from meetings
- Creates/updates People/ profiles
- Updates stakeholder-register.md
- Builds RACI matrices for projects

**Input:** Structured notes from Tier 1
**Output:**
- People/[Name].md profiles
- Updated stakeholder-register.md
- RACI matrices (if applicable)

**Time:** 15-20 minutes
**Dependencies:** Requires ai-pm-meeting-notes output
**Can run in parallel with:** todo-tracker

**Example Output (People/Alan.md):**
```markdown
# Alan - Technical Lead

## Basic Information
- **Role:** Technical Lead / Architect
- **Team:** Engineering
- **Influence:** High
- **Interest:** High
- **Stance:** Supportive

## Key Priorities
- MuleSoft architecture design
- Copado DevOps setup
- Technical team mentoring

## Communication Preferences
- **Preferred:** Technical deep-dives, detailed documentation
- **Response Time:** Same day
- **Meeting Style:** Data-driven, prefers diagrams

## Meeting History
- 2025-11-15: MuleSoft Planning - Approved architecture
- 2025-11-18: Technical Review - Field mapping decisions
```

---

#### TIER 3: Analysis (Run THIRD - Can parallelize)

**TIER 3A: project-status-reporter**

**What it does:**
- Generates weekly/monthly status reports
- Analyzes project health (Green/Yellow/Red)
- Identifies accomplishments, risks, blockers
- Creates executive summaries

**Input:** Meeting notes, MASTER-TODOS.md, project files
**Output:** `Status/weekly-updates/` and `Status/monthly-reports/`
**Time:** 20-25 minutes
**Dependencies:** Needs Tier 1 & 2 outputs
**Can run in parallel with:** schedule-planner

**Example Output:**
```markdown
# MuleSoft Integration - Weekly Update

**Week Ending:** 2025-11-22
**Status:** 🟢 Green

## Overview
Phase 1 architecture approved. Development environment setup in progress.

## Accomplishments This Week
- Completed architecture design and approval
- Started development environment configuration
- Documented API integration patterns

## Planned Work Next Week
- Complete dev environment setup
- Begin API integration testing
- Conduct team training on MuleSoft basics

## Risks
- 🟡 MEDIUM: API rate limits may impact testing schedule
  - Mitigation: Implementing batch processing approach
```

---

**TIER 3B: schedule-planner**

**What it does:**
- Creates project schedules and timelines
- Identifies dependencies and critical path
- Generates Agile or Waterfall schedules based on context
- Updates existing schedules based on meeting outcomes

**Input:** Meeting notes, MASTER-TODOS.md, project context
**Output:** `Projects/[Project]/schedule.md`
**Time:** 15-20 minutes
**Dependencies:** Needs Tier 1 & 2 outputs
**Can run in parallel with:** project-status-reporter

**Example Output:**
```markdown
# MuleSoft Integration - Agile Schedule

## Project Overview
- **Methodology:** Agile (2-week sprints)
- **Duration:** 8 weeks (4 sprints)
- **Team:** 1 PM, 2 Engineers
- **Start:** 2025-11-18
- **Target:** 2025-01-13

## Sprint 1 (Nov 18-29) - Foundation
**Goal:** Architecture and environment setup

**Stories (21 points):**
- [ ] 8pts: Design integration architecture
- [ ] 8pts: Set up development environment
- [ ] 5pts: Document API patterns

**Dependencies:**
- Copado setup completion (due Nov 22)

## Critical Path
Architecture → Dev Environment → API Integration → Testing → Deploy
```

---

#### TIER 4: Forward Planning (Run LAST - As needed)

**Agent:** `meeting-agenda-preparer`

**What it does:**
- Prepares agendas for upcoming recurring meetings
- Reviews action items from previous meetings
- Identifies meetings ready for [Closed] status
- Provides stakeholder context for meetings

**Input:** All previous tier outputs
**Output:** Meeting agendas for upcoming meetings
**Time:** 15-20 minutes
**Dependencies:** Needs ALL previous tier outputs
**When to run:** As needed for upcoming meetings, or end of month

**Example Output:**
```markdown
# Meeting Agenda: 1-on-1 with Alan

**Date:** 2025-11-25, 2:00 PM PT
**Duration:** 30 minutes

## Action Items from Previous Meeting (2025-11-18)
- ✅ [COMPLETED] Architecture design document
- 🟡 [IN PROGRESS] Dev environment setup - Due: 2025-11-22

## Discussion Topics
1. **API Rate Limit Risk** (15 min)
   - Decision needed on batch processing approach
2. **Team Training Plan** (10 min)
   - Review training materials
3. **Phase 2 Scope** (5 min)
   - Discuss priorities

## Decisions Needed
- [ ] Approve batch processing implementation
- [ ] Sign off on training materials
```

---

## Recommended Processing Approach

### Option 1: Monthly Processing (RECOMMENDED)

**Frequency:** Process 1 month per session
**Time per session:** 3-4 hours
**Schedule:** Every Friday 2:00-6:00 PM (or dedicated block)
**Total timeline:** 12 sessions to complete all months

**Why this is best:**
- ✅ Maintains high quality control
- ✅ Learn and improve between months
- ✅ Prevents agent context overload
- ✅ Easier to spot and fix errors
- ✅ Clear milestones and progress
- ✅ Can refine process based on learnings

**Processing Order:**
1. **Start:** November 2024 (current month - fresh data)
2. **Then:** Work backwards → October → September → August... → January
3. **Why backwards:** Recent context more valuable, action items more relevant

---

### Option 2: Weekly Processing (Alternative)

**Frequency:** Process 1 week's meetings (8-10 transcripts) weekly
**Time per session:** 60-90 minutes
**Total timeline:** ~12 weeks to complete

**When to use:**
- Prefer shorter, more frequent sessions
- Want to stay continuously updated
- Have limited blocks of time available

---

### Option 3: Quarterly Processing (Aggressive)

**Frequency:** Process 3 months (108 transcripts) quarterly
**Time per session:** 10-12 hours
**Total timeline:** 4 quarters

**Trade-offs:**
- ⚡ Faster overall completion
- ⚠️ Higher error risk
- ⚠️ Harder to maintain quality
- ⚠️ May lose temporal granularity

---

### NOT RECOMMENDED: All at Once

**Why to avoid processing all 400 transcripts at once:**
- ❌ Overwhelming (30-40 hours of processing)
- ❌ Error amplification (one mistake × 400)
- ❌ Agent context overload
- ❌ No opportunity to learn and improve mid-way
- ❌ Nearly impossible to validate quality
- ❌ High likelihood of duplicate/conflicting data

---

## Step-by-Step Workflow (Per Month)

### PHASE 1: Preparation (15 minutes)

**Step 1.1: Organize Source Files**
```bash
# Verify files in Input/[Month]/
ls -la "Input/November/"

# Count transcripts
find Input/November -name "*.txt" | wc -l
```

**Step 1.2: Create Processing Workspace**
```bash
mkdir -p "Processing/November-Batch"
mkdir -p "Processing/November-Batch/logs"
```

**Step 1.3: Quality Pre-Check**
- [ ] All .txt files are readable
- [ ] No duplicate filenames
- [ ] Date information extractable
- [ ] No corrupted/incomplete files

**Step 1.4: Take Baseline Backup**
```bash
# Commit current state to git
git add -A
git commit -m "Pre-processing snapshot: November ready"
```

---

### PHASE 2: Tier 1 Execution (60-90 minutes)

**Agent:** ai-pm-meeting-notes

**Step 2.1: Process in Batches of 8-10**

**Batch 1: Transcripts 1-10**
```bash
# For each transcript in batch:
# 1. Run ai-pm-meeting-notes agent
# 2. Input: Raw transcript text
# 3. Output: Structured notes.md

# Quality check after batch:
# - Verify 4-section structure
# - Check attendee names extracted (not "Speaker 1/2")
# - Confirm action items formatted correctly
```

**Repeat for:**
- Batch 2: Transcripts 11-20
- Batch 3: Transcripts 21-28
- Batch 4: Transcripts 29-36

**Step 2.2: Meeting Type Classification**

Determine folder location for each meeting:
- **1-on-1:** Two participants by name → `Meetings/1-on-1/`
- **Team:** 3-8 participants, same team → `Meetings/Team/`
- **General:** 8+ or cross-functional → `Meetings/General/`

**Step 2.3: Quality Checkpoint (per batch)**
- [ ] Metadata header present (Date, Time, Attendees)
- [ ] Actual names extracted (no generic "Speaker 1/2")
- [ ] All 4 sections present (TLDR, Tasks, Risks, Notes)
- [ ] Action items have: Owner | Due | Status
- [ ] No fabricated information (TBD where needed)

**Step 2.4: Create Processing Log**
```markdown
# Tier 1 Processing Log - November

## Batch 1 (Transcripts 1-10)
- Start: 2025-11-18 14:00
- End: 2025-11-18 14:45
- Success: 10/10
- Issues: None

## Batch 2 (Transcripts 11-20)
- Start: 2025-11-18 15:00
- End: 2025-11-18 15:45
- Success: 10/10
- Issues: 1 meeting had unclear attendees, marked TBD
```

**Output:** 36 structured meeting notes in Meetings/[Type]/

---

### PHASE 3: Tier 2 Execution (30-45 minutes)

**Run in Parallel (if using multiple sessions)**

#### 3A: todo-tracker Agent (20-30 min)

**Step 3A.1: Run Repository Scan**
```bash
# The agent will:
# - Scan all new meeting notes
# - Extract action items
# - Add to MASTER-TODOS.md
# - Mark source files with [TRACKED]
```

**Step 3A.2: Review Agent Report**

The agent produces 6-section report:
1. **Scan Results Summary** - Verify counts
2. **Items Added** - Review for accuracy
3. **Source Files Updated** - Confirm [TRACKED]
4. **Items Needing Clarification** - Address TBDs
5. **Project Todos Reviewed** - Note rollups
6. **Recommendations** - Action suggestions

**Step 3A.3: Quality Check**
- [ ] MASTER-TODOS.md has new entries
- [ ] All entries have 5 fields (Due, Priority, Status, Notes, Source)
- [ ] Source links are clickable
- [ ] [TRACKED] markers added to source files
- [ ] No duplicate tasks

---

#### 3B: stakeholder-management-copilot Agent (15-20 min)

**Step 3B.1: Run Stakeholder Extraction**
```bash
# The agent will:
# - Analyze meeting notes
# - Extract stakeholder mentions
# - Create/update People/ profiles
# - Update stakeholder-register.md
```

**Step 3B.2: Review Outputs**
- Check People/ folder for new profiles
- Review stakeholder-register.md updates
- Verify no duplicate profiles created

**Step 3B.3: Quality Check**
- [ ] All mentioned stakeholders have profiles
- [ ] No duplicates
- [ ] Influence/Interest levels assigned
- [ ] Communication strategies documented

---

### PHASE 4: Tier 3 Execution (30-40 minutes)

**Run in Parallel (if using multiple sessions)**

#### 4A: project-status-reporter Agent (20-25 min)

**Step 4A.1: Identify Projects**
Review meetings to identify active projects/programs

**Step 4A.2: Generate Status Reports**
```bash
# For each project:
# - Run agent to create status report
# - Use "Long Email with Bullets" format
# - Output to Status/weekly-updates/
```

**Step 4A.3: Create Month Summary**
```bash
# Create Status/monthly-reports/November-Summary.md
# - Comprehensive rollup of all activity
# - Cross-project dependencies
# - Risks and blockers
```

---

#### 4B: schedule-planner Agent (15-20 min)

**Step 4B.1: Identify Scheduling Needs**
From meetings and todos, identify:
- New projects needing schedules
- Timeline updates required
- Dependency changes

**Step 4B.2: Generate Schedules**
```bash
# For new projects:
# - Create Projects/[Project]/schedule.md
# - Define phases, milestones, dependencies

# For existing:
# - Update timelines based on meetings
# - Adjust for new dependencies
```

---

### PHASE 5: Tier 4 Execution (15-20 min, as needed)

**Agent:** meeting-agenda-preparer

**Step 5.1: Identify Recurring Meetings**
From processed month, identify:
- Weekly syncs continuing
- Monthly check-ins
- Regular 1-on-1s

**Step 5.2: Prepare Agendas**
For each recurring meeting:
- Generate agenda based on history
- Include previous action items
- Add project deliverables due

**Step 5.3: Close Completed Meetings**
Mark meetings as [Closed] when:
- All action items 100% complete
- No carried-forward topics
- Superseded by later meetings

---

### PHASE 6: Validation & Cleanup (20-30 minutes)

**Step 6.1: Cross-Reference Check**
```bash
# Verify consistency:
# - Meeting notes ↔ MASTER-TODOS.md
# - MASTER-TODOS.md ↔ Project todos.md
# - Status reports ↔ Meeting outcomes
# - Stakeholder register ↔ People/ profiles
```

**Step 6.2: Data Quality Audit**
```bash
# Check for unmarked action items
grep -r "\[ACTION\]" Meetings/ | grep -v "\[TRACKED\]" | grep -v "MASTER-TODOS"

# Find TBD items needing attention
grep -r "TBD" MASTER-TODOS.md
```

**Step 6.3: Spot-Check Sample**
Review 10% of outputs:
- 3-4 random meeting notes
- 5-6 MASTER-TODOS entries
- 2-3 stakeholder profiles

**Step 6.4: Create Completion Report**
```markdown
# November Processing Completion Report

## Summary
- Transcripts processed: 36/36
- Meeting notes created: 36
- Action items tracked: 47
- Stakeholders identified: 12
- Projects identified: 4
- Status reports generated: 4

## Quality Metrics
- Notes with all 4 sections: 36/36 (100%)
- Action items with owners: 45/47 (96%)
- Tasks with due dates: 42/47 (89%)
- Transcripts with issues: 1 (attendees unclear)

## Issues & Resolutions
- Meeting "AC Daily Meeting.txt" only 81 bytes - excluded as incomplete

## Recommendations for Next Month
- Consider automating date extraction from file metadata
```

**Step 6.5: Archive & Commit**
```bash
# Move processed files to archive
mv Input/November Archive/

# Clean up temp files
rm -rf Processing/November-Batch/temp/

# Git commit
git add -A
git commit -m "Process November 2024: 36 meetings, 47 action items tracked"
```

---

## Batch Size Recommendations

### Within-Month: 8-10 Transcripts

**For Tier 1 (ai-pm-meeting-notes):**
- ✅ Process in batches of 8-10 transcripts
- ✅ Quality checks between batches
- ✅ Prevents agent context overload
- ✅ Easy to pause/resume

**For Tiers 2-4:**
- ✅ Process full month at once
- ✅ Designed for repository-wide scans
- ✅ More efficient

### Month-to-Month: 1 Month

**RECOMMENDED:**
- ✅ Process 1 month (36 transcripts) per session
- ✅ Maintains quality control
- ✅ Learn between months
- ✅ Clear milestones

**ALTERNATIVE (Time-Constrained):**
- ⚠️ Process quarterly (3 months, ~100 transcripts)
- ⚠️ Trade speed for some quality

**NOT RECOMMENDED:**
- ❌ All 12 months at once (400 transcripts)
- ❌ Quality will suffer significantly

---

## Quality Controls

### Checkpoints

**Checkpoint 1: After Each Tier 1 Batch**
- [ ] Meeting notes have 4-section structure
- [ ] Attendee names extracted (not "Speaker 1/2")
- [ ] Action items properly formatted
- [ ] No fabricated dates/names
- [ ] Files in correct Meetings/ subfolder

**Time:** 5 minutes per batch

---

**Checkpoint 2: After Tier 2**

**For todo-tracker:**
- [ ] MASTER-TODOS.md has new entries
- [ ] All entries have 5 required fields
- [ ] Source links work
- [ ] [TRACKED] markers added
- [ ] Count matches expectations

**For stakeholder-copilot:**
- [ ] People/ folder updated
- [ ] Stakeholder register current
- [ ] No duplicate profiles
- [ ] Influence/Interest assigned

**Time:** 10 minutes

---

**Checkpoint 3: After Tier 3**

**For project-status-reporter:**
- [ ] Status reports created
- [ ] Health indicators accurate
- [ ] Risks have mitigations
- [ ] Consistent with MASTER-TODOS.md

**For schedule-planner:**
- [ ] Schedules created for new projects
- [ ] Dependencies identified
- [ ] Timeline realistic

**Time:** 10 minutes

---

**Checkpoint 4: Final Validation**
- Cross-reference audit
- Data quality audit
- 10% spot-check sample
- Completion report

**Time:** 20-30 minutes

---

### Quality Metrics to Track

**Completion Metrics:**
- Transcripts processed / Total
- Meeting notes created / Transcripts
- Action items tracked / Total found

**Quality Metrics:**
- % notes with all 4 sections (Target: 100%)
- % action items with owners (Target: >90%)
- % tasks with specific dates (Target: >80%)
- % stakeholders with profiles (Target: 100%)

**Error Metrics:**
- Transcripts with errors
- Failed agent runs
- Manual corrections required (Target: <5%)

---

### Automated Validation Scripts

**Create:** `scripts/validate-processing.sh`

```bash
#!/bin/bash
# Validation script for month processing

echo "=== PROCESSING VALIDATION REPORT ==="
echo ""

# Check 1: All transcripts processed
TRANSCRIPT_COUNT=$(find Input/$1 -name "*.txt" 2>/dev/null | wc -l)
NOTES_COUNT=$(find Meetings -name "notes.md" -mtime -1 2>/dev/null | wc -l)
echo "Transcripts in Input/$1: $TRANSCRIPT_COUNT"
echo "Notes created today: $NOTES_COUNT"
echo ""

# Check 2: Unmarked action items
UNMARKED=$(grep -r "\[ACTION\]" Meetings/ 2>/dev/null | grep -v "\[TRACKED\]" | grep -v "MASTER-TODOS" | wc -l)
echo "Unmarked action items: $UNMARKED"
if [ $UNMARKED -gt 0 ]; then
    echo "⚠️  WARNING: Found unmarked action items!"
fi
echo ""

# Check 3: TBD items
TBD_COUNT=$(grep -c "TBD" MASTER-TODOS.md 2>/dev/null || echo "0")
echo "TBD items in MASTER-TODOS: $TBD_COUNT"
echo ""

# Check 4: New profiles
PEOPLE_COUNT=$(find People -name "*.md" -mtime -1 2>/dev/null | wc -l)
echo "New stakeholder profiles: $PEOPLE_COUNT"
echo ""

echo "=== END VALIDATION REPORT ==="
```

**Usage:**
```bash
chmod +x scripts/validate-processing.sh
./scripts/validate-processing.sh November
```

---

## Time Estimates

### Per Month (36 transcripts)

| Phase | Activity | Time | Parallel? |
|-------|----------|------|-----------|
| Phase 1 | Preparation | 15 min | No |
| Phase 2 | Tier 1: ai-pm-meeting-notes (4 batches) | 60-90 min | No |
| | Checkpoints (4 × 5 min) | 20 min | No |
| Phase 3 | Tier 2A: todo-tracker | 20-30 min | Yes |
| | Tier 2B: stakeholder-copilot | 15-20 min | Yes |
| | Checkpoint 2 | 10 min | No |
| Phase 4 | Tier 3A: project-status-reporter | 20-25 min | Yes |
| | Tier 3B: schedule-planner | 15-20 min | Yes |
| | Checkpoint 3 | 10 min | No |
| Phase 5 | Tier 4: meeting-agenda-preparer | 15-20 min | No |
| Phase 6 | Validation & Cleanup | 20-30 min | No |

**TOTAL (with parallelization):** 3 hours 15 min - 4 hours 15 min per month

---

### Full Year (12 months)

**With Parallelization:**
- Best case: 12 × 3.25 hours = **39 hours**
- Worst case: 12 × 4.25 hours = **51 hours**

**Processing Schedule Options:**

**Option 1: Weekly (Recommended)**
- 1 month per week
- 4 hours/week
- **12 weeks total**

**Option 2: Bi-weekly**
- 1 month every 2 weeks
- 4 hours every 2 weeks
- **24 weeks total**

**Option 3: Sprint (Aggressive)**
- 2 weeks dedicated
- 4-5 hours/day
- **Complete in 2 weeks**
- ⚠️ Higher burnout risk

---

## What Each Agent Produces

### 1. ai-pm-meeting-notes

**Produces:**
- Structured meeting notes with:
  - Meeting metadata (Date, Time, Attendees)
  - TLDR (2-6 bullets with semantic tags)
  - Tasks & Assignments (Owner, Due, Status)
  - Risks (with severity and mitigation)
  - Notes (detailed context)

**Location:** `Meetings/[Type]/YYYY-MM-DD-Topic/notes.md`

---

### 2. todo-tracker

**Produces:**
- Updated MASTER-TODOS.md with consolidated action items
- [TRACKED] markers in source meeting files
- 6-section processing report:
  1. Scan Results Summary
  2. Items Added to MASTER-TODOS.md
  3. Source Files Updated
  4. Items Needing Clarification
  5. Project Todos Reviewed
  6. Recommendations

**Location:** `MASTER-TODOS.md` + source files

---

### 3. stakeholder-management-copilot

**Produces:**
- Individual stakeholder profiles
- Updated stakeholder register
- RACI matrices (when applicable)
- Communication plans

**Location:**
- `People/[Name].md`
- `People/stakeholder-register.md`
- `Programs/[Program]/raci-matrix.md`

---

### 4. project-status-reporter

**Produces:**
- Weekly status updates
- Monthly summary reports
- Project health indicators
- Risk and blocker tracking

**Location:**
- `Status/weekly-updates/YYYY-MM-DD-[Project]-Update.md`
- `Status/monthly-reports/YYYY-MM-[Month]-Summary.md`

---

### 5. schedule-planner

**Produces:**
- Project schedules (Agile or Waterfall)
- Sprint plans (for Agile projects)
- Gantt charts (text format for Waterfall)
- Dependency maps
- Critical path identification

**Location:** `Projects/[Project]/schedule.md`

---

### 6. meeting-agenda-preparer

**Produces:**
- Meeting agendas for upcoming meetings
- Action item status from previous meetings
- Stakeholder context
- Discussion topics and decisions needed
- Meeting closure recommendations

**Location:** New meeting folders or clipboard

---

## Next Steps

### Immediate Actions

1. **Review this plan**
   - Understand the tier sequence
   - Familiarize with quality checkpoints
   - Note time estimates

2. **Set up environment**
   ```bash
   # Create necessary directories
   mkdir -p Processing
   mkdir -p scripts
   mkdir -p Archive

   # Create validation script
   # (Copy script from Quality Controls section)
   ```

3. **Schedule first processing session**
   - Block 4 hours on calendar
   - Choose: This Friday 2:00-6:00 PM
   - Month: November 2024

4. **Prepare November files**
   ```bash
   # Verify files
   ls -la Input/November/

   # Count files
   find Input/November -name "*.txt" | wc -l

   # Check first file
   head -20 "Input/November/AC Backlog Grooming.txt"
   ```

### Week 1: Process November

**Day 1 (Friday):**
- Phase 1: Preparation (15 min)
- Phase 2: Tier 1 - Batches 1-2 (90 min)
- Break (15 min)
- Phase 2: Tier 1 - Batches 3-4 (90 min)
- Phase 3: Tier 2 (40 min)
- **Total: ~4 hours**

**Day 2 (Weekend or Monday):**
- Phase 4: Tier 3 (35 min)
- Phase 5: Tier 4 (20 min)
- Phase 6: Validation (30 min)
- **Total: 85 min**

**Or combine into single 4-hour session**

### Week 2: Process October

- Repeat workflow
- Document improvements
- Refine process

### Weeks 3-12: Complete Remaining Months

- September → August → July... → January
- 1 month per week
- Track improvements in processing log

---

## Success Criteria

**You'll know the workflow is successful when:**

1. ✅ **Completeness:** All 12 months processed with zero lost action items
2. ✅ **Quality:** >95% of outputs require no manual correction
3. ✅ **Consistency:** All files follow standard formats
4. ✅ **Traceability:** Every action item links to source meeting
5. ✅ **Utility:** Team actively uses repository for PM work
6. ✅ **Maintainability:** Process repeatable for future months

---

## Key Success Factors

1. **Follow the tier sequence strictly** - Dependencies matter
2. **Don't skip checkpoints** - Catch errors early
3. **Use batches of 8-10** - Quality over speed
4. **Process backwards** - Nov → Oct → Sep... → Jan
5. **Document improvements** - Refine between months
6. **Maintain quality >95%** - Minimal manual fixes
7. **Stay disciplined** - Commit to weekly cadence

---

## Appendix: Processing Checklist

### Pre-Processing Checklist

- [ ] Month selected (Start: November)
- [ ] Files verified in Input/[Month]/
- [ ] Processing workspace created
- [ ] Validation script ready
- [ ] 4 hours blocked on calendar
- [ ] Git backup taken

### Phase 1 Checklist

- [ ] Files organized
- [ ] Quality pre-check complete
- [ ] No corrupted files
- [ ] Processing log initialized

### Phase 2 Checklist (per batch)

- [ ] 8-10 transcripts selected
- [ ] ai-pm-meeting-notes run
- [ ] Notes have 4 sections
- [ ] Attendees extracted (not "Speaker 1/2")
- [ ] Meeting type classified
- [ ] Checkpoint passed
- [ ] Batch logged

### Phase 3 Checklist

- [ ] todo-tracker run
- [ ] MASTER-TODOS.md updated
- [ ] [TRACKED] markers added
- [ ] stakeholder-copilot run
- [ ] People/ profiles created
- [ ] Stakeholder register updated
- [ ] Checkpoint 2 passed

### Phase 4 Checklist

- [ ] project-status-reporter run
- [ ] Status reports generated
- [ ] schedule-planner run
- [ ] Project schedules created
- [ ] Checkpoint 3 passed

### Phase 5 Checklist (as needed)

- [ ] meeting-agenda-preparer run
- [ ] Agendas created
- [ ] Meetings marked [Closed]

### Phase 6 Checklist

- [ ] Cross-reference audit complete
- [ ] Validation script run
- [ ] 10% sample reviewed
- [ ] Completion report created
- [ ] Files archived
- [ ] Git commit made

### Post-Processing Checklist

- [ ] Processing log updated
- [ ] Improvements documented
- [ ] Next month scheduled
- [ ] Stakeholders notified (optional)

---

## Contact & Support

**For questions or issues:**
- Review this plan
- Check agent descriptions in `.claude/agents/`
- Review templates in `Resources/templates/`
- Consult repository README.md

**Process improvements:**
- Document in `Processing/improvements.md`
- Update this plan as needed
- Share learnings in processing log

---

**Plan Version:** 1.0
**Last Updated:** 2025-11-18
**Next Review:** After November processing complete
