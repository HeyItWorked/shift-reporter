# Shift Reporter

A desktop application prototype for automating QA shift reporting in medical device manufacturing environments. Designed for AED testing facilities to streamline end-of-shift reporting, contact management, and training compliance tracking.

[![.NET](https://img.shields.io/badge/.NET-8.0-512BD4?logo=dotnet)](https://dotnet.microsoft.com/)
[![WPF](https://img.shields.io/badge/UI-WPF-0078D4?logo=windows)](https://github.com/dotnet/wpf)
[![License](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)

> **⚠️ Note:** This is a prototype project using mock data. Not intended for production use without proper validation and regulatory compliance review.

---

## 📋 Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Technology Stack](#technology-stack)
- [Architecture](#architecture)
- [Getting Started](#getting-started)
- [Project Structure](#project-structure)
- [Mock Data](#mock-data)
- [Security Model](#security-model)
- [Development Roadmap](#development-roadmap)
- [Contributing](#contributing)
- [License](#license)

---

## 🎯 Overview

### The Problem

In medical device QA environments, shift leads must manually:

- Count devices tested by each operator from ZAS log files
- Log into company portal to submit production numbers
- Look up contact information during emergencies (especially 3rd shift)
- Track training certifications and expiration dates
- Document everything for FDA compliance

This manual process takes 10-15 minutes per shift and is error-prone.

### The Solution

Shift Reporter automates this workflow:

1. **Scans** ZAS log files automatically
2. **Parses** device test data and aggregates by operator
3. **Displays** results in an easy-to-review dashboard
4. **Generates** professional email reports with Outlook integration
5. **Provides** quick access to vital contacts with on-call awareness
6. **Tracks** training certifications and compliance status

**Time savings:** ~10 minutes per shift = 250 hours/year for 3 shifts
**Error reduction:** Eliminates manual counting and data entry mistakes

---

## ✨ Features

### Core Functionality

#### 📊 Automated Shift Reporting

- Scans ZAS log directory for completed test sessions
- Parses device serial numbers, operators, test results, timestamps
- Aggregates production statistics by operator
- Identifies failed tests with reason codes
- Generates shift summary with pass/fail rates
- Human verification before sending

#### 📧 Outlook Integration

- Pre-fills email TO/CC/SUBJECT fields
- Rich HTML formatting with tables and styling
- Launches Outlook for final review and send
- No automatic sending - user maintains control

#### 📞 Vital Contact Directory

- Emergency contacts (Security, Medical, Safety)
- Management on-call schedule (auto-calculates current shift)
- Technical support (IT, Biomedical Engineering)
- Shift-to-shift handoff contacts
- Smart filtering by availability and role
- One-click call/email actions (simulated in prototype)

#### 📋 Training Management

- Employee certification tracking
- Training module library with checklists
- Digital sign-off with PIN verification
- Expiration alerts and notifications
- Compliance dashboard for supervisors
- Audit-ready documentation

### Security & Compliance

- Windows username logging (automatic)
- Comprehensive audit trail
- PIN-based approval for critical actions
- Role-based access control
- Read-only access to source logs
- Append-only audit logs
- FDA 21 CFR Part 11 considerations

---

## 🛠️ Technology Stack

### Core Technologies

- **Language:** C# 12
- **Framework:** .NET 8.0
- **UI Framework:** WPF (Windows Presentation Foundation)
- **Database:** SQLite 3
- **JSON Parsing:** Newtonsoft.Json
- **MVVM:** CommunityToolkit.Mvvm
- **Email Integration:** Microsoft.Office.Interop.Outlook (COM)

### Development Tools

- **IDE:** Visual Studio 2022 Community
- **Version Control:** Git
- **Database Viewer:** DB Browser for SQLite
- **Testing:** MSTest / xUnit (TBD)

### Target Platform

- **OS:** Windows 10/11
- **Requirements:** .NET 8.0 Runtime, Microsoft Outlook (for email feature)
- **Deployment:** Standalone executable or network share

---

## 🏗️ Architecture

### High-Level Architecture

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

### Design Patterns

- **MVVM** (Model-View-ViewModel) for UI separation
- **Repository Pattern** for data access abstraction
- **Factory Pattern** for object creation
- **Observer Pattern** for notifications and alerts

---

## 🚀 Getting Started

### Prerequisites

1. **Windows 10/11** (64-bit)
2. **Visual Studio 2022** (Community Edition or higher)
   - Workload: .NET Desktop Development
3. **Microsoft Outlook** (for email integration feature)
4. **.NET 8.0 SDK** (included with Visual Studio)

### Installation

```bash
# Clone the repository
git clone https://github.com/yourusername/shift-reporter.git
cd shift-reporter

# Open in Visual Studio
start ShiftReporter.sln

# Restore NuGet packages
# (Visual Studio will prompt automatically)

# Build the solution
# Build > Build Solution (Ctrl+Shift+B)

# Run the application
# Debug > Start Debugging (F5)
```

### NuGet Package Installation

If packages don't restore automatically:

```powershell
# In Package Manager Console (Tools > NuGet Package Manager)
Install-Package System.Data.SQLite
Install-Package Newtonsoft.Json
Install-Package CommunityToolkit.Mvvm
Install-Package MaterialDesignThemes  # Optional for UI styling
```

### Quick Start

1. **Launch the application** - Run from Visual Studio or execute `ShiftReporter.exe`
2. **Mock data loads automatically** - Sample ZAS logs and contacts pre-configured
3. **Click "Scan Logs"** - Parses mock log files from `/mock_data/zas_logs/`
4. **Review report** - Check operator statistics and test results
5. **Add notes** - Optional comments about the shift
6. **Click "Preview Email"** - See what would be sent
7. **Click "Open in Outlook"** - Launches Outlook with pre-filled email

---

## 📁 Project Structure

```
ShiftReporter/
│
├── /src/
│   ├── /ShiftReporter/              # Main WPF application
│   │   ├── /Views/                      # XAML windows and user controls
│   │   │   ├── MainWindow.xaml
│   │   │   ├── ContactsWindow.xaml
│   │   │   ├── TrainingWindow.xaml
│   │   │   └── PinDialog.xaml
│   │   │
│   │   ├── /ViewModels/                 # View models (MVVM)
│   │   │   ├── MainViewModel.cs
│   │   │   ├── ContactsViewModel.cs
│   │   │   └── TrainingViewModel.cs
│   │   │
│   │   ├── /Models/                     # Data models (POCOs)
│   │   │   ├── Employee.cs
│   │   │   ├── DeviceTestLog.cs
│   │   │   ├── Contact.cs
│   │   │   ├── ShiftReport.cs
│   │   │   └── TrainingRecord.cs
│   │   │
│   │   ├── /Services/                   # Business logic
│   │   │   ├── LogParserService.cs
│   │   │   ├── ReportGeneratorService.cs
│   │   │   ├── ContactManagerService.cs
│   │   │   ├── TrainingManagerService.cs
│   │   │   ├── SecurityService.cs
│   │   │   └── AuditLoggerService.cs
│   │   │
│   │   ├── /Data/                       # Data access
│   │   │   ├── DatabaseContext.cs
│   │   │   ├── MockDataLoader.cs
│   │   │   └── /Repositories/
│   │   │       ├── IRepository.cs
│   │   │       ├── EmployeeRepository.cs
│   │   │       └── AuditLogRepository.cs
│   │   │
│   │   ├── /Utils/                      # Utilities
│   │   │   ├── OutlookIntegration.cs
│   │   │   ├── FileSystemHelper.cs
│   │   │   └── HtmlGenerator.cs
│   │   │
│   │   ├── App.xaml                     # Application entry point
│   │   └── App.xaml.cs
│   │
│   └── /ShiftReporter.Tests/        # Unit tests
│       ├── LogParserTests.cs
│       ├── ReportGeneratorTests.cs
│       └── SecurityTests.cs
│
├── /mock_data/                           # Sample data for prototype
│   ├── /zas_logs/                       # Mock ZAS log files
│   │   ├── 20251022_223015_X25K45892.log
│   │   ├── 20251022_224512_X25K45893.log
│   │   └── ... (20+ sample files)
│   │
│   ├── contacts.json                    # Mock contact directory
│   ├── employees.json                   # Mock employee roster
│   ├── training_modules.json            # Training templates
│   └── config.json                      # Application configuration
│
├── /database/                           # Local SQLite database
│   ├── schema.sql                       # Database schema
│   └── prototype.db                     # SQLite database file
│
├── /docs/                               # Documentation
│   ├── ARCHITECTURE.md                  # Architecture details
│   ├── SECURITY.md                      # Security model
│   ├── API.md                           # Code documentation
│   ├── /screenshots/                    # UI screenshots
│   └── /design/                         # Design mockups
│
├── .gitignore
├── LICENSE
└── README.md
```

---

## 🗂️ Mock Data

This prototype uses realistic mock data for demonstration purposes.

### ZAS Log Files

**Location:** `/mock_data/zas_logs/`

Sample log structure:

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

**Includes:**

- 25+ sample log files per shift
- Mix of PASS (95%) and FAIL (5%) results
- 3 different operators
- Realistic timestamps across 3rd shift (10 PM - 6 AM)
- Various device serial numbers

### Contact Directory

**Location:** `/mock_data/contacts.json`

**Categories:**

- Emergency contacts (911, Security, Safety)
- Management (Shift Supervisors, QA Manager)
- Technical Support (IT, Biomedical Engineering)
- Other shifts (1st, 2nd shift leads)

**Features:**

- On-call schedules by day of week
- Available hours
- Multiple contact methods (phone, email, extension)

### Employee Data

**Location:** `/mock_data/employees.json`

**Sample employees:**

- John Smith (Shift Lead, 3rd shift)
- Amy Lee (QA Technician, 3rd shift)
- Brian Wong (QA Technician, 3rd shift)

**Each includes:**

- Employee ID
- Name, username, role
- Shift assignment
- Training certifications with expiration dates

### Training Modules

**Location:** `/mock_data/training_modules.json`

**Available modules:**

- ZAS Basic Operation (4 hours, annual renewal)
- AED Plus Testing (3 hours, annual renewal)
- Language Configuration (2 hours, annual renewal)
- Troubleshooting Level 1 (4 hours, biannual renewal)
- GMP for Medical Devices (2 hours, annual renewal)

---

## 🔒 Security Model

### Overview

This prototype demonstrates a **"trust but verify"** security model appropriate for shared workstations in controlled manufacturing environments.

### Security Layers

#### 1. Physical Access Control

- Assumes facility has badge access to QA area
- Workstation in secure, camera-monitored location
- Existing company IT policies apply

#### 2. Windows Identity

- Leverages Windows login (no separate authentication)
- Automatic capture of `Environment.UserName`
- Cannot be spoofed without Windows admin access

#### 3. Role-Based Access

- Config file defines shift leads, supervisors, QA techs
- UI adapts based on current user's role
- Actions enabled/disabled accordingly

```json
// Example: config.json
{
  "shift_leads": ["jsmith", "alee", "bwong"],
  "supervisors": ["mjohnson", "schen"]
}
```

#### 4. PIN Verification

- Simple 4-digit PIN for critical actions
- Prevents accidental clicks
- Not cryptographic security (audit log is real security)

#### 5. Comprehensive Audit Trail

- Every action logged with timestamp, user, computer
- Append-only log file (tamper-evident)
- Includes: report sent, contacts viewed, training records modified

```
Example audit entry:
2025-10-22T22:35:00 | COMPANY\jsmith | QA-STATION-03 | ReportSent | 42 units
```

#### 6. Outlook Review Step

- User manually reviews and sends from Outlook
- Final human checkpoint before email goes out

### What This Protects Against

✅ Accidental misuse
✅ Unauthorized personnel
✅ Lost audit trail
✅ Data entry errors

### What This Assumes

- Physical security exists
- Determined malicious insider will be caught by audit log
- Windows authentication is managed by IT
- Outlook is already secured by company email policies

### Compliance Considerations

- **FDA 21 CFR Part 11:** Audit trail, electronic signatures (PIN), data integrity
- **ISO 13485:** Quality management documentation
- **GMP:** Good Manufacturing Practice record-keeping

---

## 🗓️ Development Roadmap

### Phase 1: Foundation (Weeks 1-2) ✅

- [X] Project setup and structure
- [X] Mock data creation (ZAS logs, contacts, employees)
- [X] Basic UI skeleton (main window, navigation)
- [X] Log file parser implementation
- [X] Data aggregation logic

### Phase 2: Core Features (Weeks 3-4)

- [ ] Report generation and display
- [ ] Outlook integration (COM automation)
- [ ] Contact directory UI and logic
- [ ] On-call schedule calculation
- [ ] Basic training status view

### Phase 3: Advanced Features (Weeks 5-6)

- [ ] Training module checklist system
- [ ] Digital sign-off with PIN
- [ ] Certification expiration alerts
- [ ] Supervisor compliance dashboard
- [ ] Enhanced reporting with charts

### Phase 4: Security & Polish (Weeks 7-8)

- [ ] Security manager implementation
- [ ] Audit logging system
- [ ] Role-based access control
- [ ] UI styling and polish (Material Design)
- [ ] Error handling and validation
- [ ] Help documentation

### Phase 5: Testing & Documentation (Weeks 9-10)

- [ ] Unit test coverage (>80%)
- [ ] Integration testing
- [ ] User acceptance testing
- [ ] Performance optimization
- [ ] Complete documentation
- [ ] Demo video creation

### Future Enhancements (Post-MVP)

- [ ] Real ZAS integration (production)
- [ ] PDF export of reports
- [ ] Email template customization
- [ ] Multi-facility support
- [ ] Web-based dashboard version
- [ ] Mobile notifications
- [ ] Analytics and trends

---

## 🤝 Contributing

### Development Guidelines

1. **Branch naming:** `feature/description`, `bugfix/description`, `docs/description`
2. **Commit messages:** Use conventional commits format

```
   feat: add contact search functionality
   fix: resolve null reference in log parser
   docs: update architecture diagram
```

3. **Code style:** Follow C# coding conventions, use StyleCop
4. **Testing:** Write unit tests for new features
5. **Documentation:** Update README and inline comments

### Pull Request Process

1. Fork the repository
2. Create a feature branch
3. Make your changes with tests
4. Update documentation
5. Submit PR with clear description
6. Address review feedback

### Code Review Checklist

- [ ] Code compiles without warnings
- [ ] Unit tests pass
- [ ] No hardcoded paths or credentials
- [ ] Follows MVVM pattern
- [ ] Audit logging included for new actions
- [ ] Error handling implemented
- [ ] Comments explain "why" not "what"

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

### Third-Party Licenses

- .NET - [MIT License](https://github.com/dotnet/runtime/blob/main/LICENSE.TXT)
- Newtonsoft.Json - [MIT License](https://github.com/JamesNK/Newtonsoft.Json/blob/master/LICENSE.md)
- SQLite - [Public Domain](https://www.sqlite.org/copyright.html)
- Material Design - [MIT License](https://github.com/MaterialDesignInXAML/MaterialDesignInXamlToolkit/blob/master/LICENSE)

---

## 🙏 Acknowledgments

- Inspired by real-world challenges in medical device QA
- Medical for their excellent AED products (this is an independent project)
- Medical device manufacturing professionals who provided insights
- Open-source community for excellent tools and libraries

---

## 📞 Contact & Support

**Project Maintainer:** [Your Name]
**Email:** your.email@example.com
**GitHub:** [@yourusername](https://github.com/yourusername)

### Getting Help

- **Bug reports:** [Open an issue](https://github.com/yourusername/-shift-reporter/issues)
- **Feature requests:** [Open an issue](https://github.com/yourusername/-shift-reporter/issues)
- **Questions:** [Discussions tab](https://github.com/yourusername/-shift-reporter/discussions)

---

## 📊 Project Status

**Current Version:** 0.1.0-alpha (Prototype)
**Status:** Active Development
**Last Updated:** October 2025

### Metrics

- **Lines of Code:** ~2,500 (estimated at completion)
- **Test Coverage:** Target 80%+
- **Documentation:** In progress
- **Contributors:** 1 (seeking contributors!)

---

## 🎯 Project Goals

### Learning Objectives

- Master WPF and MVVM pattern
- Practice enterprise application architecture
- Understand medical device quality systems
- Implement real-world security considerations
- Build portfolio-worthy project

### Business Objectives

- Reduce shift reporting time by 80%
- Eliminate manual data entry errors
- Improve 3rd shift supervisor access to contacts
- Streamline training compliance tracking
- Create audit trail for regulatory compliance

---

Made with ❤️ for medical device QA professionals
