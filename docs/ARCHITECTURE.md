# Shift Reporter - Architecture Documentation

## 🏗️ High-Level Architecture

```
┌─────────────────────────────────────────────┐
│           PRESENTATION LAYER                │
│                  (WPF)                      │
│  ┌──────────┐ ┌──────────┐ ┌─────────────┐│
│  │  Main    │ │ Contacts │ │  Training   ││
│  │  Window  │ │  Window  │ │   Window    ││
│  └──────────┘ └──────────┘ └─────────────┘│
└──────────────────┬──────────────────────────┘
                   │
┌──────────────────▼──────────────────────────┐
│         BUSINESS LOGIC LAYER                │
│  ┌──────────────┐  ┌────────────────────┐  │
│  │  LogParser   │  │  ReportGenerator   │  │
│  └──────────────┘  └────────────────────┘  │
│  ┌──────────────┐  ┌────────────────────┐  │
│  │ContactManager│  │  TrainingManager   │  │
│  └──────────────┘  └────────────────────┘  │
│  ┌──────────────┐  ┌────────────────────┐  │
│  │SecurityMgr   │  │   AuditLogger      │  │
│  └──────────────┘  └────────────────────┘  │
└──────────────────┬──────────────────────────┘
                   │
┌──────────────────▼──────────────────────────┐
│            DATA ACCESS LAYER                │
│  ┌──────────────┐  ┌────────────────────┐  │
│  │  Database.cs │  │ MockDataLoader.cs  │  │
│  └──────────────┘  └────────────────────┘  │
│  ┌──────────────┐  ┌────────────────────┐  │
│  │ FileSystem   │  │   Models (POCO)    │  │
│  └──────────────┘  └────────────────────┘  │
└─────────────────────────────────────────────┘
```

## 🎨 Design Patterns

### MVVM (Model-View-ViewModel)
- **Views** (XAML): Pure UI, no business logic
- **ViewModels**: Handle UI state, commands, data binding
- **Models**: Data structures (POCOs)

**Benefit:** Clean separation, testability, maintainability

### Repository Pattern
- Abstracts data access behind interfaces
- Allows swapping mock data for production data
- Simplifies unit testing

### Factory Pattern
- Used for creating complex objects (reports, emails)
- Centralizes object creation logic

### Observer Pattern
- Notifications and alerts (training expiration, failed tests)
- Event-driven architecture

## 📂 Project Structure

```
/src/ShiftReporter/
├── /Views/              # XAML UI files
├── /ViewModels/         # MVVM ViewModels
├── /Models/             # Data models (POCOs)
├── /Services/           # Business logic services
├── /Data/               # Data access layer
│   └── /Repositories/   # Repository implementations
├── /Utils/              # Helper utilities
├── App.xaml             # Application entry point
└── App.xaml.cs
```

## 🔄 Data Flow

### Report Generation Flow
1. User clicks "Scan Logs" button (View)
2. Command triggers in ViewModel
3. ViewModel calls `LogParserService.ScanAndParse()`
4. Service reads files, parses data, returns models
5. ViewModel updates `ObservableCollection<OperatorStats>`
6. View automatically updates via data binding
7. User clicks "Preview Report"
8. `ReportGeneratorService.GenerateHtml()` creates report
9. Preview window displays HTML

### Contact Lookup Flow
1. User opens Contacts window
2. `ContactsViewModel` loads from `ContactManagerService`
3. Service reads `contacts.json` via `MockDataLoader`
4. Models returned to ViewModel
5. View binds to collection, displays contacts
6. On-call calculation happens in ViewModel
7. Real-time filtering via search box

## 🗄️ Database Schema (SQLite)

### Tables

**employees**
- employee_id (INTEGER PRIMARY KEY)
- username (TEXT UNIQUE)
- full_name (TEXT)
- role (TEXT)
- shift (TEXT)

**training_records**
- record_id (INTEGER PRIMARY KEY)
- employee_id (INTEGER FK)
- module_id (TEXT)
- certified_date (DATETIME)
- expiration_date (DATETIME)
- trainer_username (TEXT)
- notes (TEXT)

**audit_logs**
- log_id (INTEGER PRIMARY KEY)
- timestamp (DATETIME)
- username (TEXT)
- computer_name (TEXT)
- action (TEXT)
- details (TEXT)

**contacts** (may use JSON instead)
- contact_id (INTEGER PRIMARY KEY)
- category (TEXT)
- name (TEXT)
- role (TEXT)
- phone (TEXT)
- email (TEXT)
- on_call_schedule (JSON)

## 🔌 External Integrations

### Outlook COM Automation
```csharp
// Simplified example
var outlook = new Outlook.Application();
var mailItem = outlook.CreateItem(Outlook.OlItemType.olMailItem);
mailItem.To = "qa-manager@company.com";
mailItem.Subject = "Shift Report";
mailItem.HTMLBody = generatedHtml;
mailItem.Display(false); // Show but don't send
```

### File System Access
- Read logs from configured directory
- Read-only access (no modification)
- Handle locked files gracefully

## 🧪 Testing Strategy

### Unit Tests
- Test parsers with sample log files
- Test aggregation logic with known data
- Test date calculations (expiration, on-call)
- Mock external dependencies (file system, Outlook)

### Integration Tests
- Test full report generation flow
- Test database operations
- Test MVVM bindings

### Manual Testing
- UI interaction testing
- Cross-window navigation
- Error handling scenarios

## 🚀 Deployment

### Distribution Options
1. **Standalone Executable:** Single `.exe` with dependencies
2. **Network Share:** Shared location for all QA workstations
3. **ClickOnce:** Auto-updating deployment (future)

### Requirements
- Windows 10/11
- .NET 8.0 Runtime
- Microsoft Outlook (for email feature)
- Write access to local AppData (for database)

## 🔄 Future Architecture Considerations

### Potential Improvements
- **Web Version:** ASP.NET Core with Blazor UI
- **REST API:** Centralized backend for multiple clients
- **Real-time Sync:** Multi-user collaboration
- **Cloud Database:** Azure SQL or Cosmos DB
- **Mobile App:** Xamarin or MAUI for mobile access

### Scalability
- Current design: Single-user desktop app
- Can scale to ~50 users with file-based approach
- Beyond that, needs centralized database

## 📝 Key Design Decisions

| Decision | Rationale |
|----------|-----------|
| WPF over Windows Forms | Modern UI framework, better data binding |
| SQLite over SQL Server | Lightweight, no server required, simple deployment |
| MVVM pattern | Industry standard for WPF, clean separation |
| Mock data for prototype | Allows development without production access |
| COM Interop for Outlook | Leverages existing email infrastructure |
| JSON for configuration | Human-readable, easy to modify |

## 📚 References

- [WPF Documentation](https://docs.microsoft.com/en-us/dotnet/desktop/wpf/)
- [MVVM Pattern](https://docs.microsoft.com/en-us/dotnet/architecture/modernize-desktop/mvvm-pattern)
- [SQLite Documentation](https://www.sqlite.org/docs.html)
- [Outlook Interop](https://docs.microsoft.com/en-us/office/vba/api/overview/outlook)
