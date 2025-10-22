# Sprint 3: Outlook Integration & Contact Directory

**Dates:** Week 5-6

**Sprint Goal:** Integrate Outlook for email reports, build contact directory feature

**Velocity Target:** 24 points

---

## 📊 Sprint Backlog

| Story | Title | Points | Status |
|-------|-------|--------|--------|
| 3.1 | Email preview | 5 | 📝 Todo |
| 3.2 | Outlook integration | 8 | 📝 Todo |
| 3.3 | Contact directory | 8 | 📝 Todo |
| 3.4 | On-call indicator | 3 | 📝 Todo |

**Total Points:** 24

**Dependencies:** Sprint 2 complete (needs report data structure)

---

## 📖 User Story 3.1: Email Preview

**As a shift lead, I want to preview the email report before sending**

### Priority
MUST HAVE

### Story Points
5

### Acceptance Criteria
- [ ] "Preview Report" button visible on main window
- [ ] Opens new window showing HTML-formatted report
- [ ] Includes all data: summary, operator table, notes
- [ ] Professional email styling (header, tables, colors)
- [ ] Preview window has "Close" and "Open in Outlook" buttons

### Tasks
- [ ] Create `ReportPreviewWindow.xaml`
- [ ] Create `ReportGeneratorService.cs`
- [ ] Implement `GenerateHtmlReport(ShiftReport report)` method
- [ ] Use WebBrowser control to display HTML
- [ ] Design HTML email template with inline CSS
- [ ] Create header with logo/title
- [ ] Format operator table in HTML
- [ ] Test HTML rendering in window
- [ ] Add navigation buttons

### Definition of Done
- [x] Preview window displays formatted report
- [x] All data present and accurate
- [x] HTML looks professional
- [x] Buttons work correctly

### HTML Template Example
```csharp
public string GenerateHtmlReport(ShiftReport report)
{
    var html = $@"
    <html>
    <head>
        <style>
            body {{ font-family: Arial, sans-serif; margin: 20px; }}
            .header {{ background-color: #2c3e50; color: white; padding: 20px; }}
            .summary {{ margin: 20px 0; }}
            .summary-item {{ display: inline-block; margin: 10px 20px; }}
            table {{ border-collapse: collapse; width: 100%; }}
            th {{ background-color: #3498db; color: white; padding: 10px; }}
            td {{ border: 1px solid #ddd; padding: 8px; }}
            tr:nth-child(even) {{ background-color: #f2f2f2; }}
            .fail {{ color: red; font-weight: bold; }}
        </style>
    </head>
    <body>
        <div class='header'>
            <h1>QA Shift Report - {report.ShiftType} Shift - {report.ShiftDate:yyyy-MM-dd}</h1>
        </div>
        <div class='summary'>
            <div class='summary-item'><strong>Total Units:</strong> {report.TotalUnits}</div>
            <div class='summary-item'><strong>Passed:</strong> {report.PassedUnits}</div>
            <div class='summary-item'><strong>Failed:</strong> {report.FailedUnits}</div>
            <div class='summary-item'><strong>Pass Rate:</strong> {report.PassRate:F1}%</div>
        </div>
        <h2>Operator Breakdown</h2>
        <table>
            <tr><th>Operator</th><th>Total</th><th>Passed</th><th>Failed</th><th>Pass Rate</th></tr>
            {string.Join("", report.OperatorStats.Select(op => $@"
            <tr>
                <td>{op.OperatorName}</td>
                <td>{op.TotalUnits}</td>
                <td>{op.PassedUnits}</td>
                <td class='{(op.FailedUnits > 0 ? "fail" : "")}'>{op.FailedUnits}</td>
                <td>{op.PassRate:F1}%</td>
            </tr>"))}
        </table>
        {(!string.IsNullOrEmpty(report.Notes) ? $"<h2>Notes</h2><p>{report.Notes}</p>" : "")}
    </body>
    </html>";
    return html;
}
```

---

## 📖 User Story 3.2: Outlook Integration

**As a shift lead, I want to open Outlook with a pre-filled email**

### Priority
MUST HAVE

### Story Points
8

### Acceptance Criteria
- [ ] "Open in Outlook" button launches Microsoft Outlook
- [ ] TO field pre-filled with recipients from config
- [ ] SUBJECT field pre-filled: "QA Shift Report - 3rd Shift - 2025-10-22"
- [ ] BODY contains full HTML report
- [ ] Email appears but does NOT auto-send
- [ ] User can edit email before sending
- [ ] Graceful error if Outlook not installed

### Tasks
- [ ] Add `Microsoft.Office.Interop.Outlook` COM reference
- [ ] Create `OutlookIntegration.cs` service class
- [ ] Implement `CreateEmail(ShiftReport report)` method using COM
- [ ] Load recipients from `config.json`
- [ ] Format subject line with shift and date
- [ ] Set `HTMLBody` property with report
- [ ] Call `mailItem.Display(false)` to show (not send)
- [ ] Add try-catch for COM exceptions
- [ ] Test with Outlook installed
- [ ] Test error handling without Outlook

### Definition of Done
- [x] Outlook opens with pre-filled email
- [x] All fields populated correctly
- [x] HTML formatting preserved
- [x] Error message shown if Outlook missing
- [x] User maintains control (manual send)

### Implementation Example
```csharp
public class OutlookIntegrationService
{
    public void CreateEmail(ShiftReport report, string htmlBody)
    {
        try
        {
            var outlook = new Outlook.Application();
            var mailItem = (Outlook.MailItem)outlook.CreateItem(Outlook.OlItemType.olMailItem);
            
            // Load config
            var config = LoadConfig();
            mailItem.To = string.Join(";", config.ReportRecipients);
            mailItem.Subject = $"QA Shift Report - {report.ShiftType} Shift - {report.ShiftDate:yyyy-MM-dd}";
            mailItem.HTMLBody = htmlBody;
            
            // Display but don't send
            mailItem.Display(false);
        }
        catch (COMException ex)
        {
            MessageBox.Show("Microsoft Outlook is not installed or not configured properly.", 
                "Outlook Error", MessageBoxButton.OK, MessageBoxImage.Error);
            // Log exception
        }
    }
}
```

### Config.json
```json
{
  "reportRecipients": [
    "qa-manager@company.com",
    "shift-supervisor@company.com"
  ],
  "ccRecipients": [
    "production-team@company.com"
  ]
}
```

---

## 📖 User Story 3.3: Contact Directory

**As a shift lead, I want to see a contact directory**

### Priority
MUST HAVE

### Story Points
8

### Acceptance Criteria
- [ ] "Contacts" button opens new window
- [ ] Categories: Emergency, Management, Technical Support
- [ ] Each contact shows: Role, Name, Phone, Email
- [ ] Contacts sortable/filterable by category
- [ ] Search box to find contacts quickly
- [ ] Professional layout with icons

### Tasks
- [ ] Create `ContactsWindow.xaml`
- [ ] Create `ContactsViewModel.cs`
- [ ] Create `Contact` model class
- [ ] Load contacts from `mock_data/contacts.json`
- [ ] Create `ContactManagerService.cs`
- [ ] Implement `LoadContacts()` method
- [ ] Design UI with grouped ListView
- [ ] Add search TextBox with filter logic
- [ ] Add icons for categories (Material Design icons or emoji)
- [ ] Test with mock contact data

### Definition of Done
- [x] Contact window displays all contacts
- [x] Grouped by category clearly
- [x] Search filters contacts in real-time
- [x] Layout is clean and readable
- [x] Data loads from JSON file

### Contact Model
```csharp
public class Contact
{
    public string Category { get; set; }
    public string Role { get; set; }
    public string Name { get; set; }
    public string Phone { get; set; }
    public string Email { get; set; }
    public string Extension { get; set; }
    public OnCallSchedule OnCallSchedule { get; set; }
    
    public bool IsAvailableNow => OnCallSchedule?.IsAvailableNow() ?? true;
}

public class OnCallSchedule
{
    public List<DayOfWeek> OnCallDays { get; set; }
    public TimeSpan StartTime { get; set; }
    public TimeSpan EndTime { get; set; }
    
    public bool IsAvailableNow()
    {
        var now = DateTime.Now;
        return OnCallDays.Contains(now.DayOfWeek) &&
               now.TimeOfDay >= StartTime &&
               now.TimeOfDay <= EndTime;
    }
}
```

### contacts.json Example
```json
{
  "contacts": [
    {
      "category": "Emergency",
      "role": "Security",
      "name": "Security Desk",
      "phone": "555-0100",
      "extension": "100",
      "email": "security@company.com"
    },
    {
      "category": "Management",
      "role": "QA Manager",
      "name": "Mary Johnson",
      "phone": "555-0201",
      "email": "mjohnson@company.com",
      "onCallSchedule": {
        "onCallDays": ["Monday", "Tuesday", "Wednesday", "Thursday", "Friday"],
        "startTime": "06:00:00",
        "endTime": "18:00:00"
      }
    }
  ]
}
```

---

## 📖 User Story 3.4: On-Call Indicator

**As a shift lead, I want to see who's on-call right now**

### Priority
SHOULD HAVE

### Story Points
3

### Acceptance Criteria
- [ ] Contacts with on-call schedules show availability indicator
- [ ] Green dot = available now
- [ ] Gray dot = not currently on-call
- [ ] Calculates based on current day of week and time
- [ ] Shows "ON CALL NOW" badge for active on-call person

### Tasks
- [ ] Implement `IsAvailableNow()` method in Contact model
- [ ] Check current `DateTime.Now.DayOfWeek`
- [ ] Check current time against available hours
- [ ] Create visual indicator (colored circle) in XAML
- [ ] Add value converter for availability to color
- [ ] Test with different days/times
- [ ] Test edge cases (midnight boundary, weekend)

### Definition of Done
- [x] Availability calculated correctly
- [x] Visual indicator displays properly
- [x] Works for all days of week
- [x] Handles shift boundaries (10 PM - 6 AM spanning two days)

### XAML Example
```xml
<DataTemplate x:Key="ContactTemplate">
    <Border BorderBrush="Gray" BorderThickness="1" Padding="10" Margin="5">
        <StackPanel>
            <StackPanel Orientation="Horizontal">
                <Ellipse Width="10" Height="10" 
                         Fill="{Binding IsAvailableNow, Converter={StaticResource AvailabilityToColorConverter}}"
                         Margin="0,0,10,0"/>
                <TextBlock Text="{Binding Role}" FontWeight="Bold"/>
                <TextBlock Text=" - ON CALL" Foreground="Green" FontWeight="Bold"
                           Visibility="{Binding IsAvailableNow, Converter={StaticResource BoolToVisibilityConverter}}"/>
            </StackPanel>
            <TextBlock Text="{Binding Name}"/>
            <TextBlock Text="{Binding Phone}"/>
            <TextBlock Text="{Binding Email}" Foreground="Blue"/>
        </StackPanel>
    </Border>
</DataTemplate>
```

---

## ⚠️ Sprint 3 Risks

| Risk | Probability | Impact | Mitigation |
|------|-------------|--------|------------|
| COM Interop complexity | Medium | High | Research examples, test early, have mailto: fallback |
| HTML rendering differences | Medium | Medium | Test in both WebBrowser and Outlook |
| Outlook not installed | Low | Medium | Graceful error handling, test on work machine |

---

## 📚 Sprint 3 Resources

- [Outlook Interop Documentation](https://docs.microsoft.com/en-us/office/vba/api/overview/outlook)
- [COM Interop Guide](https://docs.microsoft.com/en-us/dotnet/framework/interop/com-interop-part-1--calling-unmanaged-functions)
- [HTML Email Best Practices](https://www.campaignmonitor.com/dev-resources/guides/coding/)

---

## 📦 Sprint 3 Deliverable

Working features:
- ✅ Email preview window with HTML report
- ✅ Outlook integration (COM automation)
- ✅ Contact directory with search
- ✅ On-call availability indicator

**Ready for Sprint 4:** Training management system
