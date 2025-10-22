# Shift Reporter - Sprint Planning (2-Week Sprints)

## 📊 Project Overview

**Total Duration:** 10 weeks (5 sprints)

**Sprint Length:** 2 weeks

**Team Size:** 1 developer (solo project)

**Velocity Target:** 20-25 story points per sprint

**Start Date:** TBD

---

## 🎯 Sprint 0: Pre-Planning & Setup

**Duration:** Before Sprint 1

**Goal:** Get development environment ready, no coding yet

### Tasks

* [ ] Install Visual Studio 2022 Community
* [ ] Install DB Browser for SQLite
* [ ] Create GitHub repository
* [ ] Setup `.gitignore` for .NET projects
* [ ] Create project structure (folders)
* [ ] Add README.md
* [ ] Install required NuGet packages
* [ ] Create mock data files (ZAS logs, JSON files)
* [ ] Verify everything compiles

**Deliverable:** Empty project that builds successfully with mock data in place

---

## 🚀 Sprint 1: Foundation & Log Parser

**Dates:** Week 1-2

**Sprint Goal:** Get basic log parsing working and display results in simple UI

**Velocity Target:** 21 points

### User Stories

#### Story 1.1: As a shift lead, I want to scan ZAS log files so that I don't have to manually count devices

**Priority:** MUST HAVE

**Story Points:** 8

**Acceptance Criteria:**

* [ ] Application can read files from `/mock_data/zas_logs/` directory
* [ ] Identifies `.log` files created in current shift timeframe
* [ ] Lists found files in UI with count
* [ ] Shows loading indicator while scanning
* [ ] Handles empty directory gracefully

**Tasks:**

* [ ] Create `LogParserService.cs` class
* [ ] Implement `ScanLogDirectory()` method
* [ ] Add shift time calculation logic (1st/2nd/3rd shift)
* [ ] Filter files by modification date/time
* [ ] Create `DeviceTestLog` model class
* [ ] Write unit test for file scanning

**Definition of Done:**

* Code compiles without warnings
* Unit tests pass
* Scans folder and returns list of log files
* Can determine current shift programmatically

---

#### Story 1.2: As a shift lead, I want log files parsed automatically so I can see device details

**Priority:** MUST HAVE

**Story Points:** 8

**Acceptance Criteria:**

* [ ] Extracts device serial number from log
* [ ] Extracts operator name/username
* [ ] Extracts test result (PASS/FAIL)
* [ ] Extracts software version
* [ ] Extracts language configured
* [ ] Extracts timestamp
* [ ] Handles malformed log files gracefully

**Tasks:**

* [ ] Implement regex patterns for log parsing
* [ ] Create `ParseLogFile(string path)` method
* [ ] Handle different log formats (if any variations)
* [ ] Map Windows username to employee name
* [ ] Add error handling for corrupt files
* [ ] Write unit tests with sample logs
* [ ] Test with all 25 mock log files

**Definition of Done:**

* Parses all mock log files successfully
* Extracts all required fields
* Returns structured `DeviceTestLog` objects
* Unit tests cover edge cases
* No exceptions thrown on bad data

---

#### Story 1.3: As a shift lead, I want to see aggregated statistics by operator

**Priority:** MUST HAVE

**Story Points:** 5

**Acceptance Criteria:**

* [ ] Groups devices by operator
* [ ] Counts total units per operator
* [ ] Counts passed units per operator
* [ ] Counts failed units per operator
* [ ] Calculates pass rate percentage
* [ ] Sorts by operator name or unit count

**Tasks:**

* [ ] Create `OperatorStats` model class
* [ ] Implement `AggregateByOperator()` method using LINQ
* [ ] Calculate pass rates with proper rounding
* [ ] Handle division by zero (no units tested)
* [ ] Sort logic implementation
* [ ] Write unit tests for aggregation logic

**Definition of Done:**

* Correctly aggregates mock data
* Math is accurate (pass rate calculation)
* Handles edge cases (0 units, all pass, all fail)
* Unit tests verify calculations

---

### Sprint 1 Backlog Summary

| Story | Title                | Points | Status  |
| ----- | -------------------- | ------ | ------- |
| 1.1   | Scan ZAS logs        | 8      | 📝 Todo |
| 1.2   | Parse log files      | 8      | 📝 Todo |
| 1.3   | Aggregate statistics | 5      | 📝 Todo |

**Total Points:** 21

**Sprint Capacity:** 21 points ✅

### Sprint 1 Risks

* ⚠️ Log file format may vary (mitigate with regex flexibility)
* ⚠️ Parsing performance with many files (test with 100+ files)

---

## 🎨 Sprint 2: Basic UI & Report Display

**Dates:** Week 3-4

**Sprint Goal:** Create main UI to display report data, add navigation framework

**Velocity Target:** 23 points

### User Stories

#### Story 2.1: As a shift lead, I want a clean main window to navigate the application

**Priority:** MUST HAVE

**Story Points:** 5

**Acceptance Criteria:**

* [ ] Main window with title bar
* [ ] Menu buttons: Reports, Contacts, Training, Settings
* [ ] Status bar shows current shift and date
* [ ] Application icon displayed
* [ ] Window is resizable with minimum size
* [ ] Professional appearance (colors, fonts)

**Tasks:**

* [ ] Create `MainWindow.xaml` layout
* [ ] Add navigation buttons (WPF Button controls)
* [ ] Create `MainViewModel.cs`
* [ ] Implement MVVM binding for current shift display
* [ ] Add application icon (design or find free icon)
* [ ] Style with basic colors (#2c3e50 dark blue header, etc.)
* [ ] Test window resize behavior

**Definition of Done:**

* Window looks professional
* All navigation buttons present (even if not wired up yet)
* MVVM pattern properly implemented
* Current shift displays correctly

---

#### Story 2.2: As a shift lead, I want to see a summary of tonight's production

**Priority:** MUST HAVE

**Story Points:** 8

**Acceptance Criteria:**

* [ ] Displays total units tested (large, prominent)
* [ ] Displays total passed count
* [ ] Displays total failed count
* [ ] Displays overall pass rate percentage
* [ ] Uses visual indicators (colors: green for good, yellow/red for issues)
* [ ] Updates when "Scan Logs" button clicked

**Tasks:**

* [ ] Create `ShiftReport` model class
* [ ] Design summary panel in XAML (Grid layout)
* [ ] Create data bindings to ViewModel
* [ ] Implement "Scan Logs" button command
* [ ] Add `ICommand` implementation for button
* [ ] Style summary panel (background color, borders)
* [ ] Add loading spinner during scan
* [ ] Test with mock data

**Definition of Done:**

* Summary displays correctly after scan
* Numbers match mock log data
* Visual styling is clean
* Loading indicator works
* Button can be clicked repeatedly

---

#### Story 2.3: As a shift lead, I want to see a breakdown by operator in a table

**Priority:** MUST HAVE

**Story Points:** 8

**Acceptance Criteria:**

* [ ] Table with columns: Operator, Units, Passed, Failed, Pass Rate
* [ ] Rows display each operator's stats
* [ ] Table is sortable by column (click header to sort)
* [ ] Pass rate shows percentage with 1 decimal place
* [ ] Failed tests highlighted in red/yellow if count > 0
* [ ] Professional table styling

**Tasks:**

* [ ] Use WPF DataGrid control
* [ ] Create `ObservableCollection<OperatorStats>` in ViewModel
* [ ] Bind DataGrid to collection
* [ ] Implement column definitions in XAML
* [ ] Add sorting functionality
* [ ] Create value converter for pass rate formatting
* [ ] Create value converter for color coding failed tests
* [ ] Style DataGrid (alternating row colors, header style)
* [ ] Test with 1, 3, and 10 operators

**Definition of Done:**

* DataGrid displays operator stats correctly
* Sorting works for all columns
* Visual styling matches design
* Color coding works for failures
* Handles empty data gracefully

---

#### Story 2.4: As a shift lead, I want to add optional notes about the shift

**Priority:** SHOULD HAVE

**Story Points:** 2

**Acceptance Criteria:**

* [ ] Multi-line text box for notes
* [ ] Label: "Additional Notes (optional)"
* [ ] Placeholder text: "Example: Unit X25K45892 failed battery test, replaced and retested successfully"
* [ ] Character limit indicator (0/500 characters)
* [ ] Notes persist in report data

**Tasks:**

* [ ] Add TextBox control to XAML (multiline)
* [ ] Bind to `ShiftReport.Notes` property
* [ ] Implement character counter
* [ ] Add `UpdateSourceTrigger=PropertyChanged`
* [ ] Style text box (border, padding)
* [ ] Test character limit enforcement

**Definition of Done:**

* Text box accepts input
* Character counter updates in real-time
* Notes saved in report object
* Looks clean and intuitive

---

### Sprint 2 Backlog Summary

| Story | Title           | Points | Status  |
| ----- | --------------- | ------ | ------- |
| 2.1   | Main window UI  | 5      | 📝 Todo |
| 2.2   | Summary display | 8      | 📝 Todo |
| 2.3   | Operator table  | 8      | 📝 Todo |
| 2.4   | Notes field     | 2      | 📝 Todo |

**Total Points:** 23

**Sprint Capacity:** 23 points ✅

### Sprint 2 Dependencies

* Depends on Sprint 1 completion (needs parsed data)

### Sprint 2 Risks

* ⚠️ WPF learning curve (mitigate with tutorials/documentation)
* ⚠️ MVVM pattern complexity (use CommunityToolkit.Mvvm helpers)

---

## 📧 Sprint 3: Outlook Integration & Contact Directory

**Dates:** Week 5-6

**Sprint Goal:** Integrate Outlook for email reports, build contact directory feature

**Velocity Target:** 24 points

### User Stories

#### Story 3.1: As a shift lead, I want to preview the email report before sending

**Priority:** MUST HAVE

**Story Points:** 5

**Acceptance Criteria:**

* [ ] "Preview Report" button visible on main window
* [ ] Opens new window showing HTML-formatted report
* [ ] Includes all data: summary, operator table, notes
* [ ] Professional email styling (header, tables, colors)
* [ ] Preview window has "Close" and "Open in Outlook" buttons

**Tasks:**

* [ ] Create `ReportPreviewWindow.xaml`
* [ ] Implement `GenerateHtmlReport()` method
* [ ] Use WebBrowser control to display HTML
* [ ] Design HTML email template with CSS
* [ ] Create header with logo/title
* [ ] Format operator table in HTML
* [ ] Test HTML rendering in window
* [ ] Add navigation buttons

**Definition of Done:**

* Preview window displays formatted report
* All data present and accurate
* HTML looks professional
* Buttons work correctly

---

#### Story 3.2: As a shift lead, I want to open Outlook with a pre-filled email

**Priority:** MUST HAVE

**Story Points:** 8

**Acceptance Criteria:**

* [ ] "Open in Outlook" button launches Microsoft Outlook
* [ ] TO field pre-filled with recipients from config
* [ ] SUBJECT field pre-filled: "QA Shift Report - 3rd Shift - 2025-10-22"
* [ ] BODY contains full HTML report
* [ ] Email appears but does NOT auto-send
* [ ] User can edit email before sending
* [ ] Graceful error if Outlook not installed

**Tasks:**

* [ ] Add `Microsoft.Office.Interop.Outlook` COM reference
* [ ] Create `OutlookIntegration.cs` service class
* [ ] Implement `CreateEmail()` method using COM
* [ ] Load recipients from `config.json`
* [ ] Format subject line with shift and date
* [ ] Set `HTMLBody` property with report
* [ ] Call `mailItem.Display(false)` to show (not send)
* [ ] Add try-catch for COM exceptions
* [ ] Test with Outlook installed
* [ ] Test error handling without Outlook

**Definition of Done:**

* Outlook opens with pre-filled email
* All fields populated correctly
* HTML formatting preserved
* Error message shown if Outlook missing
* User maintains control (manual send)

---

#### Story 3.3: As a shift lead, I want to see a contact directory

**Priority:** MUST HAVE

**Story Points:** 8

**Acceptance Criteria:**

* [ ] "Contacts" button opens new window
* [ ] Categories: Emergency, Management, Technical Support
* [ ] Each contact shows: Role, Name, Phone, Email
* [ ] Contacts sortable/filterable by category
* [ ] Search box to find contacts quickly
* [ ] Professional layout with icons

**Tasks:**

* [ ] Create `ContactsWindow.xaml`
* [ ] Create `ContactsViewModel.cs`
* [ ] Create `Contact` model class
* [ ] Load contacts from `mock_data/contacts.json`
* [ ] Create `ContactManagerService.cs`
* [ ] Implement `LoadContacts()` method
* [ ] Design UI with ListBox or ListView grouped by category
* [ ] Add search TextBox with filter logic
* [ ] Add icons for categories (emoji or Material Design icons)
* [ ] Test with mock contact data

**Definition of Done:**

* Contact window displays all contacts
* Grouped by category clearly
* Search filters contacts in real-time
* Layout is clean and readable
* Data loads from JSON file

---

#### Story 3.4: As a shift lead, I want to see who's on-call right now

**Priority:** SHOULD HAVE

**Story Points:** 3

**Acceptance Criteria:**

* [ ] Contacts with on-call schedules show availability indicator
* [ ] Green dot = available now
* [ ] Gray dot = not currently on-call
* [ ] Calculates based on current day of week and time
* [ ] Shows "ON CALL NOW" badge for active on-call person

**Tasks:**

* [ ] Add `OnCallSchedule` property to Contact model
* [ ] Implement `IsAvailableNow()` method
* [ ] Check current `DateTime.Now.DayOfWeek`
* [ ] Check current time against available hours
* [ ] Create visual indicator (colored circle) in XAML
* [ ] Add value converter for availability to color
* [ ] Test with different days/times
* [ ] Test edge cases (midnight boundary, weekend)

**Definition of Done:**

* Availability calculated correctly
* Visual indicator displays properly
* Works for all days of week
* Handles shift boundaries (10 PM - 6 AM spanning two days)

---

### Sprint 3 Backlog Summary

| Story | Title               | Points | Status  |
| ----- | ------------------- | ------ | ------- |
| 3.1   | Email preview       | 5      | 📝 Todo |
| 3.2   | Outlook integration | 8      | 📝 Todo |
| 3.3   | Contact directory   | 8      | 📝 Todo |
| 3.4   | On-call indicator   | 3      | 📝 Todo |

**Total Points:** 24

**Sprint Capacity:** 24 points ✅

### Sprint 3 Dependencies

* Depends on Sprint 2 (needs report data structure)
* Requires Outlook installed for testing

### Sprint 3 Risks

* ⚠️ COM interop complexity (mitigate with good error handling)
* ⚠️ HTML rendering differences (test in Outlook and WebBrowser)

---

## 📋 Sprint 4: Training Management System

**Dates:** Week 7-8

**Sprint Goal:** Build training tracking, certification display, and basic sign-off workflow

**Velocity Target:** 22 points

### User Stories

#### Story 4.1: As a shift lead, I want to see my training certifications

**Priority:** MUST HAVE

**Story Points:** 5

**Acceptance Criteria:**

* [ ] "Training" button opens training window
* [ ] Shows list of my certifications
* [ ] Each cert shows: Module name, Certified date, Expiration date, Status
* [ ] Status indicators: Valid (green), Expires Soon (yellow), Expired (red)
* [ ] "Expires Soon" = within 30 days
* [ ] Shows employee info (name, ID, role)

**Tasks:**

* [ ] Create `TrainingWindow.xaml`
* [ ] Create `TrainingViewModel.cs`
* [ ] Create `Employee` and `Certification` model classes
* [ ] Load employees from `mock_data/employees.json`
* [ ] Implement `GetCurrentUserCertifications()` method
* [ ] Match current Windows username to employee
* [ ] Design certification list view with DataGrid or ListView
* [ ] Implement status calculation logic
* [ ] Create value converters for status colors
* [ ] Test with mock employee data

**Definition of Done:**

* Training window opens and displays certifications
* Status calculated correctly based on dates
* Color coding works (green/yellow/red)
* Shows correct data for current Windows user
* Handles employee not found gracefully

---

#### Story 4.2: As a supervisor, I want to see team training compliance

**Priority:** SHOULD HAVE

**Story Points:** 5

**Acceptance Criteria:**

* [ ] "Team View" tab in training window (if supervisor)
* [ ] Table showing all team members
* [ ] Columns: Name, Overall Compliance %, Expiring Soon count, Status
* [ ] Sortable by compliance percentage
* [ ] Click employee name to see details
* [ ] Shows summary: X of Y employees fully compliant

**Tasks:**

* [ ] Add `UserPermissions.cs` for role checking
* [ ] Check if current user is supervisor
* [ ] Show/hide "Team View" tab based on permission
* [ ] Implement `GetTeamComplianceOverview()` method
* [ ] Calculate overall compliance percentage per employee
* [ ] Count expiring certifications
* [ ] Create summary stats
* [ ] Design team compliance DataGrid
* [ ] Add drill-down to employee detail view
* [ ] Test with multiple employees

**Definition of Done:**

* Team view visible only for supervisors
* Compliance calculated correctly
* Table displays all team members
* Sorting works
* Drill-down navigation works

---

#### Story 4.3: As a trainer, I want to document training completion

**Priority:** SHOULD HAVE

**Story Points:** 8

**Acceptance Criteria:**

* [ ] "New Training" button opens training form
* [ ] Select trainee from dropdown
* [ ] Select training module from dropdown
* [ ] Training checklist displays with sections
* [ ] Can check off each item in checklist
* [ ] Can add notes per section
* [ ] Shows progress percentage (X of Y items complete)
* [ ] Can save draft or complete training

**Tasks:**

* [ ] Create `TrainingSessionWindow.xaml`
* [ ] Load training modules from JSON
* [ ] Create `TrainingModule` and `TrainingSection` models
* [ ] Design checklist UI (CheckBox controls in grouped ListView)
* [ ] Implement check/uncheck functionality with binding
* [ ] Calculate completion percentage dynamically
* [ ] Add notes TextBox per section
* [ ] Implement "Save Draft" functionality
* [ ] Create `TrainingSession` model to store state
* [ ] Test with sample training module

**Definition of Done:**

* Training form displays checklist items
* Checkboxes work and update progress
* Progress percentage calculates correctly
* Can add notes to sections
* Draft saves to file or database
* All data preserved when reopening

---

#### Story 4.4: As a trainer, I want to digitally sign off on completed training

**Priority:** SHOULD HAVE

**Story Points:** 4

**Acceptance Criteria:**

* [ ] After all checklist items complete, "Complete & Sign" button enabled
* [ ] Opens signature dialog
* [ ] Shows summary of training completed
* [ ] Trainee enters PIN to acknowledge
* [ ] Trainer enters PIN to certify
* [ ] Captures timestamp automatically
* [ ] Records to training database/JSON
* [ ] Generates certification with expiration date

**Tasks:**

* [ ] Create `SignOffDialog.xaml`
* [ ] Display training summary in dialog
* [ ] Add two PIN entry fields (trainee and trainer)
* [ ] Implement PIN verification from config
* [ ] Capture `DateTime.Now` timestamp
* [ ] Create `TrainingRecord` model
* [ ] Save completed training to JSON file
* [ ] Calculate expiration date (certified date + valid duration)
* [ ] Update employee's certifications list
* [ ] Show success confirmation
* [ ] Test full workflow

**Definition of Done:**

* Sign-off dialog appears after completion
* PIN verification works for both parties
* Timestamp captured automatically
* Training record saved successfully
* Employee's certifications updated
* Expiration date calculated correctly

---

### Sprint 4 Backlog Summary

| Story | Title                  | Points | Status  |
| ----- | ---------------------- | ------ | ------- |
| 4.1   | My certifications view | 5      | 📝 Todo |
| 4.2   | Team compliance view   | 5      | 📝 Todo |
| 4.3   | Training checklist     | 8      | 📝 Todo |
| 4.4   | Digital sign-off       | 4      | 📝 Todo |

**Total Points:** 22

**Sprint Capacity:** 22 points ✅

### Sprint 4 Dependencies

* Needs completed contact directory (for supervisor role check)

### Sprint 4 Risks

* ⚠️ Complex UI for checklist (mitigate with simple layout first)
* ⚠️ Date calculations for expiration (test edge cases)

---

## 🎨 Sprint 5: Polish, Security & Documentation

**Dates:** Week 9-10

**Sprint Goal:** Add security features, polish UI, complete documentation, prepare for demo

**Velocity Target:** 21 points

### User Stories

#### Story 5.1: As an administrator, I want all actions logged for audit compliance

**Priority:** MUST HAVE

**Story Points:** 5

**Acceptance Criteria:**

* [ ] All critical actions logged: report sent, contacts viewed, training signed
* [ ] Log includes: timestamp, Windows user, computer name, action, details
* [ ] Logs written to append-only file
* [ ] Logs also saved to SQLite database
* [ ] No logs can be deleted by normal users
* [ ] Audit log viewer window (read-only)

**Tasks:**

* [ ] Create `AuditLoggerService.cs`
* [ ] Implement `LogAction()` method
* [ ] Create `AuditLog` model class
* [ ] Setup SQLite database schema for audit logs
* [ ] Write logs to both file and database
* [ ] Hook logging into all critical operations
* [ ] Create `AuditLogViewer.xaml` window
* [ ] Display logs in DataGrid (read-only)
* [ ] Add date range filter
* [ ] Test logging for all actions

**Definition of Done:**

* All critical actions logged
* Logs contain required information
* File and database both updated
* Audit viewer displays logs
* Cannot modify or delete logs from UI

---

#### Story 5.2: As a shift lead, I want PIN protection for sending reports

**Priority:** MUST HAVE

**Story Points:** 3

**Acceptance Criteria:**

* [ ] "Approve & Send" button prompts for PIN
* [ ] PIN dialog shows current user and action
* [ ] 4-digit PIN entry field (masked)
* [ ] Verifies PIN against config file
* [ ] Shows error message on wrong PIN
* [ ] Logs all PIN attempts (success and failure)

**Tasks:**

* [ ] Create `PinDialog.xaml`
* [ ] Create `PinVerification.cs` service
* [ ] Load PIN mapping from config.json
* [ ] Implement PIN verification logic
* [ ] Add PasswordBox control to dialog
* [ ] Show current Windows username in dialog
* [ ] Wire PIN dialog to "Approve & Send" button
* [ ] Log PIN attempts to audit log
* [ ] Test with correct and incorrect PINs
* [ ] Test with user not in PIN list

**Definition of Done:**

* PIN dialog appears when clicking send
* Correct PIN allows action to proceed
* Incorrect PIN shows error and blocks action
* All attempts logged to audit trail
* User-friendly error messages

---

#### Story 5.3: As a user, I want a polished, professional-looking interface

**Priority:** SHOULD HAVE

**Story Points:** 8

**Acceptance Criteria:**

* [ ] Consistent color scheme across all windows
* [ ] Professional fonts and sizing
* [ ] Icons for all major buttons
* [ ] Hover effects on buttons
* [ ] Smooth transitions/animations
* [ ] Proper spacing and alignment
* [ ] Loading indicators for long operations
* [ ] Tooltips on buttons and controls
* [ ] Responsive layout (works at different window sizes)

**Tasks:**

* [ ] Install MaterialDesignThemes NuGet package
* [ ] Apply Material Design styles to buttons
* [ ] Create consistent color palette (#2c3e50, #3498db, etc.)
* [ ] Add icons to all buttons (Material Design icons or emojis)
* [ ] Implement hover effects in XAML styles
* [ ] Add ProgressRing for loading states
* [ ] Add tooltips to all interactive controls
* [ ] Review and fix alignment issues
* [ ] Test at different window sizes
* [ ] Add fade-in animations for windows

**Definition of Done:**

* All windows use consistent styling
* Icons present on all major buttons
* Hover effects work smoothly
* Loading indicators appear during operations
* Tooltips provide helpful hints
* No alignment or spacing issues
* Professional appearance matching mockups

---

#### Story 5.4: As a developer, I want comprehensive documentation for maintenance

**Priority:** MUST HAVE

**Story Points:** 5

**Acceptance Criteria:**

* [ ] README.md complete with all sections
* [ ] ARCHITECTURE.md explains design decisions
* [ ] SECURITY.md documents security model
* [ ] API.md has XML documentation for public methods
* [ ] Code comments explain "why" not "what"
* [ ] Setup guide for new developers
* [ ] Mock data generation script/instructions
* [ ] Known issues and limitations documented

**Tasks:**

* [ ] Complete README.md (already drafted)
* [ ] Write ARCHITECTURE.md file
* [ ] Write SECURITY.md file
* [ ] Add XML documentation comments to all public APIs
* [ ] Add inline comments for complex logic
* [ ] Create SETUP.md for developer onboarding
* [ ] Document mock data structure
* [ ] Create CHANGELOG.md
* [ ] Add screenshots to /docs/screenshots/
* [ ] Create simple demo video (optional)

**Definition of Done:**

* All markdown files complete
* XML comments on all public methods
* Complex code has inline comments
* Screenshots in documentation folder
* New developer can setup from docs alone

---

### Sprint 5 Backlog Summary

| Story | Title          | Points | Status  |
| ----- | -------------- | ------ | ------- |
| 5.1   | Audit logging  | 5      | 📝 Todo |
| 5.2   | PIN protection | 3      | 📝 Todo |
| 5.3   | UI polish      | 8      | 📝 Todo |
| 5.4   | Documentation  | 5      | 📝 Todo |

**Total Points:** 21

**Sprint Capacity:** 21 points ✅

### Sprint 5 Focus

* **Quality over quantity** - refine existing features
* **User experience** - make it feel professional
* **Maintainability** - document everything

---

## 📊 Summary: All Sprints

| Sprint   | Goal                  | Stories | Points | Status  |
| -------- | --------------------- | ------- | ------ | ------- |
| Sprint 1 | Log Parser Foundation | 3       | 21     | 📝 Todo |
| Sprint 2 | UI & Report Display   | 4       | 23     | 📝 Todo |
| Sprint 3 | Outlook & Contacts    | 4       | 24     | 📝 Todo |
| Sprint 4 | Training Management   | 4       | 22     | 📝 Todo |
| Sprint 5 | Polish & Security     | 4       | 21     | 📝 Todo |

**Total Project Points:** 111

**Estimated Completion:** 10 weeks (5 sprints × 2 weeks)

---

## 🎯 Definition of Done (Project-Wide)

A story is "Done" when:

### Code Quality

* [ ] Code compiles without warnings
* [ ] No hardcoded values (use config files)
* [ ] Follows C# naming conventions
* [ ] MVVM pattern properly implemented
* [ ] Exception handling added
* [ ] Null checks in place

### Testing

* [ ] Unit tests written for business logic
* [ ] Manual testing completed
* [ ] Edge cases tested
* [ ] Works with mock data

### Documentation

* [ ] Public methods have XML comments
* [ ] Complex logic has inline comments
* [ ] README updated if needed

### Integration

* [ ] Integrates with existing codebase
* [ ] No breaking changes to other features
* [ ] Tested in full application context

### Review

* [ ] Self code review completed
* [ ] Git commit with clear message
* [ ] Pushed to repository

---

## 🚀 Sprint Ceremonies (Even for Solo Project!)

### Sprint Planning (Start of each sprint)

* [ ] Review previous sprint (what went well, what didn't)
* [ ] Select stories for current sprint
* [ ] Break down stories into tasks
* [ ] Estimate time for each task
* [ ] Set sprint goal

### Daily Standup (Quick check-in)

Ask yourself daily:

* What did I complete yesterday?
* What will I work on today?
* Any blockers or challenges?

### Sprint Review (End of each sprint)

* [ ] Demo working features
* [ ] Record demo video/screenshots
* [ ] Document completed stories
* [ ] Note what didn't get done

### Sprint Retrospective (End of each sprint)

Reflect on:

* What went well this sprint?
* What could be improved?
* What will I do differently next sprint?
* Did I overcommit or undercommit?

---

## 📈 Velocity Tracking

Track your actual velocity to improve estimation:

| Sprint   | Planned | Completed | Actual Velocity | Notes |
| -------- | ------- | --------- | --------------- | ----- |
| Sprint 1 | 21 pts  | __ pts    | __%             |       |
| Sprint 2 | 23 pts  | __ pts    | __%             |       |
| Sprint 3 | 24 pts  | __ pts    | __%             |       |
| Sprint 4 | 22 pts  | __ pts    | __%             |       |
| Sprint 5 | 21 pts  | __ pts    | __%             |       |

**Average Velocity:** Calculate after Sprint 2

**Trend:** Increasing / Stable / Decreasing

---

## ⚠️ Risk Register

| Risk                              | Probability | Impact | Mitigation Strategy                    |
| --------------------------------- | ----------- | ------ | -------------------------------------- |
| WPF learning curve                | High        | Medium | Allocate extra time, follow tutorials  |
| COM Interop complexity            | Medium      | High   | Test early, have fallback (mailto:)    |
| Mock data doesn't match real logs | Medium      | Medium | Get real sample early if possible      |
| Time estimation too optimistic    | High        | Medium | Track velocity, adjust sprint 2+       |
| Scope creep                       | Medium      | High   | Stick to MVP, document "nice to haves" |
| Outlook not installed for testing | Low         | Medium | Test on work machine, have VM ready    |

---

## 🎉 Milestone Checklist

### Sprint 1 Complete ✅

* [ ] Can scan log directory
* [ ] Can parse log files
* [ ] Can aggregate by operator
* [ ] Unit tests pass

### Sprint 2 Complete ✅

* [ ] Main window looks professional
* [ ] Report displays in UI
* [ ] Operator table works
* [ ] Can add notes

### Sprint 3 Complete ✅

* [ ] Can preview email report
* [ ] Outlook integration works
* [ ] Contact directory functional
* [ ] On-call indicator works

### Sprint 4 Complete ✅

* [ ] Training certifications display
* [ ] Team compliance view works
* [ ] Training checklist functional
* [ ] Digital sign-off works

### Sprint 5 Complete ✅

* [ ] Audit logging implemented
* [ ] PIN protection works
* [ ] UI polished and professional
* [ ] Documentation complete

### Project Complete 🎊

* [ ] All features working
* [ ] Demo video recorded
* [ ] Screenshots taken
* [ ] README finalized
* [ ] Repository published
* [ ] Portfolio piece ready!

---

## 📝 Notes for Success

### Tips for Solo Development

1. **Don't skip ceremonies** - Even solo, sprint planning keeps you organized
2. **Track your time** - Know how long things actually take
3. **Take breaks** - Code quality drops when tired
4. **Commit often** - Small, frequent commits are safer
5. **Document as you go** - Don't wait until end
6. **Test continuously** - Don't let bugs accumulate
7. **Celebrate wins** - Reward yourself at sprint milestones!

### When You Get Stuck

1. Check documentation (Microsoft Docs, Stack Overflow)
2. Simplify the problem (build incrementally)
3. Ask for help (forums, Discord communities)
4. Take a break (walk away, come back fresh)
5. Skip and move on (come back to it later)

### Adjusting the Plan

**Too Fast?**

* Add "nice to have" features from backlog
* Improve test coverage
* Refactor for code quality
* Add more polish

**Too Slow?**

* Move stories to later sprint
* Reduce scope of current stories
* Simplify implementation
* Focus on MVP features only

---

## 🏁 Ready to Start?

You now have:

* ✅ 5 sprints planned in detail
* ✅ 19 user stories with acceptance criteria
* ✅ 111 total story points estimated
* ✅ Tasks broken down for each story
* ✅ Definition of done
* ✅ Risk mitigation strategies
* ✅ Velocity tracking framework

**Next Steps:**

1. Set your Sprint 1 start date
2. Create a project board (GitHub Projects, Trello, or Notion)
3. Add all stories as cards/issues
4. Start Sprint 1, Story 1.1: Scan ZAS Logs
5. Code, commit, test, repeat!
