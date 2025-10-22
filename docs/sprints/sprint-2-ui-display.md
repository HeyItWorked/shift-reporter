# Sprint 2: Basic UI & Report Display

**Dates:** Week 3-4

**Sprint Goal:** Create main UI to display report data, add navigation framework

**Velocity Target:** 23 points

---

## 📊 Sprint Backlog

| Story | Title | Points | Status |
|-------|-------|--------|--------|
| 2.1 | Main window UI | 5 | 📝 Todo |
| 2.2 | Summary display | 8 | 📝 Todo |
| 2.3 | Operator table | 8 | 📝 Todo |
| 2.4 | Notes field | 2 | 📝 Todo |

**Total Points:** 23

**Dependencies:** Sprint 1 complete (needs parsed data)

---

## 📖 User Story 2.1: Main Window UI

**As a shift lead, I want a clean main window to navigate the application**

### Priority
MUST HAVE

### Story Points
5

### Acceptance Criteria
- [ ] Main window with title bar "Shift Reporter"
- [ ] Menu buttons: Reports, Contacts, Training, Settings
- [ ] Status bar shows current shift and date
- [ ] Application icon displayed
- [ ] Window is resizable with minimum size (800x600)
- [ ] Professional appearance (colors, fonts)

### Tasks
- [ ] Create `MainWindow.xaml` layout
- [ ] Design navigation panel (left sidebar or top menu)
- [ ] Add navigation buttons (WPF Button controls)
- [ ] Create `MainViewModel.cs`
- [ ] Implement MVVM binding for current shift display
- [ ] Add application icon (design or find free icon)
- [ ] Style with color scheme (#2c3e50 dark blue header, white background)
- [ ] Set minimum window size
- [ ] Test window resize behavior

### Definition of Done
- [x] Window looks professional
- [x] All navigation buttons present (even if not wired up yet)
- [x] MVVM pattern properly implemented
- [x] Current shift displays correctly
- [x] Window resizes properly

### XAML Layout Example
```xml
<Window x:Class="ShiftReporter.MainWindow"
        Title="Shift Reporter" Height="600" Width="900"
        MinHeight="600" MinWidth="800">
    <DockPanel>
        <!-- Top Menu Bar -->
        <Menu DockPanel.Dock="Top">
            <MenuItem Header="Reports" Command="{Binding ShowReportsCommand}"/>
            <MenuItem Header="Contacts" Command="{Binding ShowContactsCommand}"/>
            <MenuItem Header="Training" Command="{Binding ShowTrainingCommand}"/>
        </Menu>
        
        <!-- Status Bar -->
        <StatusBar DockPanel.Dock="Bottom">
            <StatusBarItem Content="{Binding CurrentShift}"/>
            <StatusBarItem Content="{Binding CurrentDate}"/>
        </StatusBar>
        
        <!-- Main Content Area -->
        <ContentControl Content="{Binding CurrentView}"/>
    </DockPanel>
</Window>
```

---

## 📖 User Story 2.2: Summary Display

**As a shift lead, I want to see a summary of tonight's production**

### Priority
MUST HAVE

### Story Points
8

### Acceptance Criteria
- [ ] Displays total units tested (large, prominent)
- [ ] Displays total passed count
- [ ] Displays total failed count
- [ ] Displays overall pass rate percentage
- [ ] Uses visual indicators (colors: green for good, yellow/red for issues)
- [ ] Updates when "Scan Logs" button clicked

### Tasks
- [ ] Create `ShiftReport` model class
- [ ] Design summary panel in XAML (Grid layout with cards)
- [ ] Create data bindings to ViewModel
- [ ] Implement "Scan Logs" button command
- [ ] Add `ICommand` implementation using CommunityToolkit.Mvvm
- [ ] Call `LogParserService` from ViewModel
- [ ] Style summary panel (background colors, borders, icons)
- [ ] Add loading spinner during scan
- [ ] Test with mock data

### Definition of Done
- [x] Summary displays correctly after scan
- [x] Numbers match mock log data
- [x] Visual styling is clean and readable
- [x] Loading indicator works
- [x] Button can be clicked repeatedly

### Model Structure
```csharp
public class ShiftReport
{
    public int TotalUnits { get; set; }
    public int PassedUnits { get; set; }
    public int FailedUnits { get; set; }
    public double PassRate => TotalUnits > 0 
        ? Math.Round((double)PassedUnits / TotalUnits * 100, 1) 
        : 0;
    public DateTime ShiftDate { get; set; }
    public Shift ShiftType { get; set; }
    public string Notes { get; set; }
    public List<OperatorStats> OperatorStats { get; set; }
}
```

### ViewModel Command
```csharp
[RelayCommand]
private async Task ScanLogsAsync()
{
    IsLoading = true;
    try
    {
        var logs = await _logParserService.ScanAndParseAsync();
        var stats = _logParserService.AggregateByOperator(logs);
        
        CurrentReport = new ShiftReport
        {
            TotalUnits = logs.Count,
            PassedUnits = logs.Count(l => l.Result == TestResult.Pass),
            FailedUnits = logs.Count(l => l.Result == TestResult.Fail),
            ShiftDate = DateTime.Now.Date,
            ShiftType = GetCurrentShift(),
            OperatorStats = stats
        };
    }
    finally
    {
        IsLoading = false;
    }
}
```

---

## 📖 User Story 2.3: Operator Table

**As a shift lead, I want to see a breakdown by operator in a table**

### Priority
MUST HAVE

### Story Points
8

### Acceptance Criteria
- [ ] Table with columns: Operator, Units, Passed, Failed, Pass Rate
- [ ] Rows display each operator's stats
- [ ] Table is sortable by column (click header to sort)
- [ ] Pass rate shows percentage with 1 decimal place (95.0%)
- [ ] Failed tests highlighted in red/yellow if count > 0
- [ ] Professional table styling (alternating rows)

### Tasks
- [ ] Use WPF DataGrid control
- [ ] Create `ObservableCollection<OperatorStats>` in ViewModel
- [ ] Bind DataGrid to collection
- [ ] Implement column definitions in XAML
- [ ] Add sorting functionality (built-in DataGrid feature)
- [ ] Create value converter for pass rate formatting
- [ ] Create value converter for color coding failed tests
- [ ] Style DataGrid (alternating row colors, header style, borders)
- [ ] Test with 1, 3, and 10 operators

### Definition of Done
- [x] DataGrid displays operator stats correctly
- [x] Sorting works for all columns
- [x] Visual styling matches design (professional look)
- [x] Color coding works for failures
- [x] Handles empty data gracefully (shows "No data")

### DataGrid XAML
```xml
<DataGrid ItemsSource="{Binding CurrentReport.OperatorStats}"
          AutoGenerateColumns="False"
          CanUserAddRows="False"
          IsReadOnly="True"
          AlternatingRowBackground="LightGray">
    <DataGrid.Columns>
        <DataGridTextColumn Header="Operator" Binding="{Binding OperatorName}" Width="*"/>
        <DataGridTextColumn Header="Total" Binding="{Binding TotalUnits}" Width="80"/>
        <DataGridTextColumn Header="Passed" Binding="{Binding PassedUnits}" Width="80"/>
        <DataGridTextColumn Header="Failed" Binding="{Binding FailedUnits}" Width="80">
            <DataGridTextColumn.ElementStyle>
                <Style TargetType="TextBlock">
                    <Style.Triggers>
                        <DataTrigger Binding="{Binding FailedUnits, Converter={StaticResource GreaterThanZero}}" Value="True">
                            <Setter Property="Foreground" Value="Red"/>
                            <Setter Property="FontWeight" Value="Bold"/>
                        </DataTrigger>
                    </Style.Triggers>
                </Style>
            </DataGridTextColumn.ElementStyle>
        </DataGridTextColumn>
        <DataGridTextColumn Header="Pass Rate" Binding="{Binding PassRate, StringFormat={}{0:F1}%}" Width="100"/>
    </DataGrid.Columns>
</DataGrid>
```

---

## 📖 User Story 2.4: Notes Field

**As a shift lead, I want to add optional notes about the shift**

### Priority
SHOULD HAVE

### Story Points
2

### Acceptance Criteria
- [ ] Multi-line text box for notes
- [ ] Label: "Additional Notes (optional)"
- [ ] Placeholder text visible when empty
- [ ] Character limit indicator (0/500 characters)
- [ ] Notes persist in report data

### Tasks
- [ ] Add TextBox control to XAML (multiline, text wrapping)
- [ ] Bind to `ShiftReport.Notes` property
- [ ] Implement character counter
- [ ] Add `UpdateSourceTrigger=PropertyChanged` for real-time binding
- [ ] Style text box (border, padding, margin)
- [ ] Test character limit enforcement (max 500)

### Definition of Done
- [x] Text box accepts input
- [x] Character counter updates in real-time
- [x] Notes saved in report object
- [x] Looks clean and intuitive
- [x] Placeholder text displays properly

### XAML Example
```xml
<StackPanel Margin="10">
    <Label Content="Additional Notes (optional)" FontWeight="Bold"/>
    <TextBox Text="{Binding CurrentReport.Notes, UpdateSourceTrigger=PropertyChanged}"
             TextWrapping="Wrap"
             AcceptsReturn="True"
             Height="100"
             MaxLength="500"
             VerticalScrollBarVisibility="Auto"/>
    <TextBlock Text="{Binding NotesCharacterCount, StringFormat={}{0}/500 characters}"
               HorizontalAlignment="Right"
               Foreground="Gray"
               FontSize="10"/>
</StackPanel>
```

---

## ⚠️ Sprint 2 Risks

| Risk | Probability | Impact | Mitigation |
|------|-------------|--------|------------|
| WPF learning curve | High | Medium | Watch tutorials, use documentation, start simple |
| MVVM pattern complexity | Medium | Medium | Use CommunityToolkit.Mvvm, follow examples |
| DataGrid styling challenges | Medium | Low | Use built-in styles first, customize later |

---

## 📚 Sprint 2 Resources

### WPF Tutorials
- [Microsoft WPF Documentation](https://docs.microsoft.com/en-us/dotnet/desktop/wpf/)
- [WPF Tutorial](https://www.wpftutorial.net/)

### MVVM Resources
- [CommunityToolkit.Mvvm](https://learn.microsoft.com/en-us/dotnet/communitytoolkit/mvvm/)
- [MVVM Pattern Overview](https://docs.microsoft.com/en-us/dotnet/architecture/modernize-desktop/mvvm-pattern)

---

## 🎯 Sprint 2 Definition of Done

Same as Sprint 1, plus:
- [x] UI is responsive and user-friendly
- [x] MVVM pattern correctly implemented
- [x] Data binding works in both directions
- [x] Styling is consistent

---

## 📦 Sprint 2 Deliverable

Working UI that:
- ✅ Displays shift report summary
- ✅ Shows operator breakdown table
- ✅ Allows adding notes
- ✅ Has professional appearance
- ✅ Uses MVVM pattern correctly

**Ready for Sprint 3:** Outlook integration and contacts
