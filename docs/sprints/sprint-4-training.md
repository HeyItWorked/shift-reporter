# Sprint 4: Training Management System

**Dates:** Week 7-8

**Sprint Goal:** Build training tracking, certification display, and basic sign-off workflow

**Velocity Target:** 22 points

---

## 📊 Sprint Backlog

| Story | Title | Points | Status |
|-------|-------|--------|--------|
| 4.1 | My certifications view | 5 | 📝 Todo |
| 4.2 | Team compliance view | 5 | 📝 Todo |
| 4.3 | Training checklist | 8 | 📝 Todo |
| 4.4 | Digital sign-off | 4 | 📝 Todo |

**Total Points:** 22

---

## 📖 User Story 4.1: My Certifications View

**As a shift lead, I want to see my training certifications**

### Priority
MUST HAVE

### Story Points
5

### Acceptance Criteria
- [ ] "Training" button opens training window
- [ ] Shows list of my certifications
- [ ] Each cert shows: Module name, Certified date, Expiration date, Status
- [ ] Status indicators: Valid (green), Expires Soon (yellow), Expired (red)
- [ ] "Expires Soon" = within 30 days
- [ ] Shows employee info (name, ID, role)

### Tasks
- [ ] Create `TrainingWindow.xaml`
- [ ] Create `TrainingViewModel.cs`
- [ ] Create `Employee` and `Certification` model classes
- [ ] Load employees from `mock_data/employees.json`
- [ ] Implement `GetCurrentUserCertifications()` method
- [ ] Match current Windows username to employee
- [ ] Design certification list view with DataGrid
- [ ] Implement status calculation logic
- [ ] Create value converters for status colors
- [ ] Test with mock employee data

### Definition of Done
- [x] Training window opens and displays certifications
- [x] Status calculated correctly based on dates
- [x] Color coding works (green/yellow/red)
- [x] Shows correct data for current Windows user
- [x] Handles employee not found gracefully

### Model Structure
```csharp
public class Employee
{
    public int EmployeeId { get; set; }
    public string Username { get; set; }
    public string FullName { get; set; }
    public string Role { get; set; }
    public string Shift { get; set; }
    public List<Certification> Certifications { get; set; }
}

public class Certification
{
    public string ModuleId { get; set; }
    public string ModuleName { get; set; }
    public DateTime CertifiedDate { get; set; }
    public DateTime ExpirationDate { get; set; }
    public string TrainerName { get; set; }
    
    public CertificationStatus Status
    {
        get
        {
            if (ExpirationDate < DateTime.Now) return CertificationStatus.Expired;
            if ((ExpirationDate - DateTime.Now).TotalDays <= 30) return CertificationStatus.ExpiresSoon;
            return CertificationStatus.Valid;
        }
    }
}

public enum CertificationStatus
{
    Valid,
    ExpiresSoon,
    Expired
}
```

---

## 📖 User Story 4.2: Team Compliance View

**As a supervisor, I want to see team training compliance**

### Priority
SHOULD HAVE

### Story Points
5

### Acceptance Criteria
- [ ] "Team View" tab in training window (if supervisor)
- [ ] Table showing all team members
- [ ] Columns: Name, Overall Compliance %, Expiring Soon count, Status
- [ ] Sortable by compliance percentage
- [ ] Click employee name to see details
- [ ] Shows summary: X of Y employees fully compliant

### Tasks
- [ ] Add `UserPermissions.cs` for role checking
- [ ] Check if current user is supervisor
- [ ] Show/hide "Team View" tab based on permission
- [ ] Implement `GetTeamComplianceOverview()` method
- [ ] Calculate overall compliance percentage per employee
- [ ] Count expiring certifications
- [ ] Create summary stats
- [ ] Design team compliance DataGrid
- [ ] Add drill-down to employee detail view
- [ ] Test with multiple employees

### Definition of Done
- [x] Team view visible only for supervisors
- [x] Compliance calculated correctly
- [x] Table displays all team members
- [x] Sorting works
- [x] Drill-down navigation works

### Compliance Calculation
```csharp
public class EmployeeCompliance
{
    public string EmployeeName { get; set; }
    public int TotalCertifications { get; set; }
    public int ValidCertifications { get; set; }
    public int ExpiringSoonCount { get; set; }
    public int ExpiredCount { get; set; }
    
    public double CompliancePercentage => TotalCertifications > 0
        ? Math.Round((double)ValidCertifications / TotalCertifications * 100, 1)
        : 0;
    
    public ComplianceStatus Status
    {
        get
        {
            if (ExpiredCount > 0) return ComplianceStatus.NonCompliant;
            if (ExpiringSoonCount > 0) return ComplianceStatus.Warning;
            return ComplianceStatus.Compliant;
        }
    }
}
```

---

## 📖 User Story 4.3: Training Checklist

**As a trainer, I want to document training completion**

### Priority
SHOULD HAVE

### Story Points
8

### Acceptance Criteria
- [ ] "New Training" button opens training form
- [ ] Select trainee from dropdown
- [ ] Select training module from dropdown
- [ ] Training checklist displays with sections
- [ ] Can check off each item in checklist
- [ ] Can add notes per section
- [ ] Shows progress percentage (X of Y items complete)
- [ ] Can save draft or complete training

### Tasks
- [ ] Create `TrainingSessionWindow.xaml`
- [ ] Load training modules from `mock_data/training_modules.json`
- [ ] Create `TrainingModule` and `TrainingSection` models
- [ ] Design checklist UI (CheckBox controls in grouped ListView)
- [ ] Implement check/uncheck functionality with binding
- [ ] Calculate completion percentage dynamically
- [ ] Add notes TextBox per section
- [ ] Implement "Save Draft" functionality
- [ ] Create `TrainingSession` model to store state
- [ ] Test with sample training module

### Definition of Done
- [x] Training form displays checklist items
- [x] Checkboxes work and update progress
- [x] Progress percentage calculates correctly
- [x] Can add notes to sections
- [x] Draft saves to file or database
- [x] All data preserved when reopening

### Training Module Structure
```csharp
public class TrainingModule
{
    public string ModuleId { get; set; }
    public string ModuleName { get; set; }
    public int DurationHours { get; set; }
    public int ValidDurationMonths { get; set; }
    public List<TrainingSection> Sections { get; set; }
}

public class TrainingSection
{
    public string SectionName { get; set; }
    public List<ChecklistItem> ChecklistItems { get; set; }
}

public class ChecklistItem
{
    public string Description { get; set; }
    public bool IsCompleted { get; set; }
}

public class TrainingSession
{
    public string TraineeUsername { get; set; }
    public string ModuleId { get; set; }
    public DateTime StartDate { get; set; }
    public List<TrainingSection> Sections { get; set; }
    public Dictionary<string, string> SectionNotes { get; set; }
    public bool IsComplete { get; set; }
    
    public double CompletionPercentage
    {
        get
        {
            var totalItems = Sections.Sum(s => s.ChecklistItems.Count);
            var completedItems = Sections.Sum(s => s.ChecklistItems.Count(i => i.IsCompleted));
            return totalItems > 0 ? Math.Round((double)completedItems / totalItems * 100, 1) : 0;
        }
    }
}
```

### training_modules.json Example
```json
{
  "modules": [
    {
      "moduleId": "ZAS-001",
      "moduleName": "ZAS Basic Operation",
      "durationHours": 4,
      "validDurationMonths": 12,
      "sections": [
        {
          "sectionName": "Safety & PPE",
          "checklistItems": [
            "Reviewed ESD precautions",
            "Proper handling of AED devices demonstrated",
            "Understood battery safety procedures"
          ]
        },
        {
          "sectionName": "System Navigation",
          "checklistItems": [
            "Logged into ZAS successfully",
            "Navigated main menu options",
            "Located device history records"
          ]
        }
      ]
    }
  ]
}
```

---

## 📖 User Story 4.4: Digital Sign-Off

**As a trainer, I want to digitally sign off on completed training**

### Priority
SHOULD HAVE

### Story Points
4

### Acceptance Criteria
- [ ] After all checklist items complete, "Complete & Sign" button enabled
- [ ] Opens signature dialog
- [ ] Shows summary of training completed
- [ ] Trainee enters PIN to acknowledge
- [ ] Trainer enters PIN to certify
- [ ] Captures timestamp automatically
- [ ] Records to training database/JSON
- [ ] Generates certification with expiration date

### Tasks
- [ ] Create `SignOffDialog.xaml`
- [ ] Display training summary in dialog
- [ ] Add two PIN entry fields (trainee and trainer)
- [ ] Implement PIN verification from config
- [ ] Capture `DateTime.Now` timestamp
- [ ] Create `TrainingRecord` model
- [ ] Save completed training to JSON file
- [ ] Calculate expiration date (certified date + valid duration)
- [ ] Update employee's certifications list
- [ ] Show success confirmation
- [ ] Test full workflow

### Definition of Done
- [x] Sign-off dialog appears after completion
- [x] PIN verification works for both parties
- [x] Timestamp captured automatically
- [x] Training record saved successfully
- [x] Employee's certifications updated
- [x] Expiration date calculated correctly

### Sign-Off Implementation
```csharp
public class TrainingRecord
{
    public int RecordId { get; set; }
    public int EmployeeId { get; set; }
    public string ModuleId { get; set; }
    public DateTime CertifiedDate { get; set; }
    public DateTime ExpirationDate { get; set; }
    public string TraineeUsername { get; set; }
    public string TrainerUsername { get; set; }
    public string TraineePinUsed { get; set; }  // Not the actual PIN, just confirmation
    public string TrainerPinUsed { get; set; }
    public Dictionary<string, string> SectionNotes { get; set; }
}

public async Task<bool> CompleteTrainingAsync(TrainingSession session, string traineePin, string trainerPin)
{
    // Verify PINs
    if (!VerifyPin(session.TraineeUsername, traineePin)) return false;
    if (!VerifyPin(Environment.UserName, trainerPin)) return false;
    
    // Create record
    var record = new TrainingRecord
    {
        EmployeeId = GetEmployeeId(session.TraineeUsername),
        ModuleId = session.ModuleId,
        CertifiedDate = DateTime.Now,
        ExpirationDate = CalculateExpiration(session.ModuleId),
        TraineeUsername = session.TraineeUsername,
        TrainerUsername = Environment.UserName,
        SectionNotes = session.SectionNotes
    };
    
    // Save to database
    await SaveTrainingRecordAsync(record);
    
    // Update employee certifications
    await UpdateEmployeeCertificationsAsync(record);
    
    // Log to audit trail
    AuditLogger.Log("TrainingCompleted", $"{session.ModuleId} - {session.TraineeUsername}");
    
    return true;
}
```

---

## ⚠️ Sprint 4 Risks

| Risk | Probability | Impact | Mitigation |
|------|-------------|--------|------------|
| Complex checklist UI | Medium | Medium | Start with simple layout, iterate |
| Date calculation edge cases | Medium | Medium | Write unit tests for all scenarios |
| File vs. database decision | Low | Low | Use JSON for prototype, SQLite for production |

---

## 📦 Sprint 4 Deliverable

Working training system:
- ✅ View personal certifications
- ✅ View team compliance (supervisors)
- ✅ Complete training with checklist
- ✅ Digital sign-off with dual PINs

**Ready for Sprint 5:** Polish, security, and documentation
