# Feature: Contact Directory

## Overview
Quick-access directory for emergency contacts, management, and support personnel.

## Functionality
- Categorized contact list (Emergency, Management, Technical)
- Search/filter contacts
- On-call availability indicator
- Real-time availability calculation

## UI Components
- Grouped ListView by category
- Search TextBox with live filtering
- Visual indicators (colored dots) for availability
- Click-to-call/email actions (simulated)

## Technical Details

### Models
- `Contact` - Contact information
- `OnCallSchedule` - Availability schedule

### Services
- `ContactManagerService` - Contact management

### Data Source
Loads from `mock_data/contacts.json`

## On-Call Logic
Calculates availability based on:
- Current day of week
- Current time
- Configured on-call schedule

### Indicators
- 🟢 Green dot: Available now
- ⚪ Gray dot: Not on-call

## Implemented In
- Sprint 3

## Related Stories
- Story 3.3: Contact directory
- Story 3.4: On-call indicator

## Use Cases
- Emergency contact lookup (3rd shift)
- Finding who's on-call right now
- Quick access to management contacts
