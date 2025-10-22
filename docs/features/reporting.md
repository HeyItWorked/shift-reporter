# Feature: Shift Reporting

## Overview
Generate and display shift production reports with operator breakdowns.

## Functionality
- Display shift summary (total units, pass rate)
- Show operator breakdown table
- Add optional shift notes
- Preview HTML-formatted report
- Export to Outlook email

## UI Components
- Summary cards with key metrics
- Sortable DataGrid for operator stats
- Multi-line notes field
- Preview window with WebBrowser control

## Technical Details

### Models
- `ShiftReport` - Complete shift data
- `OperatorStats` - Per-operator breakdown

### Services
- `ReportGeneratorService` - HTML generation

### ViewModels
- `MainViewModel` - Report display logic

## Visual Indicators
- Green: Good pass rates (>95%)
- Yellow: Warning (90-95%)
- Red: Issues (<90% or failures)

## Implemented In
- Sprint 2: UI Display
- Sprint 3: Email preview

## Related Stories
- Story 2.2: Summary display
- Story 2.3: Operator table
- Story 2.4: Notes field
- Story 3.1: Email preview
