# ERROR HANDLING PROTOCOL

Comprehensive error categorization and response procedures.

---

## Error Categories

### 1. TRANSIENT ERRORS (Retry Immediately)

**Examples:**
- Network timeouts
- Temporary file locks
- API rate limits
- "Resource busy" errors

**Response:**
```
TRANSIENT ERROR DETECTED
Action: Retry after 5 seconds (Attempt 1 of 2)
```

Wait 5 seconds, retry once.

---

### 2. VALIDATION ERRORS (Fix and Retry)

**Examples:**
- Missing required fields
- Incorrect file formats
- Incomplete sections
- Invalid data structures

**Response:**
```
VALIDATION ERROR DETECTED

Issue: {description}
File: {path}

Options:
[F] Fix automatically
[M] Manual intervention
[S] Skip this item
[R] Retry agent
[A] Abort

Your choice:
```

---

### 3. CRITICAL ERRORS (Stop Immediately)

**Examples:**
- File system permission denied
- Disk full
- Missing dependencies
- Corrupted input files
- Missing critical directories

**Response:**
```
🔴 CRITICAL ERROR - WORKFLOW STOPPED

Error: {description}
Location: Tier {N} → {agent}
Time: {timestamp}

State:
- Completed: Tiers 1-{X}
- Files processed: {count}
- Checkpoint: temp/processing/{Month}-checkpoint.json

Recovery:
1. {Fix step}
2. {Verify step}
3. Resume from checkpoint

DO NOT retry automatically.
```

**DO NOT** retry. STOP and await user action.

---

## Retry Logic Decision Tree
```
ERROR DETECTED
    ↓
    ├─ Transient?
    │  └─ YES → Wait 5 sec → Retry once
    │             ↓
    │             ├─ Success? → Continue
    │             └─ Fail? → Treat as CRITICAL
    │
    ├─ Validation?
    │  └─ YES → Report → Get guidance → Act
    │
    └─ Critical?
       └─ YES → STOP → Save checkpoint → Report
```

---

## Pre-flight Checks

Before starting workflow:
```bash
# Verify input directory exists
[ -d "Input/{Month}" ] && [ "$(ls -A Input/{Month})" ]

# Check write permissions
[ -w "Meetings/" ] && [ -w "People/" ] && [ -w "Projects/" ]

# Verify required directories
mkdir -p Meetings People Projects Status temp/processing temp/archive

# Check disk space (need 100MB)
SPACE=$(df -m . | tail -1 | awk '{print $4}')
[ $SPACE -gt 100 ]

# Verify agents exist
for agent in ai-pm-meeting-notes todo-tracker stakeholder-management-copilot \
             project-status-reporter schedule-planner meeting-agenda-preparer; do
  [ -f ".claude/agents/$agent.md" ]
done
```

If any check fails, report and abort.
