# Odoo 19 Developer Assessment Task

## Overview
**Time Limit:** 1 Day (8 hours)  
**Odoo Version:** 19.0 Community Edition  
**Module Technical Name:** Choose an appropriate name following Odoo conventions

## Business Context
You are tasked with building a biometric device attendance management system. The system should enable companies to:
- Register and manage attendance capture devices deployed across different locations
- Receive and store attendance punch data from these devices
- Synchronize device records with Odoo's standard attendance module
- Generate reports and export data for analysis
- Handle timezone conversions for multi-location deployments

This assessment simulates real-world integration scenarios you'll encounter working with external hardware systems.

---

## Functional Requirements

### Part 1: Data Models & Business Logic (40%)

You need to design and implement the necessary data structure to support the system. Consider what information needs to be stored and how different entities relate to each other.

#### Device Management
Create a model to represent physical attendance devices. Think about:
- What information identifies a device uniquely?
- How do you connect to a device (connection parameters)?
- Where is the device installed (location tracking)?
- How do you track device status and last communication?
- What computed information would be useful (statistics, counts)?
- What business rules should be enforced (uniqueness, validation)?

**Key Requirements:**
- Device identification and connection details
- Location/company association
- Timezone handling (devices may be in different timezones)
- Active/inactive status management
- Connection status tracking
- Statistical information (computed fields)
- Data validation using both SQL and Python constraints
- Action button to test device connectivity (should update status and notify user)

#### Attendance Records
Create a model to store raw attendance punches from devices. Consider:
- What information comes from the device?
- How do you link device data to Odoo employees?
- How do you handle timezone differences?
- What's the processing workflow for these records?
- How do you track whether a record has been processed?

**Key Requirements:**
- Link to both device and employee
- Timestamp handling (UTC storage + local timezone conversion)
- Punch type identification (in/out)
- Processing status workflow
- Error tracking capability
- Computed fields where appropriate
- Proper ondelete behaviors for relationships

#### Employee Extension
Extend Odoo's employee model to support device integration:
- How do you identify employees on biometric devices?
- Can an employee be registered on multiple devices?
- How can HR managers quickly access device-related data for an employee?

**Implementation Notes:**
- Add necessary fields for device identification
- Create relationships between employees and devices
- Implement navigation features (smart buttons, actions)

#### Attendance Integration
Extend Odoo's standard attendance model:
- How do you differentiate device-generated attendance from manual entries?
- How do you link back to original device records for traceability?

---

### Part 2: User Interface (25%)

Design appropriate views for all your models. Consider what information users need to see and how they'll interact with the system.

#### Device Management Interface
Create a complete CRUD interface for device management:
- List view showing key device information and status
- Detailed form view with logical field grouping
- Visual status indicators (use appropriate widgets and decorations)
- Statistical information display (use stat buttons)
- Action buttons for device operations
- Search and filtering capabilities (by status, location, etc.)

#### Records Management Interface
Create views for attendance records:
- Efficient list view showing relevant information
- Consider making records editable where appropriate
- Color-coding based on processing status
- Detailed form view with all information
- Action buttons for record processing
- Comprehensive search/filter/group options

#### Employee View Enhancement
Enhance the employee form:
- Add device-related fields in a logical location
- Provide quick access to device records
- Use smart buttons to show related data counts

#### Menu Structure
Design an intuitive menu structure under the Attendances app.

---

### Part 3: API & Integration (20%)

Build the integration layer that external devices will use to communicate with Odoo.

#### Data Synchronization Endpoint
Create a JSON API endpoint that devices use to push attendance data.

**Specifications:**
- Choose an appropriate route path
- Accept POST requests with JSON payload
- Payload should include device identification and array of punch records
- Each record contains: employee identifier, timestamp, punch type
- Implement proper authentication (verify device exists and is active)
- Match employees using their device ID
- Create attendance records automatically
- Update device sync timestamp
- Return structured JSON response indicating success/failure with details

**Consider:**
- Error handling (device not found, employee not found, validation errors)
- Bulk processing (multiple records in one request)
- Response format (status, message, counts)

#### Data Export Endpoint
Create an HTTP endpoint for exporting attendance data.

**Specifications:**
- Accept GET requests with query parameters for filtering
- Support filtering by device, date range, etc.
- Generate CSV file with relevant columns
- Return file download with appropriate headers

---

### Part 4: Reporting & SQL (15%)

#### Custom SQL Report
Implement a method that generates an attendance summary using raw SQL (not ORM).

**Requirements:**
- Query must use SQL directly (use `self.env.cr.execute()`)
- Join relevant tables to gather complete information
- Include aggregations (counts, grouping)
- Filter by date range
- Should return summary data including:
  - Device information
  - Employee counts
  - Record statistics (total, by type)
  - Timing information

**Deliverable:**
Create a simple reporting wizard that:
- Accepts date range input
- Triggers the SQL report generation
- Displays results in an appropriate view







---

## Bonus Points (Extra Credit)

If you complete all core requirements ahead of time, consider implementing additional features to demonstrate advanced skills:

### Ideas for Enhancement:
- **Automated Processing**: Set up background jobs to automatically process pending records
- **Data Validation**: Implement business rules to prevent logical errors (e.g., duplicate punches, missing check-outs)
- **Advanced Analytics**: Create dashboards or visual reports showing attendance patterns
- **Notifications**: Alert administrators when devices go offline or errors occur
- **Working Hours Integration**: Cross-reference punches with scheduled working hours

Choose features that showcase your strongest skills or areas you want to highlight.

---

## Technical Requirements

### Code Quality
- Follow Odoo coding guidelines (OCA standards)
- Proper model naming conventions (`model.name.with.dots`)
- Use appropriate field types and attributes
- Add proper help text for fields
- Include docstrings for models and methods
- Handle exceptions properly with user-friendly messages

### Module Structure
Design your module following Odoo best practices. Your module should include:
- Proper folder organization (models, views, controllers, security, data, static)
- Manifest file with appropriate dependencies
- Security definitions (access rights, record rules if needed)
- Demo data (optional but recommended for testing)

Refer to Odoo documentation for standard module structure conventions.

---

## Submission Guidelines

### What to Submit
1. Complete module as a zip file
2. Brief README including:
   - Setup/installation notes
   - Key features you implemented
   - Any design decisions or assumptions you made
   - Known limitations (if any)
3. Instructions for testing the main features

### Before Submitting
Verify your module:
- Installs cleanly without errors
- All core functionality works
- Views render correctly
- API endpoints respond appropriately
- No Python or JavaScript errors in logs
- Security access rights are configured
- Database constraints work as expected

---

## Evaluation Criteria

Your submission will be evaluated based on:

| Category | Weight | Focus Areas |
|----------|--------|-------------|
| **Architecture & Models** | 40% | Data structure design, relationships, business logic, constraints |
| **User Interface** | 25% | View completeness, usability, appropriate widgets, visual design |
| **Integration Layer** | 20% | API functionality, error handling, data processing |
| **Reporting & Queries** | 15% | SQL proficiency, data aggregation, report accuracy |
| **Code Quality** | Bonus | Best practices, documentation, maintainability |

We value well-structured, working code over feature completeness. Quality and problem-solving approach matter more than implementing every detail.

---

## Tips for Success

- **Plan before coding** - Sketch out your models and relationships first
- **Test incrementally** - Verify each component works before moving to the next
- **Use available resources** - Odoo documentation, AI assistants, and similar modules are valuable references
- **Focus on core functionality** - A working subset is better than broken completeness
- **Handle errors gracefully** - Users appreciate clear error messages
- **Document your decisions** - Brief comments explaining "why" help reviewers understand your approach
- **Research similar patterns** - Odoo's hr_attendance module is a good reference

---

## Resources

- Odoo 19 Documentation: https://www.odoo.com/documentation/19.0/
- Odoo Development Tutorials: https://www.odoo.com/documentation/19.0/developer/tutorials.html
- OCA Guidelines: https://github.com/OCA/odoo-community.org/blob/master/website/Contribution/CONTRIBUTING.rst

---

## Questions During Assessment?

If you encounter ambiguity:
- Make reasonable assumptions and document them in your README
- Focus on demonstrating your problem-solving skills
- Simplified but working solutions are valued over complex broken ones

---

## What We're Looking For

Beyond just working code, we want to see:
- How you approach system design and architecture
- Your understanding of Odoo framework patterns and conventions
- Problem-solving methodology
- Code organization and readability
- Ability to make pragmatic technical decisions

**Good luck! Show us how you build solutions! 🚀**
