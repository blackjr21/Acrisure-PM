# VALIDATION PROCEDURES

Comprehensive validation protocols for each tier.

---

## Adaptive Sampling Formula
```bash
calculate_sample_size() {
  local total=$1
  local sample=3  # absolute minimum
  
  if [ $total -le 15 ]; then
    sample=3
  elif [ $total -le 50 ]; then
    sample=$(( total * 20 / 100 ))
    [ $sample -lt 3 ] && sample=3
  else
    sample=$(( total * 15 / 100 ))
    [ $sample -gt 10 ] && sample=10
    [ $sample -lt 3 ] && sample=3
  fi
  
  echo $sample
}
```

---

## TIER 1 VALIDATION: Meeting Notes

### Files to Validate
```bash
NOTES=$(find Meetings/ -name "notes.md" -mtime -1)
TOTAL=$(echo "$NOTES" | wc -l)
SAMPLE_SIZE=$(calculate_sample_size $TOTAL)
SAMPLE=$(echo "$NOTES" | shuf -n $SAMPLE_SIZE)
```

### Validation Checks (per sampled file)

#### Check 1: 5-Section Structure
```bash
validate_sections() {
  local file=$1
  local checks=0
  
  grep -q "^# " "$file" && ((checks++))  # Title
  grep -q "## TLDR" "$file" && ((checks++))
  grep -q "## Tasks & Assignments" "$file" && ((checks++))
  grep -q "## Risks" "$file" && ((checks++))
  grep -q "## Notes" "$file" && ((checks++))
  
  if [ $checks -eq 5 ]; then
    echo "✅ PASS: All 5 sections present"
    return 0
  else
    echo "❌ FAIL: Only $checks/5 sections found"
    return 1
  fi
}
```

#### Check 2: Real Attendee Names
```bash
validate_names() {
  local file=$1
  
  if grep -q "Speaker [0-9]" "$file"; then
    echo "❌ FAIL: Generic 'Speaker X' labels found"
    return 1
  else
    echo "✅ PASS: Real attendee names used"
    return 0
  fi
}
```

#### Check 3: Action Item Format
```bash
validate_actions() {
  local file=$1
  
  if grep -q "\[ACTION\]" "$file"; then
    if grep "\[ACTION\]" "$file" | grep -q "Owner:.*Due:.*Status:"; then
      echo "✅ PASS: Action items properly formatted"
      return 0
    else
      echo "❌ FAIL: Action items missing Owner/Due/Status"
      return 1
    fi
  else
    echo "ℹ️  INFO: No action items (discussion-only meeting)"
    return 0
  fi
}
```

---

## TIER 2 VALIDATION: Action Items & Stakeholders

### Check [TRACKED] Markers
```bash
ACTIONS=$(grep -r "\[ACTION\]" Meetings/ -mtime -1 | wc -l)
TRACKED=$(grep -r "\[TRACKED\]" Meetings/ -mtime -1 | wc -l)

if [ $ACTIONS -eq $TRACKED ]; then
  echo "✅ PASS: All actions have [TRACKED] markers"
else
  echo "❌ FAIL: $ACTIONS actions but only $TRACKED [TRACKED] markers"
fi
```

### Check People Profiles
```bash
NEW_PROFILES=$(find People/ -name "*.md" -mtime -1 | grep -v stakeholder-register | wc -l)
echo "✅ People profiles: $NEW_PROFILES created/updated"
```

---

## TIER 3 VALIDATION: Reports & Schedules

### File Existence Checks
```bash
WEEKLY_REPORTS=$(find Status/weekly-updates/ -name "*.md" -mtime -1 | wc -l)
SCHEDULES=$(find Projects/ -name "schedule.md" -mtime -1 | wc -l)

echo "✅ Status reports: $WEEKLY_REPORTS"
echo "✅ Schedules: $SCHEDULES"
```

### Content Quality Checks
```bash
validate_content() {
  local file=$1
  local size=$(wc -c < "$file")
  
  if [ $size -lt 500 ]; then
    echo "⚠️  WARNING: File appears incomplete ($size bytes)"
    return 1
  fi
  
  return 0
}
```

---

## Validation Failure Thresholds

| Success Rate | Action |
|--------------|--------|
| 95-100% | ✅ PASS - Continue |
| 85-94% | ⚠️  WARNING - Review, then continue |
| 70-84% | ⚠️  RETRY - Fix and re-validate |
| <70% | ❌ STOP - Manual review required |
