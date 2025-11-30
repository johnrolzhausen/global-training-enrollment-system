# Global Training Enrollment System

**A comprehensive Power Platform solution for collecting, validating, and reporting training enrollment volumes across a global network of training centers**

[![Power Platform](https://img.shields.io/badge/Power%20Platform-742774?style=flat&logo=powerapps&logoColor=white)](https://powerapps.microsoft.com/)
[![Power Apps](https://img.shields.io/badge/Power%20Apps-742774?style=flat&logo=powerapps&logoColor=white)](https://powerapps.microsoft.com/)
[![Power Automate](https://img.shields.io/badge/Power%20Automate-0066FF?style=flat&logo=powerautomate&logoColor=white)](https://flow.microsoft.com/)
[![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=flat&logo=powerbi&logoColor=black)](https://powerbi.microsoft.com/)
[![SharePoint](https://img.shields.io/badge/SharePoint-0078D4?style=flat&logo=microsoftsharepoint&logoColor=white)](https://www.microsoft.com/en-us/microsoft-365/sharepoint/collaboration)

---

## 📋 Project Overview

After 20 years in educational services, I built and maintained an enrollment tracking dashboard that served executive leadership for 8 years. When organizational requirements expanded to include training centers not in our centralized database, we attempted to patch together a solution using email-based Excel submissions and Power BI. The result was fragile—breaking whenever centers changed formats, requiring hours of manual data cleanup, and creating reporting delays.

This portfolio project represents what I wish we'd built: a properly architected Power Platform solution that handles data inconsistency gracefully, eliminates manual processing, and provides real-time visibility to stakeholders.

### Business Problem

Global training organizations with independent or franchised training centers face critical data collection challenges:

- **Fragmented Submission Process**: Centers email Excel files in inconsistent formats, creating version control chaos
- **Manual Data Consolidation**: Finance teams spend 15-20 hours monthly cleaning and consolidating data
- **Data Quality Issues**: Format variations, password-protected files, and missing fields cause errors
- **Delayed Reporting**: Multi-week lag between data submission and executive reporting
- **No Submission Visibility**: No centralized view of which centers have submitted or are overdue
- **Limited Historical Analysis**: Difficult to track trends, identify anomalies, or forecast enrollments
- **Scalability Constraints**: Adding new centers or courses requires manual process changes

### Solution

An integrated Power Platform solution that transforms enrollment data collection from a fragmented, manual process into a standardized, automated workflow:

- **Standardized Data Collection Portal**: Training centers submit volumes through user-friendly forms with built-in validation
- **Intelligent Email Processing**: Automated extraction and parsing of Excel attachments for centers unable to use the portal
- **Data Quality Dashboard**: Systematic validation, anomaly detection, and manual review workflow
- **Real-Time Executive Reporting**: Interactive Power BI dashboards with live data and drill-through capabilities
- **Automated Notifications**: Proactive reminders, anomaly alerts, and completion tracking
- **Audit Trail**: Complete history of all submissions, validations, and corrections

---

## 🎯 Key Features

### Enrollment Submission Portal (Canvas App - External)
- **Training Center Login**: Secure access for authorized center administrators
- **Dynamic Submission Forms**: Conditional logic displays relevant fields based on course type and delivery method
- **Built-in Validation**: Real-time checks prevent incomplete or invalid submissions
- **Mobile-Responsive Design**: Accessible from any device globally
- **Draft Capability**: Save partial submissions and complete later
- **Submission History**: View past submissions and confirmation receipts
- **Multi-Language Support**: Interface available in multiple languages for global accessibility

### Email Processing Automation (Power Automate)
- **Shared Mailbox Monitoring**: Continuously scans for enrollment submission emails
- **Attachment Extraction**: Automatically downloads Excel files from emails
- **Password Detection**: Attempts to open password-protected files using common patterns
- **Flexible Schema Detection**: Adapts to minor format variations across submissions
- **Error Logging**: Captures parsing failures with detailed diagnostics
- **Manual Review Queue**: Routes problematic submissions to administrators
- **Acknowledgment Emails**: Confirms receipt and processing status to centers

### Data Quality Dashboard (Canvas App - Internal)
- **Submission Tracking**: Real-time view of all centers and submission status
- **Anomaly Detection**: Flags volumes that deviate significantly from historical patterns
- **Manual Data Entry**: Interface for entering data from phone calls or special cases
- **Validation Workflow**: Approve, reject, or request clarification on submissions
- **Data Correction Tools**: Edit and annotate submissions with audit trail
- **Communication Center**: Send clarification requests directly to training centers
- **Export Functionality**: Download cleaned data for external systems

### Administrator Console (Canvas App - Internal)
- **Center Management**: Maintain training center directory with contact information
- **Course Catalog Administration**: Add, update, or retire course offerings
- **User Access Control**: Manage permissions for center administrators
- **Submission Configuration**: Set deadlines, required fields, and validation rules
- **Report Scheduling**: Configure automated report distribution
- **System Health Monitoring**: View processing statistics and error rates

### Executive Reporting Dashboard (Power BI)
- **Enrollment Overview**: Global enrollment volumes with trend indicators
- **Geographic Analysis**: Interactive map showing enrollments by region and country
- **Course Performance**: Enrollment trends by course track and delivery method
- **Center Comparison**: Performance benchmarking across training centers
- **Capacity Utilization**: Analysis of enrollments vs. center capacity
- **Forecasting**: Predictive analytics for future enrollment trends
- **Data Freshness Indicators**: Clear visibility into last update time
- **Drill-Through Capabilities**: Navigate from summary to transaction-level detail
- **Mobile App**: Power BI mobile for executives on the go

### Automated Workflows (Power Automate)
- **Submission Reminders**: Weekly emails to centers that haven't submitted by deadline
- **Escalation Notifications**: Alert regional managers when centers are consistently late
- **Anomaly Alerts**: Immediate notification when volumes deviate from expected ranges
- **Daily Status Digest**: Morning email showing submission completion percentage
- **Data Quality Reports**: Weekly summary of validation issues and resolution status
- **Executive Summary**: Monthly email with key metrics and insights
- **System Error Alerts**: Notify administrators of processing failures requiring attention

---

## 🏗️ Technical Architecture

### Data Layer (SharePoint)
**Lists and Libraries:**
- **Training Centers**: Master directory with location, contact, timezone, capacity
- **Course Catalog**: Available courses with track, duration, delivery methods
- **Enrollment Submissions**: Volume records with metadata and status tracking
- **Submission History**: Complete audit trail of all submissions and changes
- **Data Quality Flags**: Anomalies, validation issues, and resolution notes
- **Supporting Documents**: Excel files and supporting documentation library
- **User Access**: Center administrator permissions and contact information

**Key Design Principles:**
- Normalized data structure to prevent redundancy
- Lookup columns for referential integrity
- Calculated columns for derived metrics
- Version history enabled for audit compliance
- Indexed columns for query performance

### Application Layer (Power Apps)
**Three Canvas Apps:**
1. **Enrollment Submission Portal** (External-facing)
   - Public-facing for training center administrators
   - Responsive design for mobile and tablet access
   - Offline capability for draft submissions
   - Component library for consistent UI

2. **Data Quality Dashboard** (Internal)
   - Admin-only access for data validation
   - Real-time submission monitoring
   - Workflow-driven review process
   - Advanced filtering and search

3. **Administrator Console** (Internal)
   - Executive and admin access
   - System configuration management
   - User administration
   - Reporting tools

**Design Standards:**
- Delegation-friendly formulas for performance at scale
- Component-based architecture for reusability
- WCAG 2.1 accessibility compliance
- Consistent error handling and user feedback
- Loading indicators and optimistic updates

### Integration Layer (Power Automate)
**Flow Categories:**
1. **Event-Driven Flows**
   - Triggered by new submissions
   - Respond to validation flags
   - Send notifications on state changes

2. **Scheduled Flows**
   - Daily reminder checks
   - Weekly reporting jobs
   - Monthly archival processes

3. **Email Processing Flows**
   - Parse incoming emails
   - Extract and process attachments
   - Handle exceptions and errors

4. **Approval Flows**
   - Multi-stage validation routing
   - Escalation logic for overdue reviews
   - Parallel processing for efficiency

**Flow Design Principles:**
- Comprehensive error handling with retry logic
- Logging for debugging and audit
- Performance optimization for high volume
- Modular design for maintainability

### Analytics Layer (Power BI)
**Data Model:**
- DirectQuery connection to SharePoint for real-time data
- Star schema with fact and dimension tables
- Calculated tables for time intelligence
- DAX measures for complex KPIs

**Visualizations:**
- Executive summary page with key metrics
- Geographic heat map with drill-through
- Trend analysis with forecasting
- Comparative analysis across dimensions
- Data quality scorecard

**Security:**
- Row-level security based on user roles
- Regional managers see only their centers
- Executives have global view
- Finance team has full access

### Security Model
**Role-Based Access:**
- **Training Center Administrator**: Submit and view own center's data
- **Regional Manager**: View all centers in assigned region
- **Data Quality Analyst**: Validate and correct all submissions
- **System Administrator**: Full system access and configuration
- **Executive Leadership**: Read-only access to analytics
- **Finance Team**: Full data access and export capabilities

**Security Implementation:**
- Azure AD authentication
- SharePoint permission inheritance
- Power Apps context variables for role-based UI
- Power BI RLS for data filtering
- Audit logging for compliance

---

## 📊 Data Model

### Core Entities

**Training Centers**
- Center ID (Primary Key)
- Center Name
- Country
- Region
- Time Zone
- Contact Name
- Contact Email
- Status (Active/Inactive)
- Capacity
- Courses Offered (Multi-select lookup)

**Course Catalog**
- Course ID (Primary Key)
- Course Name
- Course Track (Leadership, Technical, Certification, Compliance)
- Duration (Days)
- Delivery Methods (In-Person, Virtual, Hybrid, Self-Paced)
- Status (Active/Retired)
- Minimum Enrollment
- Maximum Enrollment

**Enrollment Submissions**
- Submission ID (Primary Key)
- Training Center (Lookup to Training Centers)
- Course (Lookup to Course Catalog)
- Delivery Method
- Submission Month
- Enrollment Volume
- Submission Date/Time
- Submitted By
- Submission Method (Portal/Email)
- Status (Draft/Submitted/Validated/Flagged/Approved)
- Data Quality Score

**Data Quality Flags**
- Flag ID (Primary Key)
- Submission (Lookup to Enrollment Submissions)
- Flag Type (Anomaly/Missing Data/Format Error)
- Description
- Severity (Low/Medium/High)
- Resolution Status
- Assigned To
- Resolution Notes
- Created Date
- Resolved Date

### Relationships
- Training Centers (1) → Enrollment Submissions (Many)
- Course Catalog (1) → Enrollment Submissions (Many)
- Enrollment Submissions (1) → Data Quality Flags (Many)

### Key Metrics (DAX Calculations)
- Total Enrollments (Sum)
- YoY Growth %
- Average Enrollments per Center
- Capacity Utilization %
- Data Quality Score (Weighted average)
- Submission Completion Rate
- Average Days to Submit

*Detailed entity relationship diagrams available in `/docs/03-data-model.md`*

---

## 🚀 Getting Started

### Prerequisites
- Microsoft 365 account with appropriate licenses:
  - Power Apps per user or per app license
  - Power Automate premium connectors
  - Power BI Pro or Premium Per User license
- SharePoint site with creator permissions
- Power BI Desktop installed locally
- Admin access for security configuration

### Installation Steps

*Detailed step-by-step setup instructions will be provided in `/docs/installation-guide.md`*

**High-Level Process:**
1. Create SharePoint site and configure lists
2. Import Power Apps from solution package
3. Configure Power Automate flows with environment-specific connections
4. Publish Power BI dashboard and configure data refresh
5. Assign user roles and permissions
6. Seed initial data (training centers and course catalog)
7. Conduct user acceptance testing

---

## 📸 Screenshots

*Screenshots will be added progressively as development proceeds*

### Enrollment Submission Portal (Week 2 - Complete)

**Screen 1: Welcome & User Selection**
![Welcome Screen](power-apps/screenshots/01-welcome-screen.png)
*Modern card-based design with user dropdown filtered to active coordinators*

**Screen 2: Submission Form**
![Submission Form](power-apps/screenshots/02-submission-form.png)
*Dynamic test type loading with three-column volume entry (Actual, Budget, Forecast)*

![Submission Form - Estimate Section](power-apps/screenshots/02b-submission-form-estimate.png)
*Conditional estimate reason field with validation*

**Screen 3: Confirmation**
![Confirmation Screen](power-apps/screenshots/03-confirmation-screen.png)
*Success confirmation with complete submission details and next steps*

**SharePoint Integration**
![SharePoint Data](power-apps/screenshots/04-sharepoint-data.png)
![SharePoint Data](power-apps/screenshots/04b-sharepoint-data.png)
*Successful data writes to tbl_VolumeSubmissions with proper lookup formatting*

### Data Quality Dashboard
- Submission tracking overview
- Anomaly detection interface
- Validation workflow
- Communication tools

### Administrator Console
- Center management
- Course catalog administration
- User access control
- System monitoring

### Power BI Dashboard
- Executive summary page
- Geographic enrollment analysis
- Course performance trends
- Center comparison views
- Capacity utilization analysis

### Accessibility Considerations

**WCAG 2.1 Level AA compliance addressed:**
- All interactive controls have descriptive AccessibleLabel properties
- Labels describe both control type and purpose
- Dynamic labels reflect control state (enabled/disabled)
- Screen reader users can navigate the full workflow

**Remaining work (documented for production):**
- Gallery item accessibility (individual row navigation)
- Keyboard shortcuts for common actions
- High contrast theme support
- Focus indicators for keyboard navigation

---

## 📁 Project Structure

```
global-training-enrollment-system/
├── docs/                                    # Project documentation
│   ├── 01-project-overview.md               # Business case and objectives
│   ├── 02-requirements.md                   # Functional and technical requirements
│   ├── 03-data-model.md                     # Entity relationships and schemas
│   ├── 04-user-stories.md                   # Use cases and acceptance criteria
│   └── 05-pain-points-and-lessons-learned.md # Original problem and design decisions
│   └── 06-future-enhancements.md            # Parking lot items for future enhancement
├── power-apps/                              # Canvas app documentation
│   ├── screenshots/                         # UI screenshots & wireframes
│   ├── wireframes/                          # UI wireframes
│   │   ├── wireframes-admin_console.pptx    # Admin Console app
│   │   ├── wireframes-dataquality-portal.pptx # Data Quality Portal app 
│   │   └── wireframes-submission-portal.pptx  # Submission Portal app  
│   └── app-documentation.md                 # Design decisions and formulas
├── power-automate/                          # Flow documentation
│   ├── flow-diagrams/                       # Visual workflow diagrams
│   └── flow-documentation.md                # Flow logic and error handling
├── power-bi/                                # Dashboard documentation
│   ├── screenshots/                         # Dashboard visuals
│   └── dashboard-documentation.md           # Data model and DAX measures
├── sharepoint/                              # Data layer documentation
│   ├── screenshots/                         # UI screenshots & wireframes
│   │   ├── tbl_Countries.png                # Country Listing
│   │   ├── tbl_CountryTestTypes.png         # Junction Table Country to Test types 
│   │   ├── tbl_VolumeSubmissions            # Volume Submission Fact table 
│   │   └── tbl_VolumeSubmissions (2)        # Volume Submission Fact table 
│   └── list-schemas.md                      # List structures and relationships
└── README.md                                # This file
```

---

## 🎓 Learning Objectives

This portfolio project demonstrates proficiency in:

### Power Apps Best Practices
- **Component Architecture**: Reusable UI components across multiple apps
- **Performance Optimization**: Delegation-aware formulas for large datasets
- **User Experience Design**: Intuitive interfaces with contextual help
- **Error Handling Patterns**: Graceful failures with clear user guidance
- **Accessibility Standards**: WCAG 2.1 compliance for inclusive design
- **Mobile Responsiveness**: Adaptive layouts for multiple device types
- **State Management**: Efficient use of context variables and collections

### Power Automate Expertise
- **Complex Workflows**: Multi-stage approval and validation processes
- **Email Processing**: Parsing attachments and extracting structured data
- **Error Handling**: Comprehensive try-catch patterns with logging
- **Performance Tuning**: Parallel processing and optimized actions
- **Integration Patterns**: Connecting multiple data sources seamlessly
- **Monitoring and Debugging**: Proactive error detection and resolution

### Power BI Visualization
- **Data Modeling**: Star schema design with optimized relationships
- **DAX Mastery**: Complex measures for business calculations
- **Interactive Design**: Drill-through, bookmarks, and dynamic filtering
- **Time Intelligence**: Year-over-year, quarter-over-quarter comparisons
- **Performance Optimization**: Query folding and aggregation strategies
- **Row-Level Security**: Dynamic filtering based on user context
- **Mobile Design**: Optimized layouts for Power BI mobile app

### Solution Architecture
- **Requirements Analysis**: Translating business problems into technical solutions
- **Data Modeling**: Normalized schema design for scalability
- **Security Design**: Role-based access control across platforms
- **Integration Strategy**: Seamless data flow between Power Platform services
- **Documentation Standards**: Enterprise-grade technical documentation
- **Scalability Planning**: Designing for growth and increased usage
- **Change Management**: Considering user adoption and training needs

---

## 🛠️ Development Approach

### Phase 1: Planning & Design (Week 1)
- ✅ Repository setup and documentation framework
- ✅ Pain points analysis and lessons learned documentation
- ✅ Detailed requirements gathering
- ✅ Data model design and validation
- ✅ User story creation with acceptance criteria
- ✅ **SharePoint schema design (9 lists documented)** 
- ✅ SharePoint lists creation (9 lists built with 100+ columns, 35 sample records)
- ✅ Wireframes and UI mockups (3 apps 11 screens, 30 slides, comprehensive technical documentation)
- ✅ Technical architecture documentation

### Phase 2: Data Layer & Core Apps (Week 2)
- ✅ SharePoint site and list creation (9 lists, 100+ columns)
- ✅ Sample data generation for testing (35 records across all lists)
- ✅ Enrollment Submission Portal development (3 screens)
  - Screen 1: Welcome/User Selection
  - Screen 2: Submission Form with dynamic test type loading
  - Screen 3: Confirmation with submission details
- ✅ Submission workflows (Submit and Save Draft functionality)
- ✅ SharePoint CRUD operations (ForAll/Patch with complex lookup formats)
- ✅ End-to-end testing with multiple user personas
- ✅ Basic validation and submission workflows
- ✅ Unit testing and debugging
- ✅ Initial Power Automate flows (email notifications)
  - Submission confirmation email to coordinators
  - New submission alert to data quality analyst
- 🔲 Data Quality Dashboard development
- 🔲 Administrator Console development

### Phase 3: Data Quality & Integration (Week 3)
- 🔲 Email processing automation flows
- 🔲 Anomaly detection logic
- 🔲 Advanced notification workflows
- 🔲 Integration testing across apps and flows

### Phase 4: Analytics & Polish (Week 4)
- 🔲 Power BI data model development
- 🔲 Dashboard design and visualization
- 🔲 DAX measures and calculations
- 🔲 UI/UX refinement across all apps
- 🔲 Performance optimization
- 🔲 Documentation completion
- 🔲 Video demonstration recording
- 🔲 Final testing and bug fixes

---

## 📝 Documentation Standards

Throughout development, I'm maintaining:

### Design Decisions Log
- Rationale for architectural choices
- Alternatives considered and why they were rejected
- Trade-offs and their implications
- Performance considerations

### Lessons Learned
- Challenges encountered during development
- Solutions implemented and their effectiveness
- What I would do differently next time
- Best practices discovered through implementation

### Technical Specifications
- Detailed formula documentation
- Flow action configurations
- DAX measure definitions
- Security role definitions

### Testing Documentation
- Test scenarios and acceptance criteria
- Bug tracking and resolution
- Performance testing results
- User acceptance testing outcomes

*All documentation is version-controlled in this repository and updated in real-time as the project evolves.*

---

## 💡 Key Design Decisions

### Why Canvas Apps Over Model-Driven?
Canvas apps provide the flexibility needed for the dynamic, form-heavy interfaces required for enrollment submission. The pixel-perfect control allows for optimized mobile experiences for global users.

### Why DirectQuery vs Import in Power BI?
DirectQuery ensures executives always see real-time data without manual refreshes. For this use case, query performance is acceptable given the moderate data volume.

### Design System Implementation (App.Formulas Theme)

**Decision:** Centralized color palette using App.OnStart instead of inline RGBA values.

### Theme System: Why Collection-Based in Solutions?

**Decision:** Theme stored in collection (colTheme) accessed via LookUp, rather than App.Formulas or App.OnStart variables.

**Why:**
Canvas apps within solutions use delayed loading for performance - variables set in App.OnStart don't initialize until first screen renders. Collections are the exception - they load immediately during startup.

**Issue Encountered:**
Published app showed black backgrounds despite Theme object working perfectly in edit mode. App.Formulas and Set() variables were timing out.

**Solution:**
```powerfx
// App.OnStart
ClearCollect(colTheme, 
    {Name: "Primary", Color: RGBA(0,120,212,1)},
    // ... all theme colors
);

// App.Formulas
GetThemeColor(ColorName:Text):Color=LookUp(colTheme, Name = ColorName).Color;

// Usage in controls
Fill = GetThemeColor("Primary")
```

**Benefits:**
- Guaranteed initialization before first screen renders
- Single source of truth for brand colors
- Works reliably in managed solutions and published apps
- Enterprise-grade pattern used in ALM scenarios

**Learning:**
This is a real-world difference between development/edit mode vs published/production apps. Collections behave differently in the app lifecycle - a critical insight for professional Power Apps development.


**Benefits:**
- Single source of truth for brand colors - update once, changes everywhere
- Automatic color variations using ColorFade() for consistency
- Semantic naming improves code readability
- Enables rapid theme updates without touching individual controls
- Production-ready approach used by enterprise teams

**Why Not Inline Styling:**
Inline RGBA values (e.g., `Fill = RGBA(0,120,212,1)`) create maintenance nightmares when:
- Brand colors change (update 50+ controls manually)
- Inconsistencies creep in (slightly different blues across screens)
- New developers don't know which color to use

### Why SharePoint Lists vs Dataverse?
SharePoint provides sufficient relational capabilities for this scenario while being included in most M365 licenses, reducing deployment friction. Lists also integrate seamlessly with existing document libraries.

### Why Multiple Apps vs Single App with Screens?
Separate apps allow for distinct security contexts (external vs internal) and independent update cycles. This also improves performance by loading only necessary components.

### Why ForAll + Patch Instead of Bulk Operations?

Power Apps doesn't support true bulk insert operations. The ForAll/Patch pattern is the recommended approach for multi-row operations. Key learnings:

- **Lookup column format:** SharePoint requires `{Id: X, Value: "Y"}` format for lookups with additional columns shown
- **Choice column format:** Choice fields require `{Value: "X"}` record format, not plain strings
- **Collection strategy:** Build collection first, then iterate with ForAll for cleaner error handling
- **Debugging technique:** Used Power Automate flow to inspect actual SharePoint schema when formulas failed

This pattern successfully writes 3-15 records per submission (one per test type) with full audit trail.

### Why Conditional "Actual" vs "Estimate" Labeling?

When users check the "IsEstimate" checkbox, the confirmation screen dynamically changes the label from "Actual: 500" to "Estimate: 500" in the test type summary. This:

- Provides immediate visual confirmation that estimate flag was applied
- Reduces user anxiety about whether checkbox was properly submitted
- Aligns confirmation display with actual data state in SharePoint
- Demonstrates attention to UX detail and data accuracy

Implemented via `varActualType` context variable set during submission, consumed in confirmation gallery template.

*Detailed rationale for all architectural decisions documented in `/docs/05-pain-points-and-lessons-learned.md`*

### Email Notification Strategy

**Decision:** Automated email notifications via Power Automate for submission confirmation and analyst alerts.

**Implementation:**
Two event-driven flows trigger on SharePoint list item changes:

1. **Submission Confirmation Flow**
   - Trigger: Item created/modified in tbl_VolumeSubmissions
   - Condition: Status = "Submitted"
   - Action: Send HTML email to coordinator with submission details
   - Includes: Submission ID, country, month, date, next steps

2. **Analyst Alert Flow**
   - Trigger: Same as above
   - Action: Alert data quality analyst (Alice Katt) of new submission
   - Includes: Submission details plus IsEstimate flag for prioritization

**Technical Details:**
- SharePoint trigger: "When an item is created or modified"
- Conditional logic filters to Status = "Submitted" only
- Get item action retrieves country name via lookup
- DateTime formatting via formatDateTime() expression
- HTML email templates with inline CSS for consistent branding

**Why Automated Notifications:**
- Reduces coordinator anxiety (immediate confirmation)
- Enables proactive analyst workflow (no manual checking)
- Provides audit trail (email receipts)
- Supports async work (coordinators can submit anytime)
- Scalable pattern (add more recipients/conditions easily)

**Time Investment:**
Two flows built and tested in ~3 hours, demonstrating Power Automate proficiency alongside Power Apps development.

---

## 🤝 Contributing

This is a portfolio project demonstrating Power Platform capabilities. While it's not intended for production use, I welcome:

- Feedback on design decisions and architecture
- Suggestions for alternative approaches
- Questions about implementation details
- Ideas for extending functionality

Feel free to open an issue or reach out directly.

---

## 📧 Contact

John Rolzhausen 
LinkedIn Profile: https://www.linkedin.com/in/john-rolzhausen-195989217/
Email: john.rolzhausen@live.com  


---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## 🙏 Acknowledgments

- Inspired by 20 years of experience in educational services
- Built to solve real-world data collection challenges I encountered
- Developed using Power Platform best practices and modern architecture patterns
- Created as a demonstration of continuous learning and professional growth

---

## 🔄 Project Evolution

### Version History
- **v0.1.0** (November 19, 2025) - Initial repository setup and documentation framework
- **v0.2.0** (November 20, 2025) - Requirements and design phase complete
- **v0.25.0** (November 22, 2025) - SharePoint data layer complete (9 lists, full schema)
- **v0.3.0** (November 28, 2025) - Submission Portal complete (3 screens, full CRUD operations)
  - Welcome screen with user selection
  - Dynamic submission form with scrollable gallery
  - Confirmation screen with submission details
  - ForAll/Patch integration with SharePoint
  - IsEstimate functionality with conditional validation
  - End-to-end tested with multiple personas
- **v0.3.5** (November 29, 2025) - Power Automate notification flows complete
  - Submission confirmation email to coordinators
  - New submission alert to analyst
  - HTML email templates with professional styling
  - DateTime formatting for user-friendly display
  - Complete end-to-end submission workflow with automation
- **v0.4.0** (Target: Week 3) - Integration and automation complete
- **v1.0.0** (Target: Week 4) - Full solution with analytics and documentation

### What's Different from the Original System
This redesign addresses the specific failures of the previous implementation:

**Original Problem**: Email-based Excel submissions with inconsistent formats
**Solution**: Standardized portal with validation OR intelligent email processing with error handling

**Original Problem**: Manual data cleanup taking hours monthly
**Solution**: Automated validation with systematic review workflow

**Original Problem**: Breaking when centers changed formats
**Solution**: Flexible schema detection and graceful degradation

**Original Problem**: No visibility into submission status
**Solution**: Real-time dashboard with completion tracking

**Original Problem**: Day-old data in reports
**Solution**: Live DirectQuery connection to SharePoint

**Original Problem**: No way to identify anomalies quickly
**Solution**: Automated anomaly detection with configurable thresholds

---

**Project Status**: 🚧 In Active Development  
**Current Phase**: Phase 2 - Data Layer & Core Apps (Complete)  
**Last Updated**: November 30, 2025  
**Days Completed**: 9 of 20  
**Estimated Completion**: December 14, 2025
