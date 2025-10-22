# Sprint 1: Foundation & Log Parser

**Dates:** Week 1-2

**Sprint Goal:** Get basic log parsing working and display results

**Velocity Target:** 21 points

---

## 📊 Sprint Backlog

| Story | Title | Points | Status |
|-------|-------|--------|--------|
| 1.1 | Scan ZAS logs | 8 | 📝 Todo |
| 1.2 | Parse log files | 8 | 📝 Todo |
| 1.3 | Aggregate statistics | 5 | 📝 Todo |

**Total Points:** 21

---

## 📖 User Story 1.1: Scan ZAS Log Files

**As a shift lead, I want to scan ZAS log files so that I don't have to manually count devices**

### Priority
MUST HAVE

### Story Points
8

### Acceptance Criteria
- [ ] Application can read files from `/mock_data/zas_logs/` directory
- [ ] Identifies `.log` files created in current shift timeframe
- [ ] Lists found files in UI with count
- [ ] Shows loading indicator while scanning
- [ ] Handles empty directory gracefully

### Tasks
- [ ] Create `LogParserService.cs` class in `/Services/`
- [ ] Implement `ScanLogDirectory(string path)` method
- [ ] Add shift time calculation logic (1st/2nd/3rd shift)
  - 1st shift: 6 AM - 2 PM
  - 2nd shift: 2 PM - 10 PM
  - 3rd shift: 10 PM - 6 AM
- [ ] Filter files by modification date/time
- [ ] Create `DeviceTestLog` model class in `/Models/`
- [ ] Return list of file paths
- [ ] Write unit test for file scanning

### Definition of Done
- [x] Code compiles without warnings
- [x] Unit tests pass
- [x] Scans folder and returns list of log files
- [x] Can determine current shift programmatically
- [x] Handles non-existent directory gracefully

### Technical Notes
```csharp
// Shift calculation example
public enum Shift { First, Second, Third }

public Shift GetCurrentShift()
{
    var hour = DateTime.Now.Hour;
    if (hour >= 6 && hour < 14) return Shift.First;
    if (hour >= 14 && hour < 22) return Shift.Second;
    return Shift.Third;
}
```

---

## 📖 User Story 1.2: Parse Log Files

**As a shift lead, I want log files parsed automatically so I can see device details**

### Priority
MUST HAVE

### Story Points
8

### Acceptance Criteria
- [ ] Extracts device serial number from log
- [ ] Extracts operator name/username
- [ ] Extracts test result (PASS/FAIL)
- [ ] Extracts software version
- [ ] Extracts language configured
- [ ] Extracts timestamp
- [ ] Handles malformed log files gracefully

### Tasks
- [ ] Design regex patterns for log parsing
- [ ] Implement `ParseLogFile(string filePath)` method
- [ ] Handle different log formats (if any variations)
- [ ] Map Windows username to employee name
- [ ] Add error handling for corrupt/incomplete files
- [ ] Write unit tests with sample logs
- [ ] Test with all 25 mock log files

### Definition of Done
- [x] Parses all mock log files successfully
- [x] Extracts all required fields
- [x] Returns structured `DeviceTestLog` objects
- [x] Unit tests cover edge cases
- [x] No exceptions thrown on bad data

### Sample Log Format
```
========================================
 AED Plus - Test Session Log
========================================
Serial Number: X25K45892
Operator: J. Smith (Employee #12345)
Test Date: 2025-10-22 22:30:15
Software Version: 6.24
Language: English (US)

TESTS PERFORMED:
[22:30:20] IrDA Connection... OK
[22:30:25] Battery Test... OK (12.4V)
[22:30:30] Self-Test... OK
[22:31:05] Electrode Test... OK

TEST RESULT: PASS
========================================
```

### Model Structure
```csharp
public class DeviceTestLog
{
    public string SerialNumber { get; set; }
    public string Operator { get; set; }
    public DateTime TestDate { get; set; }
    public string SoftwareVersion { get; set; }
    public string Language { get; set; }
    public TestResult Result { get; set; }
    public string FilePath { get; set; }
}

public enum TestResult { Pass, Fail }
```

---

## 📖 User Story 1.3: Aggregate Statistics

**As a shift lead, I want to see aggregated statistics by operator**

### Priority
MUST HAVE

### Story Points
5

### Acceptance Criteria
- [ ] Groups devices by operator
- [ ] Counts total units per operator
- [ ] Counts passed units per operator
- [ ] Counts failed units per operator
- [ ] Calculates pass rate percentage
- [ ] Sorts by operator name or unit count

### Tasks
- [ ] Create `OperatorStats` model class
- [ ] Implement `AggregateByOperator(List<DeviceTestLog> logs)` method
- [ ] Use LINQ to group and aggregate
- [ ] Calculate pass rates with proper rounding
- [ ] Handle division by zero (0 units tested)
- [ ] Add sorting logic
- [ ] Write unit tests for aggregation

### Definition of Done
- [x] Correctly aggregates mock data
- [x] Math is accurate (pass rate calculation)
- [x] Handles edge cases (0 units, all pass, all fail)
- [x] Unit tests verify calculations

### Model Structure
```csharp
public class OperatorStats
{
    public string OperatorName { get; set; }
    public int TotalUnits { get; set; }
    public int PassedUnits { get; set; }
    public int FailedUnits { get; set; }
    public double PassRate => TotalUnits > 0 
        ? Math.Round((double)PassedUnits / TotalUnits * 100, 1) 
        : 0;
}
```

### LINQ Example
```csharp
var stats = logs
    .GroupBy(log => log.Operator)
    .Select(group => new OperatorStats
    {
        OperatorName = group.Key,
        TotalUnits = group.Count(),
        PassedUnits = group.Count(l => l.Result == TestResult.Pass),
        FailedUnits = group.Count(l => l.Result == TestResult.Fail)
    })
    .OrderBy(s => s.OperatorName)
    .ToList();
```

---

## ⚠️ Sprint 1 Risks

| Risk | Probability | Impact | Mitigation |
|------|-------------|--------|------------|
| Log format variations | High | Medium | Use flexible regex, test with many samples |
| Performance with large file counts | Medium | Low | Test with 100+ files, optimize if needed |
| Date/time parsing edge cases | Medium | Medium | Use TryParse, handle different formats |

---

## 🎯 Sprint 1 Definition of Done

A story is "Done" when:
- [x] Code compiles without warnings
- [x] Unit tests written and passing
- [x] Manual testing completed
- [x] Edge cases handled
- [x] Code reviewed (self-review for solo)
- [x] Committed to Git with clear message

---

## 📈 Sprint 1 Ceremonies

### Sprint Planning (Day 1)
- Review user stories
- Break down into tasks
- Set up development environment
- Create branches if needed

### Daily Standup (Quick self-check)
- What did I complete yesterday?
- What will I work on today?
- Any blockers?

### Sprint Review (Day 14)
- Demo log parsing working
- Show aggregated statistics
- Record video/screenshots

### Sprint Retrospective (Day 14)
- What went well?
- What could be improved?
- Adjust velocity for Sprint 2

---

## 📦 Sprint 1 Deliverable

Working log parser that:
- ✅ Scans log directory
- ✅ Parses all log files
- ✅ Aggregates statistics by operator
- ✅ Has unit test coverage
- ✅ Handles errors gracefully

**Ready for Sprint 2:** UI display implementation
