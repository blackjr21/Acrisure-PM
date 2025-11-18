# TIER 1: FOUNDATION

**Agent:** ai-pm-meeting-notes  
**Purpose:** Transform raw transcripts into structured meeting notes

---

## Execution Steps

### 1. Batch Processing Strategy

Divide transcripts into batches of 8-10 files:
```bash
# Get all transcripts
TRANSCRIPTS=(Input/{Month}/*.txt)
TOTAL=${#TRANSCRIPTS[@]}

# Calculate batches
BATCH_SIZE=10
BATCHES=$(( (TOTAL + BATCH_SIZE - 1) / BATCH_SIZE ))

# Process each batch
for (( i=0; i<$BATCHES; i++ )); do
  start=$(( i * BATCH_SIZE ))
  end=$(( start + BATCH_SIZE ))
  batch_files="${TRANSCRIPTS[@]:$start:$BATCH_SIZE}"
  
  echo "Processing Batch $((i+1))/$BATCHES..."
  # Delegate to agent (see below)
done
```

### 2. Agent Delegation (per batch)
```
DELEGATE to ai-pm-meeting-notes agent (MUST BE USED for transcript processing):

Task: Transform these raw Otter.ai transcripts into structured meeting notes

Transcripts to process:
{List 8-10 specific filenames from Input/{Month}/}

Requirements:
1. Extract real attendee names from conversation (NOT "Speaker 1/2/3")
2. Create 5-section structure:
   - Title + Metadata (Date, Time, Attendees)
   - TLDR (2-6 bullet executive summary with semantic tags)
   - Tasks & Assignments (Owner | Due Date | Status format)
   - Risks (with severity indicators)
   - Notes (detailed context: project stage, technical details, business value)

3. Apply semantic tags where appropriate:
   - [ACTION] for action items
   - [RISK] for identified risks
   - [BLOCKER] for blocking issues
   - [DEPENDENCY] for dependencies
   - [VALUE] for business value statements
   - [DECISION] for decisions made

4. Classify each meeting type:
   - 1-on-1: Two participants by name → save to Meetings/1-on-1/
   - Team: 3-8 participants, same team → save to Meetings/Team/
   - General: 8+ or cross-functional → save to Meetings/General/

5. Save each note to: Meetings/{Type}/YYYY-MM-DD-Topic/notes.md

Success Criteria:
- All 5 sections present in each note
- Real names extracted (grep should find ZERO "Speaker [0-9]")
- Action items in Owner|Due|Status format
- Files organized by meeting type

Estimated time: 15-20 minutes per batch
```

### 3. Progress Tracking

After each batch, update processing log:
```markdown
### Batch {N} (Files {start}-{end})
**Started**: {timestamp}
**Files**: [list filenames]
**Agent**: ai-pm-meeting-notes
**Status**: ✅ Success
**Duration**: {time}
**Output**:
  - 1-on-1 notes: {count}
  - Team notes: {count}
  - General notes: {count}
**Issues**: {None or list}

**Completed**: {timestamp}
```

### 4. Quality Checkpoint After All Batches

Once all batches complete, run validation (see validation-procedures.md for details).

---

## Interactive Mode Pause
```
╔══════════════════════════════════════╗
║   TIER 1 COMPLETE                    ║
╚══════════════════════════════════════╝

📊 Summary:
  • Batches processed: {X}/{X}
  • Transcripts processed: {Y}
  • Meeting notes created: {Z}

✅ Validation: {passed}/{sampled} files passed

Options:
  [C] Continue to Tier 2
  [S] Skip to Tier 3 or 4
  [A] Switch to Autonomous mode
  [V] View sample output
  [X] Abort workflow

Your choice:
```
