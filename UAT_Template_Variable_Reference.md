# UAT Email Template - Variable Reference Guide

## Template Overview
This comprehensive email template adapts content based on three test levels:
- **SITS**: System Integration Testing
- **UAT**: User Acceptance Testing  
- **GO-LIVE**: Production Readiness

---

## Core Variables & Mapping

### Header Variables
```
{{TEST_LEVEL}}           → SITS | UAT | GO-LIVE
{{PROJECT_NAME}}         → e.g., "MERCHANT POS verifone X990 (TermApp.eftcPOS)"
{{CYCLE_NAME}}           → e.g., "UAT 2 Cycle 1"
{{REPORT_DATE}}          → Date in format: YYYY-MM-DD
{{TERMINAL_ID}}          → Terminal ID from test data (e.g., UAT9906)
{{TESTER_NAME}}          → Test Lead Name (e.g., QA TEST LEAD)
{{OVERALL_STATUS}}       → COMPLETED | IN-PROGRESS | BLOCKED
{{STATUS_COLOR}}         → #4CAF50 (green) | #FF9800 (orange) | #f44336 (red)
{{HEADER_COLOR}}         → Dynamic based on TEST_LEVEL (see color mapping)
```

### Header Color Mapping
```
SITS      → #2196F3 (Blue)
UAT       → #FF9800 (Orange)
GO-LIVE   → #f44336 (Red)
```

### Metrics Variables
```
{{COMPLETION_PERCENTAGE}}    → % of test cases executed (0-100)
{{PASS_COUNT}}              → Number of passed tests
{{FAIL_COUNT}}              → Number of failed tests
{{PASS_PERCENTAGE}}         → (PASS_COUNT / TOTAL) × 100
{{FAIL_PERCENTAGE}}         → (FAIL_COUNT / TOTAL) × 100
{{TOTAL_TEST_CASES}}        → Sum of all test cases in phase
```

### Phase Rows (Repeat for each classification)
```
{{PHASE_NAME}}              → e.g., "CABS-ON-CABS", "ZIMSWITCH(Acquiring)"
{{CLASSIFICATION}}          → e.g., "Card-Based (CABS-ON-CABS)"
{{STATUS}}                  → COMPLETED | IN-PROGRESS | BLOCKED
{{COMPLETION}}              → Phase completion percentage
{{PASS}}                    → Count of passed tests in phase
{{FAIL}}                    → Count of failed tests in phase
{{TOTAL}}                   → Total test cases in phase

Conditional Variables:
{{#IF_STATUS_COMPLETED}}    → Show if status = COMPLETED
{{#IF_STATUS_IN_PROGRESS}}  → Show if status = IN-PROGRESS
{{#IF_STATUS_BLOCKED}}      → Show if status = BLOCKED
{{STATUS_COLOR}}            → Color for progress bar
```

### Test Case Execution Rows (Repeat for each test case)
```
{{FUNCTION_NAME}}           → e.g., "BALANCE", "SALE", "Sale + Cash"
{{PRODUCT}}                 → e.g., "Blue (Debit/Prepaid)", "Platinum (Corporate)"
{{CARD_NO}}                 → Card number (masked/partial)
{{ACCOUNT_NO}}              → Account number
{{PRE_CONDITIONS}}          → Pre-condition description
{{EXPECTED}}                → Expected result (e.g., "APPROVED", "SUCCESS")
{{ACTUAL}}                  → Actual result (e.g., "PASS", "SUCCESS")
{{FTS}}                     → FT tracking code (e.g., "FT20634:CB4")

Conditional:
{{#IF_PASS}}                → Show if result = PASS
{{#IF_FAIL}}                → Show if result = FAIL
```

### Issues Log Variables
```
{{#IF_ISSUES_EXIST}}        → Show section only if issues exist
{{ISSUES_COUNT}}            → Total number of issues logged
{{ISSUE_ID}}                → Issue identifier (e.g., "JRA-001")
{{ISSUE_TITLE}}             → Issue description
{{SEVERITY}}                → CRITICAL | HIGH | MEDIUM | LOW
{{SEVERITY_COLOR}}          → #f44336 | #ff9800 | #FFC107 | #4CAF50
{{ASSIGNED_TO}}             → Name of person assigned
{{ISSUE_STATUS}}            → Open | In-Progress | Resolved | Closed
```

### Phase-Specific Conditionals
```
{{#IF_SITS}}                → SITS-specific content block
{{#IF_UAT}}                 → UAT-specific content block
{{#IF_GOLIVE}}              → GO-LIVE-specific content block
```

### Next Steps Variables
```
{{NEXT_MILESTONE_DATE}}     → Target completion date
{{NEXT_MILESTONE_ACTION}}   → Action required by that date
{{BLOCKERS_SUMMARY}}        → Summary of current blockers
{{DEPENDENCIES_SUMMARY}}    → External dependencies
{{RISK_LEVEL}}              → LOW | MEDIUM | HIGH | CRITICAL
{{RISK_DESCRIPTION}}        → Description of risks
```

### Footer Variables
```
{{REPORT_TIME}}             → Time in format HH:MM:SS
{{QA_LEAD_EMAIL}}           → Email of QA Lead
```

---

## Data Collection from Excel Sheets

### Extract from Sheet 1 (Terminal Details)
- Terminal ID → {{TERMINAL_ID}}
- Tester Name → {{TESTER_NAME}}
- Functions with Status, Expected, Actual → Test Case Rows

### Extract from Sheet 2 (Test Summary)
- Phase Classification → {{PHASE_NAME}}, {{CLASSIFICATION}}
- Status Completed/In-Progress → {{STATUS}}
- % Completion → {{COMPLETION}}
- Pass/Fail Counts → {{PASS}}, {{FAIL}}
- Number of Test-cases → {{TOTAL}}

### Calculation Formulas
```
COMPLETION_PERCENTAGE = (Executed Test Cases / Total Test Cases) × 100
PASS_PERCENTAGE = (Pass Count / Total Executed) × 100
FAIL_PERCENTAGE = (Fail Count / Total Executed) × 100
OVERALL_STATUS = IF(FAIL_COUNT > 0, "BLOCKED", IF(COMPLETION < 100, "IN-PROGRESS", "COMPLETED"))
```

---

## Usage Instructions

### For SITS Phase:
1. Set `{{TEST_LEVEL}}` = "SITS"
2. Include all integration test metrics
3. Highlight integration points and system connectivity
4. Focus on system-level failures

### For UAT Phase:
1. Set `{{TEST_LEVEL}}` = "UAT"
2. Include business scenario validations
3. Show user workflow status
4. Emphasize user sign-offs and business approvals

### For GO-LIVE Phase:
1. Set `{{TEST_LEVEL}}` = "GO-LIVE"
2. Set `{{OVERALL_STATUS}}` = "COMPLETED" (mandatory before go-live)
3. Require 100% test case completion
4. Display critical issues separately with mandatory resolution dates

---

## Color Scheme Reference

| Element | SITS | UAT | GO-LIVE |
|---------|------|-----|---------|
| Header | #2196F3 (Blue) | #FF9800 (Orange) | #f44336 (Red) |
| Pass Badge | #4CAF50 | #4CAF50 | #4CAF50 |
| Fail Badge | #f44336 | #f44336 | #f44336 |
| Partial Badge | #FF9800 | #FF9800 | #FF9800 |
| Alert Box | Blue | Orange | Red |

---

## Sample Data Population

### Example SITS Report:
```
Test_Level: SITS
Project: MERCHANT POS verifone X990
Cycle: UAT 2 Cycle 1
Completion: 100%
Pass: 24
Fail: 0
Total: 24
Status: COMPLETED
```

### Example UAT Report:
```
Test_Level: UAT
Project: MERCHANT POS verifone X990
Cycle: UAT 2 Cycle 1
Completion: 100%
Pass: 46
Fail: 0
Total: 46
Status: COMPLETED
```

---

## Conditional Logic Reference

```
If TEST_LEVEL = "SITS":
  - Show: Integration focus areas
  - Color: Blue (#2196F3)
  - Alert: System Integration Testing in progress

If TEST_LEVEL = "UAT":
  - Show: Business validation focus
  - Color: Orange (#FF9800)
  - Alert: User Acceptance Testing underway
  - Require: User sign-off fields

If TEST_LEVEL = "GO-LIVE":
  - Show: Production readiness checklist
  - Color: Red (#f44336)
  - Alert: CRITICAL - Production readiness testing
  - Require: ZERO failed tests for approval
  - Show: Rollback procedures
```

---

## Integration with Excel

### Extract Data Process:
1. Read Terminal ID & Tester from Sheet 1 header
2. Collect all test case rows (Function, Card, Account, Pre-conditions, Expected, Actual, Status, FT's)
3. Calculate aggregated metrics from Sheet 2
4. Count Pass/Fail by phase classification
5. Extract Issues Log data
6. Populate template variables with extracted data

### Automation Recommendation:
Use Excel VBA or Python with `openpyxl` to:
- Read Excel sheets
- Calculate metrics
- Generate HTML from template
- Send via email with dynamic header color

---

## Testing the Template

### QA Checklist:
- [ ] All placeholders correctly replaced with actual data
- [ ] Header color matches TEST_LEVEL
- [ ] Metrics add up correctly (PASS + FAIL = TOTAL)
- [ ] Progress bars display correct percentages
- [ ] Conditional sections show for correct test level
- [ ] Issue log displays only when issues exist
- [ ] All links and formatting render correctly in email client
- [ ] Mobile responsive design verified
- [ ] Test case table shows all rows from Excel