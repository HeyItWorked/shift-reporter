# Shift Reporter - Project Overview

A desktop application prototype for automating QA shift reporting in medical device manufacturing environments.

[![.NET](https://img.shields.io/badge/.NET-8.0-512BD4?logo=dotnet)](https://dotnet.microsoft.com/)
[![WPF](https://img.shields.io/badge/UI-WPF-0078D4?logo=windows)](https://github.com/dotnet/wpf)

> **⚠️ Note:** This is a prototype project using mock data. Not intended for production use.

## 🎯 The Problem

In medical device QA environments, shift leads must manually:
- Count devices tested by each operator from log files
- Log into company portal to submit production numbers
- Look up contact information during emergencies (especially 3rd shift)
- Track training certifications and expiration dates
- Document everything for FDA compliance

**This manual process takes 10-15 minutes per shift and is error-prone.**

## 💡 The Solution

Shift Reporter automates this workflow:
1. **Scans** log files automatically
2. **Parses** device test data and aggregates by operator
3. **Displays** results in an easy-to-review dashboard
4. **Generates** professional email reports with Outlook integration
5. **Provides** quick access to vital contacts with on-call awareness
6. **Tracks** training certifications and compliance status

**Time savings:** ~10 minutes per shift = 250 hours/year for 3 shifts  
**Error reduction:** Eliminates manual counting and data entry mistakes

## 🛠️ Technology Stack

- **Language:** C# 12
- **Framework:** .NET 8.0
- **UI Framework:** WPF (Windows Presentation Foundation)
- **Database:** SQLite 3
- **JSON Parsing:** Newtonsoft.Json
- **MVVM:** CommunityToolkit.Mvvm
- **Email Integration:** Microsoft.Office.Interop.Outlook (COM)

## 📅 Timeline

**Total Duration:** 10 weeks (5 sprints × 2 weeks)  
**Estimated Completion:** 111 story points

## 📁 Documentation Structure

```
/docs/
├── PROJECT_OVERVIEW.md (this file)
├── ARCHITECTURE.md
├── SECURITY.md
├── /features/
│   ├── log-parsing.md
│   ├── reporting.md
│   ├── contacts.md
│   ├── training.md
│   └── outlook-integration.md
└── /sprints/
    ├── sprint-0-setup.md
    ├── sprint-1-log-parser.md
    ├── sprint-2-ui-display.md
    ├── sprint-3-outlook-contacts.md
    ├── sprint-4-training.md
    └── sprint-5-polish.md
```

## 🎯 Project Goals

### Learning Objectives
- Master WPF and MVVM pattern
- Practice enterprise application architecture
- Understand medical device quality systems
- Implement real-world security considerations

### Business Objectives
- Reduce shift reporting time by 80%
- Eliminate manual data entry errors
- Improve 3rd shift supervisor access to contacts
- Streamline training compliance tracking
- Create audit trail for regulatory compliance

## 📞 Contact

See main README.md for contact information and support.
