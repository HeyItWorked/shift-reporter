# Feature: Training Management

## Overview
Digital training tracking, certification management, and compliance monitoring.

## Functionality

### For Employees
- View personal certifications
- See expiration dates and status
- Training history

### For Supervisors
- Team compliance overview
- Identify expiring certifications
- Compliance percentage tracking

### For Trainers
- Conduct training with checklist
- Add section notes
- Digital sign-off with dual PINs
- Generate certifications

## UI Components
- Certification list (DataGrid)
- Team compliance view (tab)
- Training session window with checklist
- Sign-off dialog with PIN entry

## Technical Details

### Models
- `Employee` - Employee info and certifications
- `Certification` - Individual certification record
- `TrainingModule` - Training template with checklist
- `TrainingSession` - In-progress training
- `TrainingRecord` - Completed training record

### Services
- `TrainingManagerService` - Training operations

### Data Sources
- `employees.json` - Employee roster
- `training_modules.json` - Training templates
- SQLite database - Training records

## Status Indicators
- 🟢 Valid: More than 30 days to expiration
- 🟡 Expires Soon: Within 30 days
- 🔴 Expired: Past expiration date

## Security
- Dual PIN sign-off (trainee + trainer)
- All completions logged to audit trail
- Automatic timestamp capture

## Compliance
- Digital signatures for FDA 21 CFR Part 11
- Audit trail for ISO 13485
- Training records for GMP

## Implemented In
- Sprint 4

## Related Stories
- Story 4.1: My certifications view
- Story 4.2: Team compliance view
- Story 4.3: Training checklist
- Story 4.4: Digital sign-off
