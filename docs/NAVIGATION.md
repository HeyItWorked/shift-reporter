# Shift Reporter - Documentation Navigation

## 📁 Quick Start

**New to the project?** Start here:
1. [Project Overview](PROJECT_OVERVIEW.md)
2. [Architecture](ARCHITECTURE.md)
3. [Security Model](SECURITY.md)

---

## 🗂️ Documentation Structure

### Core Documentation
- **[PROJECT_OVERVIEW.md](PROJECT_OVERVIEW.md)** - High-level project description, goals, tech stack
- **[ARCHITECTURE.md](ARCHITECTURE.md)** - System architecture, design patterns, data flow
- **[SECURITY.md](SECURITY.md)** - Security model, compliance, best practices

### Sprint Plans
Detailed 2-week sprint breakdowns with user stories, tasks, and acceptance criteria:

- **[Sprint 0: Setup](sprints/sprint-0-setup.md)** - Development environment setup
- **[Sprint 1: Log Parser](sprints/sprint-1-log-parser.md)** - Foundation & parsing logic (21 pts)
- **[Sprint 2: UI Display](sprints/sprint-2-ui-display.md)** - Basic UI & report display (23 pts)
- **[Sprint 3: Outlook & Contacts](sprints/sprint-3-outlook-contacts.md)** - Email integration & directory (24 pts)
- **[Sprint 4: Training](sprints/sprint-4-training.md)** - Training management system (22 pts)
- **[Sprint 5: Polish](sprints/sprint-5-polish.md)** - Security, polish, documentation (21 pts)

### Feature Documentation
Deep dives into specific features:

- **[Log Parsing](features/log-parsing.md)** -  log file parsing and aggregation
- **[Reporting](features/reporting.md)** - Shift report generation and display
- **[Outlook Integration](features/outlook-integration.md)** - COM automation for email
- **[Contacts](features/contacts.md)** - Contact directory with on-call awareness
- **[Training](features/training.md)** - Training tracking and compliance

---

## 🎯 Find Information By...

### By Role

**I'm a Developer:**
- Start with [Architecture](ARCHITECTURE.md)
- Review [Sprint Plans](sprints/) for implementation details
- Check [Feature Docs](features/) for specific functionality

**I'm a Project Manager:**
- Read [Project Overview](PROJECT_OVERVIEW.md)
- Review sprint summaries for timeline
- Check velocity tracking in sprint files

**I'm a QA Tester:**
- Review acceptance criteria in [Sprint Plans](sprints/)
- Check [Feature Docs](features/) for expected behavior
- See [Security Model](SECURITY.md) for security requirements

**I'm a Stakeholder:**
- Start with [Project Overview](PROJECT_OVERVIEW.md)
- Review goals and business objectives
- Check project timeline (10 weeks, 5 sprints)

### By Topic

**Setup & Installation:**
- [Sprint 0: Setup](sprints/sprint-0-setup.md)

**Architecture & Design:**
- [Architecture](ARCHITECTURE.md)
- [Design Patterns](ARCHITECTURE.md#design-patterns)

**Security & Compliance:**
- [Security Model](SECURITY.md)
- [FDA 21 CFR Part 11](SECURITY.md#compliance-considerations)

**Specific Features:**
- See [Feature Documentation](features/) section

**Development Timeline:**
- See [Sprint Plans](sprints/) section

---

## 📊 Project Stats

- **Total Duration:** 10 weeks (5 sprints)
- **Total Story Points:** 111
- **Features:** 5 major features
- **Sprints:** 5 × 2-week sprints
- **Tech Stack:** C# 12, .NET 8.0, WPF, SQLite

---

## 🔗 External Resources

- [WPF Documentation](https://docs.microsoft.com/en-us/dotnet/desktop/wpf/)
- [MVVM Pattern](https://docs.microsoft.com/en-us/dotnet/architecture/modernize-desktop/mvvm-pattern)
- [Outlook Interop](https://docs.microsoft.com/en-us/office/vba/api/overview/outlook)
- [SQLite Documentation](https://www.sqlite.org/docs.html)

---

## 📝 Document Status

| Document | Status | Last Updated |
|----------|--------|--------------|
| PROJECT_OVERVIEW.md | ✅ Complete | 2025-10-22 |
| ARCHITECTURE.md | ✅ Complete | 2025-10-22 |
| SECURITY.md | ✅ Complete | 2025-10-22 |
| Sprint 0-5 | ✅ Complete | 2025-10-22 |
| Feature Docs | ✅ Complete | 2025-10-22 |

---

## 💡 Tips

- **First time here?** Read documents in order: Overview → Architecture → Sprint 0 → Sprint 1
- **Starting development?** Begin with Sprint 0 setup tasks
- **Looking for specific feature?** Check feature documentation first
- **Planning timeline?** Review sprint summaries for point estimates
- **Need technical details?** Check Architecture and feature docs

---

**Ready to start?** Begin with [Sprint 0: Setup](sprints/sprint-0-setup.md)!
