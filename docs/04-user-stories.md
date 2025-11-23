# User Stories & Personas
## Global Training Enrollment System

**Version:** 1.0  
**Date:** November 20, 2025  
**Author:** John Rolzhausen  
**Status:** Draft - In Review

---

## 1. Overview

This document defines the user personas and user stories for the Global Training Enrollment System. Each story includes acceptance criteria and is mapped to requirements and prioritized for development phases.

---

## 2. User Personas

### Persona 1: Ji-Won Kim (Country Volume Coordinator - Korea)

**Background:**
- Role: Regional Operations Manager, Seoul Office
- Responsibility: Submits monthly enrollment volumes for all Korea training programs
- Technical Proficiency: Moderate (comfortable with Excel, email, basic web applications)
- Languages: Korean (native), English (business proficient)

**Current Pain Points:**
- Spends 30-45 minutes monthly creating Excel file with specific format
- Must password-protect file before emailing (security policy)
- Sends password in separate email an hour later (also security policy)
- Anxious about whether file was received correctly
- No visibility into whether submission was processed
- Sometimes receives requests for corrections days later, forcing rework

**Goals:**
- Complete monthly submission in under 10 minutes
- Receive immediate confirmation of successful submission
- Know submission was received in correct format
- Avoid back-and-forth about missing or incorrect data
- Submit from office or home (flexible location)

**Technology Environment:**
- Windows laptop
- Office 365 account
- Reliable internet connectivity
- Mobile phone for occasional access

**Quote:** *"I just want to know that when I submit the numbers, they were received correctly and I'm done. The uncertainty is the worst part."*

---

### Persona 2: Marie Dubois (Country Volume Coordinator - France)

**Background:**
- Role: European Training Coordinator, Paris Office
- Responsibility: Submits volumes for ALL European countries (grouped by region: Western Europe, Eastern Europe, Nordic)
- Technical Proficiency: High (creates complex Excel workbooks, uses pivot tables, comfortable with new systems)
- Languages: French (native), English (fluent), German (conversational)

**Current Pain Points:**
- Manages volumes for 15+ countries across 3 regional groupings
- Excel file has grown unwieldy (multiple tabs, complex formulas)
- Coordinating data from multiple country managers via email
- Any format change requires updating complex formulas
- Submission takes 60-90 minutes due to data consolidation
- Frequently asked to "break out" specific countries for ad-hoc analysis

**Goals:**
- Submit all European volumes in one efficient session
- Maintain regional groupings while allowing country-level detail
- Reduce submission time to under 20 minutes
- Easily provide country-level breakouts when requested
- Collaborate with country managers before final submission

**Technology Environment:**
- Windows laptop
- Office 365 account with OneDrive
- Excellent internet connectivity
- Occasionally works from home

**Quote:** *"I'm coordinating 15 countries. I need a system that helps me organize this, not one that makes me fight with Excel formulas every month."*

---

### Persona 3: Yuki Tanaka (Country Volume Coordinator - Japan)

**Background:**
- Role: Training Administration Manager, Tokyo Office
- Responsibility: Submits monthly volumes for Japan
- Technical Proficiency: Low-Moderate (uses Excel for basic tasks, prefers familiar processes, cautious about new systems)
- Languages: Japanese (native), English (reading proficient, speaking/writing limited)

**Current Pain Points:**
- Excel file occasionally becomes corrupted during email transmission
- Must recreate file manually when corruption occurs (frustrating and time-consuming)
- Password process feels cumbersome but necessary for security
- Concerned about data security when sending via email
- Prefers consistency - doesn't want process changes frequently
- Worries about making mistakes in unfamiliar systems

**Goals:**
- Submit volumes using a reliable, secure method
- Avoid file corruption issues
- Maintain data security
- Minimal learning curve for new system
- Clear instructions in English (or ideally Japanese in future)
- Confidence that submission was successful

**Technology Environment:**
- Windows desktop computer (office only)
- Office 365 account
- Reliable internet at office
- Prefers desktop over mobile

**Quote:** *"The current process works except when the file corrupts. I just need something reliable that keeps our data secure."*

---

### Persona 4: John Rolzhausen (Data Quality Analyst)

**Background:**
- Role: Senior Finance Analyst, USA Corporate Office
- Responsibility: Consolidate all country submissions, validate data quality, export to TM1, support finance leadership
- Technical Proficiency: High (Power Platform expert, SQL knowledge, Excel power user, data analysis)
- Experience: 20 years in educational services finance

**Current Pain Points:**
- Spends 3+ hours on 3rd business day manually consolidating submissions
- Password-protected files require manual intervention
- Format variations break automated Power BI refresh
- Late submissions force repeating entire consolidation process
- No systematic way to identify anomalies until after consolidation
- Manual data quality checks are time-consuming and error-prone
- Fielding questions from coordinators about submission status

**Goals:**
- Reduce consolidation time from 3 hours to 15 minutes
- Catch data quality issues at submission time (not after)
- Automate validation and anomaly detection
- Provide real-time visibility to coordinators
- Spend time on analysis instead of data wrangling
- Generate TM1 export with one click
- Eliminate manual file format corrections

**Technology Environment:**
- Windows laptop
- Full Microsoft 365 suite
- Power Platform developer license
- SQL Server access
- TM1 access
- Multiple monitors for efficiency

**Quote:** *"I built a system that worked for 8 years. I know what a good process looks like, and I know this current process isn't it. Let's build it right."*

---

### Persona 5: Robert Chen (CFO)

**Background:**
- Role: Chief Financial Officer
- Responsibility: Financial oversight, strategic planning, board reporting
- Technical Proficiency: Moderate-High (comfortable with dashboards, Excel, wants insights without digging through data)
- Experience: 15 years as CFO, finance background

**Current Pain Points:**
- Waits until 4th-5th business day for volume data (too slow for agile decision-making)
- Cannot access data outside of scheduled report delivery
- Limited ability to explore data interactively (drill down, filter, compare)
- Wants to see trends and anomalies quickly without analyst intervention
- Needs mobile access for board meetings and travel
- Difficulty identifying which countries are driving changes

**Goals:**
- Access volume data in real-time, not batch reports
- Self-service exploration of trends and patterns
- Quick identification of anomalies requiring attention
- Mobile access to dashboard
- Drill down from global → region → country → test type
- Historical trend analysis (YoY, MoM comparisons)
- Know data quality/freshness (what's actual vs estimated)

**Technology Environment:**
- iPad Pro (primary device for meetings/travel)
- Windows laptop (office)
- Office 365 account
- Power BI mobile app
- Excellent connectivity

**Quote:** *"I don't need the data to be perfect on day 1, but I need to SEE what we have. Give me visibility, even if some numbers are estimates."*

---

### Persona 6: Alice Katt (Enters Volumes into Salesforce)

**Background:**
- Role: Business Analyst
- Responsibility: Collects monthly enrollment volumes for all countries not provided separately and enters them into Salesforce 
- Technical Proficiency: Moderate (comfortable with Excel, email, basic web applications, Salesforce expert)
- Experience: 25 years as a Business

**Current Pain Points:**
- Overwhelmed by number of emails received throughout the month.
- Countries submit on different schedules (some once a month, some weekly, some every day there's a volume)
- Must confirm total amounts at Volume submission cutoff ties to Salesforce amounts due to different schedules
- Volumes for Vietnam are labled as France due to Parent offices being located in Nice, France. Must ensure they are
  labeled correctly in Salesforce

**Goals:**
- Automate collection of volumes while still retaining validation responsibilities
- Consistent and predictible receipt of volumes more regularly but easier to submit
- Quick identification of anomalies requiring attention

**Technology Environment:**
- Windows laptop (office)
- Office 365 account
- Excellent connectivity
- Salesforce administrator license 

**Quote:** *"Emails coming in at unpredictable times is overwhelming. I wish the representatives from each country could enter their own volumes but I
would still need to have control of review and validation. It's difficult for me to take time off without my work piling up. The company won't allow 
additional Salesforce licenses and I can't have my team enter volumes.  It all falls on me."*

---

## 3. User Stories by Persona

### 3.1 Ji-Won Kim (Korea Coordinator) Stories

#### Story 1.1: Quick Submission
**As** Ji-Won,  
**I want to** submit monthly volumes through a web form in under 10 minutes,  
**So that** I can complete this task efficiently and move on to other priorities.

**Acceptance Criteria:**
- [ ] Login takes less than 30 seconds
- [ ] Form pre-populates with my assigned country (Korea)
- [ ] Form displays only relevant test types for Korea
- [ ] I can enter volumes for Actual, Budget, and Forecast
- [ ] Form validates entries in real-time (no negative numbers, required fields)
- [ ] Auto-save occurs every 2 minutes
- [ ] Submission completes within 30 seconds
- [ ] Confirmation screen appears with submission ID

**Priority:** MVP - Must Have  
**Requirements:** REQ-101, REQ-102, REQ-201-210, REQ-401-405  
**Estimated Effort:** Medium

---

#### Story 1.2: Submission Confidence
**As** Ji-Won,  
**I want to** receive immediate confirmation when I submit volumes,  
**So that** I know my submission was received correctly and I can stop worrying about it.

**Acceptance Criteria:**
- [ ] Confirmation screen displays immediately after submission
- [ ] Confirmation includes submission ID, date/time, summary of volumes submitted
- [ ] Email confirmation sent within 1 minute
- [ ] Email includes PDF receipt attachment
- [ ] I can download/print confirmation for my records
- [ ] Confirmation explicitly states "No further action required"

**Priority:** MVP - Must Have  
**Requirements:** REQ-401-405, REQ-1401  
**Estimated Effort:** Small

---

#### Story 1.3: Historical Visibility
**As** Ji-Won,  
**I want to** view my past 24 months of submissions,  
**So that** I can reference what I submitted previously and ensure consistency.

**Acceptance Criteria:**
- [ ] "Submission History" page shows all my past submissions
- [ ] Submissions sorted by date (most recent first)
- [ ] Each submission shows month, test types, volumes, status
- [ ] I can filter by date range or test type
- [ ] I can view detailed submission but cannot edit finalized ones
- [ ] Status indicators are clear (Submitted, Validated, Flagged, Approved)

**Priority:** MVP - Should Have  
**Requirements:** REQ-501-505  
**Estimated Effort:** Medium

---

#### Story 1.4: Estimate Submission for Delays
**As** Ji-Won,  
**I want to** submit reasonable estimates when I have legitimate reporting delays,  
**So that** finance has numbers to work with rather than nothing, and I can update with actuals later.

**Acceptance Criteria:**
- [ ] Option to mark submission as "Estimated"
- [ ] Required explanation field (minimum 20 characters) for why it's an estimate
- [ ] Clear visual indicator that volumes are estimates (icon, color)
- [ ] Email confirmation explicitly states submission contains estimates
- [ ] I receive reminder to submit actuals when available
- [ ] System allows me to replace estimate with actual later

**Priority:** MVP - Should Have  
**Requirements:** REQ-211  
**Estimated Effort:** Medium

---

### 3.2 Marie Dubois (France/Europe Coordinator) Stories

#### Story 2.1: Regional Submission Efficiency
**As** Marie,  
**I want to** submit volumes for all European countries in one session,  
**So that** I can efficiently complete my monthly reporting without multiple logins or forms.

**Acceptance Criteria:**
- [ ] Login shows I'm responsible for multiple countries (European region)
- [ ] I can select which countries to submit for in current session
- [ ] Form allows me to enter volumes for multiple countries sequentially
- [ ] Progress indicator shows how many countries completed
- [ ] I can save partial progress and return later
- [ ] All submissions grouped together in history with "batch" identifier

**Priority:** MVP - Must Have  
**Requirements:** REQ-301, REQ-302, REQ-208  
**Estimated Effort:** Large

---

#### Story 2.2: Draft Management
**As** Marie,  
**I want to** save draft submissions and complete them over multiple sessions,  
**So that** I can gather data from country managers over several days before final submission.

**Acceptance Criteria:**
- [ ] "Save Draft" button always visible
- [ ] Draft auto-saves every 2 minutes
- [ ] Draft submission doesn't trigger deadline notifications
- [ ] I can see which countries have drafts vs. completed submissions
- [ ] Draft data persists across sessions/devices
- [ ] Clear visual distinction between draft and submitted
- [ ] "Submit All Drafts" option available when ready

**Priority:** MVP - Must Have  
**Requirements:** REQ-207-208  
**Estimated Effort:** Medium

---

#### Story 2.3: Country-Level Detail Retention
**As** Marie,  
**I want** volumes stored at country level even though I submit regionally,  
**So that** I can easily provide country breakouts when finance requests them.

**Acceptance Criteria:**
- [ ] Volumes stored individually per country (not aggregated)
- [ ] Reports show regional totals with drill-down to countries
- [ ] I can export country-level data for ad-hoc requests
- [ ] Regional grouping maintained as metadata (not calculation)
- [ ] Finance team can see both regional and country views

**Priority:** MVP - Must Have  
**Requirements:** REQ-301, DATA-101  
**Estimated Effort:** Medium

---

### 3.3 Yuki Tanaka (Japan Coordinator) Stories

#### Story 3.1: Reliable Submission Process
**As** Yuki,  
**I want to** submit volumes through a web portal instead of email attachments,  
**So that** I never experience file corruption issues again.

**Acceptance Criteria:**
- [ ] No file creation/attachment required
- [ ] Direct data entry in secure web form
- [ ] Submission success not dependent on email delivery
- [ ] No password management required (Azure AD authentication)
- [ ] Clear confirmation that data was saved
- [ ] Process identical every month (no surprise changes)

**Priority:** MVP - Must Have  
**Requirements:** REQ-101, REQ-201-210  
**Estimated Effort:** Small

---

#### Story 3.2: Clear Instructions
**As** Yuki,  
**I want to** see clear, simple instructions for each field,  
**So that** I can complete the submission confidently despite language barriers.

**Acceptance Criteria:**
- [ ] Contextual help text for each field
- [ ] Instructions in simple English (Japanese in Phase 2)
- [ ] Examples provided where helpful (e.g., "Enter 1500, not 1,500")
- [ ] Visual cues for required vs. optional fields
- [ ] Error messages are clear and actionable
- [ ] "Help" link available throughout

**Priority:** MVP - Must Have  
**Requirements:** REQ-210  
**Estimated Effort:** Small

---

### 3.4 John Rolzhausen (Data Quality Analyst) Stories

#### Story 4.1: Real-Time Submission Monitoring
**As** John,  
**I want to** see real-time status of all country submissions,  
**So that** I can identify who hasn't submitted and proactively follow up before the deadline.

**Acceptance Criteria:**
- [ ] Dashboard shows all countries with submission status
- [ ] Color-coded indicators (green=done, yellow=draft, red=not started)
- [ ] Sortable by status, country, region, last submission date
- [ ] Click country to see submission details
- [ ] Filter by status, region, test type
- [ ] Export list of non-submitters for follow-up
- [ ] Dashboard refreshes automatically every 5 minutes

**Priority:** MVP - Must Have  
**Requirements:** REQ-601-605  
**Estimated Effort:** Large

---

#### Story 4.2: Automated Anomaly Detection
**As** John,  
**I want** the system to automatically flag volumes that deviate significantly from expectations,  
**So that** I can focus my review time on submissions that need attention rather than checking everything manually.

**Acceptance Criteria:**
- [ ] System flags volumes >50% different from prior month
- [ ] System flags volumes >25% different from budget
- [ ] System flags missing forecasts when actuals exist
- [ ] System flags zero volumes for 3+ consecutive months
- [ ] Flagged submissions appear in review queue
- [ ] I can configure thresholds by test type
- [ ] I can add custom validation rules
- [ ] False positives can be dismissed with reason

**Priority:** MVP - Must Have  
**Requirements:** REQ-701-706  
**Estimated Effort:** Large

---

#### Story 4.3: Efficient Review Workflow
**As** John,  
**I want to** review and approve/reject flagged submissions in a queue,  
**So that** I can systematically validate data quality without switching between tools or losing track.

**Acceptance Criteria:**
- [ ] Queue shows all flagged submissions ordered by priority
- [ ] Click submission to see details with flag reason
- [ ] Compare to historical data (prior month, budget, forecast)
- [ ] Options: Approve, Request Clarification, Reject, Edit
- [ ] Inline editing with required reason code
- [ ] All actions tracked in audit log
- [ ] Coordinator notified of my action automatically
- [ ] Queue counter shows remaining items

**Priority:** MVP - Must Have  
**Requirements:** REQ-801-806  
**Estimated Effort:** Large

---

#### Story 4.4: One-Click TM1 Export
**As** John,  
**I want to** generate TM1-compatible .csv file with one click,  
**So that** I can quickly upload volumes to the budgeting system without manual file creation.

**Acceptance Criteria:**
- [ ] "Export to TM1" button prominently placed
- [ ] Export includes only validated/approved submissions
- [ ] File follows TM1 format specifications exactly
- [ ] Country names mapped to TM1 conventions
- [ ] File downloads immediately (no waiting for email)
- [ ] Export logged with timestamp and user
- [ ] Option to schedule automatic export on 4th business day
- [ ] Warning if export includes estimated data

**Priority:** MVP - Must Have  
**Requirements:** REQ-2201-2208  
**Estimated Effort:** Medium

---

#### Story 4.5: Communication Tools
**As** John,  
**I want to** send messages to coordinators directly from the data quality dashboard,  
**So that** I can request clarification or provide feedback without switching to email.

**Acceptance Criteria:**
- [ ] "Send Message" button available on each submission
- [ ] Email templates for common scenarios (clarification, thank you, reminder)
- [ ] Custom message option available
- [ ] Message logged in submission history
- [ ] Coordinator receives email notification
- [ ] I can see conversation history for each submission
- [ ] Bulk messaging available for reminders

**Priority:** MVP - Should Have  
**Requirements:** REQ-1001-1004  
**Estimated Effort:** Medium

---

### 3.5 Robert Chen (CFO) Stories

#### Story 5.1: Real-Time Dashboard Access
**As** Robert,  
**I want to** access the volume dashboard anytime from any device,  
**So that** I can review trends and make decisions without waiting for scheduled reports.

**Acceptance Criteria:**
- [ ] Power BI dashboard accessible via web and mobile app
- [ ] Data updates every 4 hours during submission window
- [ ] Data freshness timestamp clearly displayed
- [ ] Dashboard loads in under 5 seconds
- [ ] Works on iPad, laptop, and mobile phone
- [ ] Saves my filter preferences between sessions
- [ ] Can be presented in board meetings (large screen compatible)

**Priority:** MVP - Must Have  
**Requirements:** REQ-1801, REQ-2101-2103, NFR-104  
**Estimated Effort:** Medium

---

#### Story 5.2: Interactive Exploration
**As** Robert,  
**I want to** drill down from global totals to regional and country details,  
**So that** I can quickly identify which areas are driving overall performance.

**Acceptance Criteria:**
- [ ] Click regional total to see country breakdown
- [ ] Click country to see test type breakdown
- [ ] Filters apply across all visualizations
- [ ] Drill-through returns to prior view easily
- [ ] Breadcrumb navigation shows current context
- [ ] Export filtered data to Excel for presentations
- [ ] Natural language Q&A: "Show me Japan actual vs forecast"

**Priority:** MVP - Must Have  
**Requirements:** REQ-2001-2005  
**Estimated Effort:** Medium

---

#### Story 5.3: Trend and Variance Analysis
**As** Robert,  
**I want to** see year-over-year and month-over-month trends with variance calculations,  
**So that** I can quickly assess performance against targets and prior periods.

**Acceptance Criteria:**
- [ ] YoY growth % calculated and displayed prominently
- [ ] MoM variance shown with directional indicators (↑↓)
- [ ] Actual vs Budget variance ($ and %)
- [ ] Actual vs Forecast variance ($ and %)
- [ ] Sparklines showing 12-month trend
- [ ] Color coding for positive/negative variances
- [ ] Top movers highlighted (biggest changes)

**Priority:** MVP - Must Have  
**Requirements:** REQ-1901-1907  
**Estimated Effort:** Large

---

#### Story 5.4: Data Quality Visibility
**As** Robert,  
**I want to** know which volumes are actual vs estimated,  
**So that** I understand data confidence when making decisions.

**Acceptance Criteria:**
- [ ] Visual indicator for estimated data (icon, asterisk, color)
- [ ] Tooltip explains why data is estimated
- [ ] Submission completion % displayed (how many countries submitted)
- [ ] Data quality scorecard page shows flagged items
- [ ] Can filter to view "actuals only" or "include estimates"
- [ ] Estimated data clearly marked in exports
- [ ] Alert if presenting data with significant estimates

**Priority:** MVP - Must Have  
**Requirements:** REQ-211, REQ-1805, REQ-2208  
**Estimated Effort:** Medium

---

### Persona 6: Alice Katt (Enters Volumes into Salesforce)

#### Story 6.1: Automation of Volume Entries
**As** Alice,  
**I want to** automate submission of volumes to be sent more often and on a predictible schedule that doesn't come directly to me,  
**So that** I can review volume submissions periodically through the month without clogging up my inbox.

**Acceptance Criteria:**
- [ ] Reduce time to enter volumes from 1 hour to 10 minutes on days volumes are received
- [ ] Ability to break out volumes into groupings and not merely net volumes
- [ ] I can enter volumes for Actual, Budget, and Forecast
- [ ] Auto-save occurs every 2 minutes
- [ ] Submission completes within 30 seconds
- [ ] Confirmation screen appears with submission ID

**Priority:** MVP - Must Have  
**Requirements:** REQ-101, REQ-102, REQ-201-210, REQ-401-405  
**Estimated Effort:** Difficult

---

## 4. Story Prioritization Summary

### MVP - Must Have (Phase 1 - Weeks 1-4)
These stories are essential for launch and solve the core problem:

**Submission Portal:**
- Story 1.1: Quick Submission (Ji-Won)
- Story 1.2: Submission Confidence (Ji-Won)
- Story 2.1: Regional Submission (Marie)
- Story 2.2: Draft Management (Marie)
- Story 2.3: Country-Level Detail (Marie)
- Story 3.1: Reliable Process (Yuki)
- Story 3.2: Clear Instructions (Yuki)
- Story 6.1: Automation of Volume Entries

**Data Quality Dashboard:**
- Story 4.1: Real-Time Monitoring (John)
- Story 4.2: Automated Anomalies (John)
- Story 4.3: Review Workflow (John)
- Story 4.4: TM1 Export (John)

**Executive Reporting:**
- Story 5.1: Real-Time Access (Robert)
- Story 5.2: Interactive Exploration (Robert)
- Story 5.3: Trend Analysis (Robert)
- Story 5.4: Data Quality Visibility (Robert)

**Total MVP Stories:** 16

---

### MVP - Should Have (Phase 1 if time permits)
Important but not blocking launch:

- Story 1.3: Historical Visibility (Ji-Won)
- Story 1.4: Estimate Submission (Ji-Won)
- Story 4.5: Communication Tools (John)

**Total Should Have Stories:** 3

---

### Phase 2 - Nice to Have
Valuable enhancements after proven adoption:

- Advanced anomaly explanation (AI-powered)
- Collaboration features for regional coordinators
- Bulk data import for historical backfill
- Multi-language support
- Automated Excel report generation
- What-if scenario modeling
- Predictive forecasting

---

## 5. Story Mapping to Requirements

| Story | Key Requirements | Complexity | Value |
|-------|-----------------|------------|-------|
| 1.1 Quick Submission | REQ-201-210, REQ-401-405 | Medium | High |
| 1.2 Submission Confidence | REQ-401-405, REQ-1401 | Small | High |
| 1.3 Historical Visibility | REQ-501-505 | Medium | Medium |
| 1.4 Estimate Submission | REQ-211 | Medium | High |
| 2.1 Regional Submission | REQ-301, REQ-302 | Large | High |
| 2.2 Draft Management | REQ-207-208 | Medium | High |
| 2.3 Country Detail | REQ-301, DATA-101 | Medium | High |
| 3.1 Reliable Process | REQ-201-210 | Small | High |
| 3.2 Clear Instructions | REQ-210 | Small | Medium |
| 4.1 Submission Monitoring | REQ-601-605 | Large | High |
| 4.2 Anomaly Detection | REQ-701-706 | Large | High |
| 4.3 Review Workflow | REQ-801-806 | Large | High |
| 4.4 TM1 Export | REQ-2201-2208 | Medium | High |
| 4.5 Communication Tools | REQ-1001-1004 | Medium | Medium |
| 5.1 Real-Time Access | REQ-1801, REQ-2101 | Medium | High |
| 5.2 Interactive Exploration | REQ-2001-2005 | Medium | High |
| 5.3 Trend Analysis | REQ-1901-1907 | Large | High |
| 5.4 Data Quality Visibility | REQ-211, REQ-1805 | Medium | High |

---

## 6. Development Sequence Recommendation

**Sprint 1 (Week 2):**
- Story 1.1: Quick Submission (foundation for everything)
- Story 3.1: Reliable Process (same as 1.1, just different persona)
- Story 1.2: Submission Confirmation (completes submission flow)

**Sprint 2 (Week 2-3):**
- Story 2.1: Regional Submission (complex but high value)
- Story 2.2: Draft Management (enables incremental work)
- Story 4.1: Submission Monitoring (John needs visibility)

**Sprint 3 (Week 3):**
- Story 4.2: Anomaly Detection (core value proposition)
- Story 4.3: Review Workflow (completes data quality process)
- Story 4.4: TM1 Export (critical integration)

**Sprint 4 (Week 4):**
- Story 5.1: Real-Time Dashboard Access (executive visibility)
- Story 5.2: Interactive Exploration (self-service analytics)
- Story 5.3: Trend Analysis (business value)
- Story 5.4: Data Quality Visibility (transparency)

**If Time Permits:**
- Story 1.3: Historical Visibility
- Story 1.4: Estimate Submission
- Story 3.2: Clear Instructions (enhance UX)
- Story 4.5: Communication Tools

---

## 7. Acceptance Testing Approach

For each story, testing will include:

1. **Functional Testing:** Does it meet acceptance criteria?
2. **Usability Testing:** Can persona complete task efficiently?
3. **Integration Testing:** Does it work with other components?
4. **Performance Testing:** Does it meet non-functional requirements?
5. **Security Testing:** Does it enforce role-based access?

**Definition of Done:**
- [ ] All acceptance criteria met
- [ ] Code/formulas documented
- [ ] Tested by someone other than developer
- [ ] Screenshots captured for documentation
- [ ] Integrated with existing components
- [ ] Performance meets NFRs
- [ ] Security validated

---

## Document History

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | 2025-11-20 | John Rolzhausen | Initial user stories based on requirements v1.1 and pain points analysis |

---

**Next Steps:**
1. Review personas for accuracy and completeness
2. Validate user stories reflect actual workflows
3. Refine acceptance criteria based on feedback
4. Finalize story prioritization
5. Begin data model design to support stories
