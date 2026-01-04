# Odoo 19 Developer Assessment Task

## Overview
**Time Limit:** 3-4 hours  
**Odoo Version:** 19.0 Community Edition  
**Module Technical Name:** `biometric_attendance`

## Business Context

Build a biometric device attendance management system that allows companies to:
- Register and manage attendance capture devices
- Receive and store attendance punch data from devices
- Process and validate attendance records

---

## Requirements

### Part 1: Data Models (50%)

#### 1.1 Device Model (`biometric.device`)

Create a model to represent physical attendance devices with:

| Field | Type | Description |
|-------|------|-------------|
| `name` | Char | Device name (required) |
| `serial_number` | Char | Unique identifier |
| `ip_address` | Char | Device IP address |
| `port` | Integer | Connection port (default: 4370) |
| `location` | Char | Physical location description |
| `company_id` | Many2one | Link to company |
| `active` | Boolean | Active/Inactive status |
| `last_sync` | Datetime | Last successful sync timestamp |
| `state` | Selection | Status: draft/connected/disconnected |
| `record_count` | Integer | Computed: total attendance records |

**Requirements:**
- SQL constraint: unique `serial_number`
- Python constraint: validate IP address format
- Action button: "Test Connection" (simulated - just updates `state` and shows notification)

#### 1.2 Attendance Record Model (`biometric.attendance.record`)

Create a model to store raw attendance punches:

| Field | Type | Description |
|-------|------|-------------|
| `device_id` | Many2one | Link to device (required, cascade delete) |
| `employee_id` | Many2one | Link to employee (required) |
| `punch_time` | Datetime | When the punch occurred (required) |
| `punch_type` | Selection | `in` or `out` |
| `device_user_id` | Char | Employee ID on the device |
| `state` | Selection | `pending` / `processed` / `error` |
| `error_message` | Text | Error details if state is error |
| `name` | Char | Computed: display name |

**Requirements:**
- Proper `ondelete` behaviors
- Default state: `pending`
- Action button: "Mark as Processed"

---

### Part 2: User Interface (30%)

#### 2.1 Device Views
- **List view:** Show name, serial_number, location, state, last_sync
- **Form view:** All fields, organized in groups, state displayed as status badge
- **Search view:** Filter by state, group by location

#### 2.2 Attendance Record Views
- **List view:** Show device, employee, punch_time, punch_type, state
- **Form view:** All fields (read-only where appropriate)
- **Search view:** Filter by state, device; group by device, punch_type

#### 2.3 Menu Structure
Create menus under **Attendances** app:
```
Attendances
└── Biometric Devices
    ├── Devices
    └── Attendance Records
```

---

### Part 3: API Integration (20%)

#### Sync Endpoint

Create a JSON API endpoint for devices to push attendance data.

**Route:** `POST /api/biometric/sync`

**Request Payload:**
```json
{
  "serial_number": "DEV001",
  "records": [
    {
      "device_user_id": "EMP001",
      "punch_time": "2025-01-04 08:30:00",
      "punch_type": "in"
    }
  ]
}
```

**Response (Success):**
```json
{
  "status": "success",
  "message": "Synced 1 records",
  "created": 1,
  "errors": 0
}
```

**Response (Error):**
```json
{
  "status": "error",
  "message": "Device not found"
}
```

**Requirements:**
- Validate device exists and is active
- Create attendance records for each punch
- Update device `last_sync` timestamp
- Return appropriate error messages

---

## Module Structure

```
biometric_attendance/
├── __manifest__.py
├── __init__.py
├── models/
│   ├── __init__.py
│   ├── biometric_device.py
│   └── biometric_attendance_record.py
├── views/
│   ├── biometric_device_views.xml
│   ├── biometric_attendance_record_views.xml
│   └── menu.xml
├── controllers/
│   ├── __init__.py
│   └── api.py
├── security/
│   └── ir.model.access.csv
└── README.md
```

---

## Security

Create access rights for:
- `biometric.device`: Full CRUD for HR Manager
- `biometric.attendance.record`: Full CRUD for HR Manager

---

## Evaluation Criteria

| Category | Weight | Focus |
|----------|--------|-------|
| **Data Models** | 50% | Correct field types, constraints, relationships |
| **User Interface** | 30% | Functional views, proper widgets, usability |
| **API Integration** | 20% | Working endpoint, error handling, validation |

---

## Submission

1. Complete module as a zip file
2. Brief notes on any assumptions made
3. Ensure module installs without errors

---

## Tips

- Focus on **working code** over feature completeness
- Test your module installs cleanly
- Use Odoo's `hr_attendance` module as reference
- Handle errors gracefully with clear messages

**Good luck! 🚀**
