# Requirements

# Requirements Document
## Global Training Enrollment System

**Version:** 1.0  
**Date:** November 20, 2025  
**Author:** John Rolzhausen  
**Status:** Draft - In Review

---

## 1. Executive Summary

This document defines the functional and non-functional requirements for the Global Training Enrollment System - a Power Platform solution designed to replace a fragmented, email-based volume collection process with a standardized, automated workflow.

**Primary Objective:** Eliminate manual data consolidation that currently requires 3+ hours monthly while providing real-time visibility to finance leadership.

**Success Criteria:**
- Reduce data consolidation time from 3 hours to 15 minutes
- Enable real-time dashboard access (vs. month-end batch reporting)
- Eliminate manual password removal and format corrections
- Achieve 100% submission by 3rd business day with automated reminders
- Maintain TM1 integration via automated .csv export

---

## 2. Stakeholder Requirements

### 2.1 Country Volume Coordinators
**Primary Users:** Representatives in each country who submit consolidated enrollment volumes

**Key Needs:**
- Quick submission process (under 15 minutes per month)
- Clear guidance on what data is required
- Ability to save drafts and complete later
- Confirmation that submission was received
- Historical view of past submissions
- Secure access without email-based passwords

**Pain Points Being Solved:**
- Current Excel format variations cause submission anxiety
- No confirmation of receipt
- Password management frustration (especially Korea monthly changes)
- No way to verify historical submissions

---

### 2.2 Data Quality Analyst (John's Role)
**Primary User:** Finance analyst responsible for data validation and consolidation

**Key Needs:**
- Real-time visibility into submission status by country
- Automated validation to catch errors immediately
- Ability to manually correct submissions with audit trail
- Tools to communicate with coordinators about issues
- Efficient workflow to review and approve submissions
- Export capability for TM1 integration

**Pain Points Being Solved:**
- Currently spends 3 hours on 3rd business day manually fixing formats
- No systematic way to identify anomalies
- Late submissions require repeating manual work
- Excel format changes break Power BI refreshes
- Password-protected files require manual intervention

---

### 2.3 Finance Leadership (CFO, Finance Team)
**Primary Users:** Executive stakeholders who consume volume reports for decision-making

**Key Needs:**
- Real-time access to submitted volumes (not month-end batch)
- Interactive dashboards with drill-down by country, region, test type
- Trend analysis and variance reporting (Actual vs Budget vs Forecast)
- Mobile access for on-the-go review
- Data freshness indicators
- Export capability for external presentations

**Pain Points Being Solved:**
- Currently must wait until 3rd business day + processing time for data
- No ability to see partial submissions mid-month
- Limited interactivity (static Excel reports)
- Cannot drill down to country-level detail easily

---

### 2.4 System Administrators
**Primary Users:** IT/admin staff responsible for system maintenance

**Key Needs:**
- User access management
- Test type and country configuration
- Submission deadline configuration
- System health monitoring
- Error log review
- Backup and recovery capabilities

---

## 3. Functional Requirements

### 3.1 Volume Submission Portal (Power App - External)

#### 3.1.1 Authentication & Access
**REQ-101:** System shall authenticate users via Azure AD
**REQ-102:** System shall assign country-specific access based on user profile
**REQ-103:** System shall support role-based permissions (Coordinator, Analyst, Admin, Executive)
**REQ-104:** System shall log all access attempts for security audit

#### 3.1.2 Submission Interface
**REQ-201:** System shall present a dynamic form based on user's assigned country
**REQ-202:** System shall support submission of three volume types: Actual, Budget, Forecast
**REQ-203:** System shall allow selection of submission month/year
**REQ-204:** System shall display all relevant test types for the selected country
**REQ-205:** System shall validate that volume entries are numeric and non-negative
**REQ-206:** System shall allow decimal values for volume entries
**REQ-207:** System shall auto-save drafts every 2 minutes
**REQ-208:** System shall allow users to save partial submissions and resume later
**REQ-209:** System shall prevent duplicate submissions for the same country/month/test combination
**REQ-210:** System shall provide contextual help text for each field

#### 3.1.3 Regional Grouping (Europe Special Case)
**REQ-301:** System shall support regional grouping for European countries
**REQ-302:** System shall allow France coordinator to submit volumes for all European regions in one session
**REQ-303:** System shall maintain region-to-country mapping table (editable by admins)
**REQ-304:** System shall support expansion to additional regional groupings as needed

#### 3.1.4 Submission Confirmation
**REQ-401:** System shall display confirmation screen upon successful submission
**REQ-402:** System shall generate unique submission ID for tracking
**REQ-403:** System shall send confirmation email to submitter with submission details
**REQ-404:** System shall provide downloadable PDF receipt of submission
**REQ-405:** System shall timestamp all submissions in UTC with local time display

#### 3.1.5 Historical View
**REQ-501:** System shall display submission history for user's assigned country
**REQ-502:** System shall allow viewing past 24 months of submissions
**REQ-503:** System shall indicate submission status (Draft, Submitted, Validated, Flagged)
**REQ-504:** System shall allow viewing but not editing of finalized submissions
**REQ-505:** System shall support comments/notes on historical submissions

---

### 3.2 Data Quality Dashboard (Power App - Internal)

#### 3.2.1 Submission Tracking
**REQ-601:** System shall display real-time status of all countries' submissions
**REQ-602:** System shall highlight countries that have not submitted by deadline
**REQ-603:** System shall show submission completeness percentage
**REQ-604:** System shall display last submission date/time for each country
**REQ-605:** System shall color-code status indicators (green=submitted, yellow=draft, red=overdue)

#### 3.2.2 Data Validation
**REQ-701:** System shall flag volumes that deviate >50% from prior month (anomaly detection)
**REQ-702:** System shall flag volumes that deviate >25% from budget (variance alert)
**REQ-703:** System shall flag missing required fields
**REQ-704:** System shall validate that Forecast volumes exist for all countries with Actuals
**REQ-705:** System shall allow analysts to configure validation thresholds by test type
**REQ-706:** System shall maintain validation rule library (expandable)

#### 3.2.3 Review Workflow
**REQ-801:** System shall provide queue of flagged submissions requiring review
**REQ-802:** System shall allow analysts to approve, reject, or request clarification
**REQ-803:** System shall support inline editing of submissions with reason codes
**REQ-804:** System shall maintain complete audit trail of all changes (who, when, what, why)
**REQ-805:** System shall send notification to coordinator when clarification requested
**REQ-806:** System shall track average review time per submission

#### 3.2.4 Manual Data Entry
**REQ-901:** System shall provide interface for analysts to manually enter volumes
**REQ-902:** System shall require reason code for manual entries (phone call, emergency, etc.)
**REQ-903:** System shall tag manually-entered data for audit purposes
**REQ-904:** System shall validate manual entries using same rules as portal submissions

#### 3.2.5 Communication Tools
**REQ-1001:** System shall allow analysts to send messages to coordinators
**REQ-1002:** System shall support email templates for common scenarios (reminder, clarification, thank you)
**REQ-1003:** System shall log all communications in submission history
**REQ-1004:** System shall support bulk messaging to multiple coordinators

---

### 3.3 Administrator Console (Power App - Internal)

#### 3.3.1 User Management
**REQ-1101:** System shall maintain user directory with country assignments
**REQ-1102:** System shall support adding/removing users
**REQ-1103:** System shall allow reassigning country responsibilities
**REQ-1104:** System shall support backup coordinators per country
**REQ-1105:** System shall send welcome email to new users with instructions

#### 3.3.2 Configuration Management
**REQ-1201:** System shall maintain master list of countries with regions
**REQ-1202:** System shall maintain master list of test types (courses)
**REQ-1203:** System shall support adding/retiring test types with effective dates
**REQ-1204:** System shall configure submission deadlines (default: 3rd business day)
**REQ-1205:** System shall support country-specific deadline exceptions
**REQ-1206:** System shall configure reminder schedule (days before deadline)

#### 3.3.3 System Monitoring
**REQ-1301:** System shall display dashboard of system health metrics
**REQ-1302:** System shall log all errors with timestamps and user context
**REQ-1303:** System shall alert admins to processing failures
**REQ-1304:** System shall track submission volume trends
**REQ-1305:** System shall monitor app performance (load times, errors)

---

### 3.4 Automated Workflows (Power Automate)

#### 3.4.1 Submission Processing
**REQ-1401:** System shall trigger confirmation email immediately upon submission
**REQ-1402:** System shall run validation checks within 5 minutes of submission
**REQ-1403:** System shall notify analyst of flagged submissions within 10 minutes
**REQ-1404:** System shall update submission status in real-time

#### 3.4.2 Reminder System
**REQ-1501:** System shall send first reminder 5 business days before deadline
**REQ-1502:** System shall send second reminder 2 business days before deadline
**REQ-1503:** System shall send final reminder on deadline day
**REQ-1504:** System shall send overdue notification to coordinator and manager day after deadline
**REQ-1505:** System shall escalate to regional manager if no submission after 3 days overdue
**REQ-1506:** System shall suppress reminders if submission already completed

#### 3.4.3 Status Notifications
**REQ-1601:** System shall notify coordinator when submission approved
**REQ-1602:** System shall notify coordinator when submission flagged with specific issues
**REQ-1603:** System shall notify coordinator when clarification requested
**REQ-1604:** System shall send weekly status digest to all coordinators
**REQ-1605:** System shall notify finance team when all submissions complete

#### 3.4.4 Reporting Automation
**REQ-1701:** System shall generate daily submission completion report for analysts
**REQ-1702:** System shall generate weekly executive summary email
**REQ-1703:** System shall generate monthly data quality report
**REQ-1704:** System shall schedule Power BI dashboard refresh every 4 hours during submission window

---

### 3.5 Executive Reporting (Power BI)

#### 3.5.1 Dashboard Pages
**REQ-1801:** System shall provide Executive Summary page with key metrics
**REQ-1802:** System shall provide Geographic Analysis page with country/region drill-down
**REQ-1803:** System shall provide Test Type Analysis page with course performance
**REQ-1804:** System shall provide Trend Analysis page with time-based comparisons
**REQ-1805:** System shall provide Data Quality Scorecard page with submission status
**REQ-1806:** System shall provide Variance Analysis page (Actual vs Budget vs Forecast)

#### 3.5.2 Key Metrics
**REQ-1901:** System shall calculate Total Enrollments (Net Volumes) by month
**REQ-1902:** System shall calculate Year-over-Year growth percentage
**REQ-1903:** System shall calculate Month-over-Month variance
**REQ-1904:** System shall calculate Actual vs Budget variance ($ and %)
**REQ-1905:** System shall calculate Actual vs Forecast variance ($ and %)
**REQ-1906:** System shall calculate submission completion rate (% of countries submitted)
**REQ-1907:** System shall display data freshness timestamp

#### 3.5.3 Interactivity
**REQ-2001:** System shall support filtering by date range, country, region, test type
**REQ-2002:** System shall support drill-through from summary to detail
**REQ-2003:** System shall support bookmarks for common views
**REQ-2004:** System shall support exporting to Excel/PDF
**REQ-2005:** System shall support natural language Q&A

#### 3.5.4 Mobile Experience
**REQ-2101:** System shall provide optimized layout for Power BI Mobile app
**REQ-2102:** System shall support offline viewing of cached data
**REQ-2103:** System shall support push notifications for key alerts

---

### 3.6 TM1 Integration

#### 3.6.1 CSV Export Requirements
**REQ-2201:** System shall generate TM1-compatible .csv file on-demand
**REQ-2202:** System shall format .csv with columns: Test Type, Country, Month, Actual, Budget, Forecast
**REQ-2203:** System shall use TM1 country naming conventions (mapping table maintained by admin)
**REQ-2204:** System shall validate .csv format before making available for download
**REQ-2205:** System shall log all export events with timestamp and user
**REQ-2206:** System shall support scheduled monthly export (automatic delivery to shared location)
**REQ-2207:** System shall include only validated/approved submissions in export
**REQ-2208:** System shall flag if export includes estimated/draft data

---

## 4. Non-Functional Requirements

### 4.1 Performance
**NFR-101:** Submission form shall load within 3 seconds on standard broadband
**NFR-102:** Submission processing shall complete within 30 seconds
**NFR-103:** Data Quality Dashboard shall load within 5 seconds
**NFR-104:** Power BI dashboard shall refresh within 2 minutes
**NFR-105:** System shall support 50 concurrent users without degradation
**NFR-106:** Auto-save shall occur within 2 seconds of data entry

### 4.2 Usability
**NFR-201:** System shall be accessible on desktop, tablet, and mobile devices
**NFR-202:** System shall support modern browsers (Chrome, Edge, Safari, Firefox - latest 2 versions)
**NFR-203:** System shall follow WCAG 2.1 Level AA accessibility standards
**NFR-204:** System shall provide consistent UI/UX across all apps
**NFR-205:** System shall display clear error messages with guidance for resolution
**NFR-206:** System shall support keyboard navigation
**NFR-207:** System shall provide contextual help throughout

### 4.3 Security
**NFR-301:** System shall use Azure AD authentication (SSO)
**NFR-302:** System shall enforce role-based access control
**NFR-303:** System shall encrypt data at rest and in transit
**NFR-304:** System shall log all data modifications with user identity
**NFR-305:** System shall comply with data privacy regulations (GDPR considerations for European data)
**NFR-306:** System shall support session timeout (30 minutes inactivity)
**NFR-307:** System shall prevent SQL injection and other common vulnerabilities

### 4.4 Reliability
**NFR-401:** System shall maintain 99.5% uptime during business hours (5am-8pm EST)
**NFR-402:** System shall implement error handling with graceful degradation
**NFR-403:** System shall provide retry logic for transient failures
**NFR-404:** System shall back up data daily
**NFR-405:** System shall maintain data integrity through validation rules
**NFR-406:** System shall recover from failures without data loss

### 4.5 Maintainability
**NFR-501:** System shall use component library for UI consistency
**NFR-502:** System shall document all formulas and business logic
**NFR-503:** System shall implement modular design for easy updates
**NFR-504:** System shall provide admin interface for configuration (no code changes)
**NFR-505:** System shall include comprehensive error logging for troubleshooting

### 4.6 Scalability
**NFR-601:** System shall support expansion to 100+ countries without architectural changes
**NFR-602:** System shall support addition of new test types without code modifications
**NFR-603:** System shall maintain performance with 10 years of historical data
**NFR-604:** System shall support multiple submission cycles per month (future requirement)

---

## 5. Business Rules

### 5.1 Volume Calculations
**BR-101:** Net Volumes = Gross Volumes (as submitted - no calculation needed in v1)
**BR-102:** If Budget is submitted, it applies to entire fiscal year
**BR-103:** Forecast must be submitted monthly and supersedes previous forecast
**BR-104:** Actual volumes are final once approved (no retroactive changes without admin approval)

### 5.2 Submission Rules
**BR-201:** Each country must submit Actual, Budget, and Forecast for all assigned test types
**BR-202:** Submissions are required by 3rd business day of following month
**BR-203:** Draft submissions do not count as "submitted" for deadline purposes
**BR-204:** Late submissions are flagged but still accepted
**BR-205:** Coordinators can only submit for their assigned countries
**BR-206:** Budget submissions required once per year (January)
**BR-207:** Forecast submissions required monthly
**BR-208:** Actual submissions required monthly

### 5.3 Validation Rules
**BR-301:** Volumes cannot be negative
**BR-302:** Volumes >50% different from prior month trigger anomaly flag
**BR-303:** Volumes >25% different from budget trigger variance flag
**BR-304:** Missing forecast when actual exists triggers warning
**BR-305:** Zero volumes for 3 consecutive months trigger investigation flag

### 5.4 Approval Rules
**BR-401:** Flagged submissions require analyst review before inclusion in reports
**BR-402:** Analyst can approve flagged submissions with reason code
**BR-403:** Analyst edits require reason code and are audited
**BR-404:** Only analysts and admins can mark submissions as "validated"
**BR-405:** Coordinators can resubmit if rejected by analyst

### 5.5 Notification Rules
**BR-501:** Reminders sent only on business days
**BR-502:** Escalations sent to regional managers only after 3 days overdue
**BR-503:** Confirmation emails sent immediately upon submission
**BR-504:** Weekly digests sent every Monday morning at 8am EST
**BR-505:** Executive summaries sent 5th business day of each month

---

## 6. Integration Requirements

### 6.1 TM1 Export
**INT-101:** System must generate .csv file compatible with TM1 import specifications
**INT-102:** Export must use TM1 naming conventions for countries and test types
**INT-103:** Export must include header row with column names
**INT-104:** Export must handle special characters in country names
**INT-105:** Export must be available by 4th business day of month

### 6.2 Email System
**INT-201:** System must integrate with Office 365 email for notifications
**INT-202:** System must support HTML email templates
**INT-203:** System must include unsubscribe option for non-critical emails
**INT-204:** System must track email delivery status

### 6.3 Azure AD
**INT-301:** System must authenticate users via Azure AD
**INT-302:** System must support SSO for seamless access across apps
**INT-303:** System must sync user profile data from Azure AD

---

## 7. Data Requirements

### 7.1 Master Data
**DATA-101:** Countries - Name, Region, Status, Timezone
**DATA-102:** Test Types - Name, Category, Status, Effective Date
**DATA-103:** Users - Email, Name, Country Assignment, Role
**DATA-104:** Regional Groupings - Region Name, Country List

### 7.2 Transaction Data
**DATA-201:** Volume Submissions - Country, Test Type, Month, Actual, Budget, Forecast, Status, Timestamp, User
**DATA-202:** Submission History - All changes with who/when/what/why
**DATA-203:** Validation Flags - Submission ID, Flag Type, Severity, Status, Resolution
**DATA-204:** Communications - Submission ID, From, To, Subject, Body, Timestamp

### 7.3 Reference Data
**DATA-301:** TM1 Country Mapping - Internal Name → TM1 Name
**DATA-302:** Business Day Calendar - For deadline calculations
**DATA-303:** Submission Deadlines - Default and exceptions by country
**DATA-304:** Email Templates - Message templates for common scenarios

---

## 8. Constraints & Assumptions

### 8.1 Constraints
**CON-101:** Must use existing Microsoft 365 licenses (no additional licensing)
**CON-102:** Must work within Power Platform governance policies
**CON-103:** Must not require custom connectors or premium features beyond standard licensing
**CON-104:** Must export to TM1 (cannot replace TM1 system)
**CON-105:** No Salesforce API access (Salesforce eliminated from workflow)
**CON-106:** Must support global users across multiple timezones

### 8.2 Assumptions
**ASM-101:** All users have Microsoft 365 accounts and Azure AD access
**ASM-102:** Users have reliable internet connectivity
**ASM-103:** Coordinators have authority to submit volumes for their countries
**ASM-104:** Current email-based process will be completely replaced (no parallel running)
**ASM-105:** Historical data migration not required (fresh start acceptable)
**ASM-106:** TM1 import process remains unchanged
**ASM-107:** Business day calendar uses US federal holidays

---

## 9. Out of Scope (Phase 2 Considerations)

The following features are explicitly excluded from v1.0 but documented for future consideration:

**OOS-101:** Multi-language support (English only in v1)
**OOS-102:** Mobile native app (mobile web only in v1)
**OOS-103:** Integration with other financial systems beyond TM1
**OOS-104:** Predictive analytics and ML-based forecasting
**OOS-105:** Bulk data upload via Excel import
**OOS-106:** Advanced reporting beyond Power BI (paginated reports, subscriptions)
**OOS-107:** Workflow for data correction approvals (analyst has direct edit authority in v1)
**OOS-108:** Integration with project management tools
**OOS-109:** Custom branding per region
**OOS-110:** Offline mobile submission capability

---

## 10. Success Metrics

### 10.1 Efficiency Metrics
- **Baseline:** 3 hours manual consolidation monthly
- **Target:** 15 minutes monthly (90% reduction)
- **Measure:** Time logged in Data Quality Dashboard

### 10.2 Timeliness Metrics
- **Baseline:** 50% on-time submission rate
- **Target:** 95% submission by deadline
- **Measure:** Submission tracking dashboard

### 10.3 Data Quality Metrics
- **Baseline:** 10-15 format errors per month requiring manual fixes
- **Target:** <2 errors per month
- **Measure:** Validation flag count

### 10.4 User Adoption Metrics
- **Target:** 100% of coordinators using portal by Month 2
- **Target:** <3 help desk tickets per coordinator in first 3 months
- **Measure:** Usage logs and support ticket tracking

### 10.5 Stakeholder Satisfaction
- **Target:** CFO accesses dashboard >3x per week
- **Target:** Finance team reports improved data confidence
- **Measure:** Usage analytics and satisfaction survey

---

## 11. Approval & Sign-Off

**Requirements Author:** John Rolzhausen  
**Date:** November 20, 2025  
**Version:** 1.0 - Draft

**Review & Approval:**
- [ ] Technical Review - Pending
- [ ] Business Review - Pending
- [ ] Security Review - Pending
- [ ] Final Approval - Pending

---

## Document History

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | 2025-11-20 | John Rolzhausen | Initial draft based on pain points analysis |

---

**Next Steps:**
1. Review and refine requirements based on feedback
2. Create user stories with acceptance criteria
3. Design data model to support requirements
4. Develop wireframes and UI mockups
5. Prioritize requirements for MVP vs Phase 2
