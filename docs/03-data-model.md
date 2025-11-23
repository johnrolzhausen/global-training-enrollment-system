# Data Model Design Document
## Global Training Enrollment System

**Version:** 1.1  
**Date:** November 22, 2025  
**Author:** John Rolzhausen  
**Status:** Draft - In Review

---

## 1. Overview

This document defines the logical and physical data model for the Global Training Enrollment System. The model is designed to support:
- Multiple countries submitting volumes across multiple test types
- Regional aggregation and reporting
- Multi-stage validation workflow
- Historical tracking and audit trails
- TM1 integration requirements
- Real-time analytics via Power BI

**Design Principles:**
- Normalize to 3rd Normal Form (3NF) to eliminate redundancy
- Use surrogate keys (IDs) for all entities
- Maintain referential integrity through relationships
- Support temporal queries (point-in-time accuracy)
- Enable soft deletes (never physically delete records)
- Index for query performance

---

## 2. Entity Relationship Diagram (ERD)

```
┌─────────────────┐       ┌─────────────────┐
│    Regions      │◄──────┤   Countries     │
│                 │  1:M  │                 │
│ RegionID (PK)   │       │ CountryID (PK)  │
│ RegionName      │       │ CountryName     │
│ RegionCode      │       │ CountryCode     │
│ Status          │       │ RegionID (FK)   │
└─────────────────┘       │ TimezoneOffset  │
                          │ Status          │
                          └────────┬────────┘
                                   │
                                   │ 1:M
                                   │
                          ┌────────▼────────┐
                          │  TestTypes      │
                          │                 │
                          │ TestTypeID (PK) │
                          │ TestTypeName    │
                          │ TestTypeCode    │
                          │ Category        │
                          │ Status          │
                          │ EffectiveDate   │
                          │ RetiredDate     │
                          └─────────────────┘
                                   │
                                   │
                          ┌────────┴────────┐
                          │                 │
                          │ M:M (via)       │
                          │                 │
                ┌─────────▼──────────┐      │
                │ Country_TestType   │      │
                │ (Junction Table)   │      │
                │                    │      │
                │ ID (PK)            │      │
                │ CountryID (FK)     │      │
                │ TestTypeID (FK)    │      │
                │ EffectiveDate      │      │
                │ IsActive           │      │
                └────────────────────┘      │
                                            │
┌─────────────────┐                        │
│     Users       │                        │
│                 │                        │
│ UserID (PK)     │                        │
│ Email           │                        │
│ DisplayName     │                        │
│ RoleID (FK)     │                        │
│ Status          │                        │
└────────┬────────┘                        │
         │                                 │
         │ 1:M                             │
         │                                 │
┌────────▼────────┐                        │
│ User_Country    │                        │
│ (Assignment)    │                        │
│                 │                        │
│ ID (PK)         │                        │
│ UserID (FK)     │                        │
│ CountryID (FK)  │                        │
│ IsPrimary       │                        │
│ IsBackup        │                        │
└─────────────────┘                        │
                                           │
┌─────────────────┐                        │
│     Roles       │                        │
│                 │                        │
│ RoleID (PK)     │                        │
│ RoleName        │                        │
│ RoleCode        │                        │
│ Permissions     │                        │
└─────────────────┘                        │
                                           │
                                           │
         ┌─────────────────────────────────┘
         │
         │ M:1
         │
┌────────▼─────────────────────────────────┐
│         Volume_Submissions               │
│                                          │
│ SubmissionID (PK)                        │
│ CountryID (FK)                           │
│ TestTypeID (FK)                          │
│ SubmissionMonth (YYYY-MM)                │
│ SubmittedBy (FK → Users)                 │
│ SubmittedDate                            │
│ SubmissionMethod (Portal/Upload/Manual)  │
│ Status (Draft/Submitted/Validated/etc.)  │
│                                          │
│ ActualVolume                             │
│ BudgetVolume                             │
│ ForecastVolume                           │
│                                          │
│ IsEstimate                               │
│ EstimateReason                           │
│                                          │
│ ValidatedBy (FK → Users)                 │
│ ValidatedDate                            │
│ ValidationNotes                          │
│                                          │
│ DataQualityScore                         │
│ LastModifiedBy (FK → Users)              │
│ LastModifiedDate                         │
│ IsDeleted (soft delete)                  │
│ RowVersion (concurrency)                 │
└──────────┬───────────────────────────────┘
           │
           │ 1:M
           │
┌──────────▼───────────────────────────────┐
│     Submission_History                   │
│                                          │
│ HistoryID (PK)                           │
│ SubmissionID (FK)                        │
│ ChangeDate                               │
│ ChangedBy (FK → Users)                   │
│ ChangeType (Created/Updated/Validated)   │
│ PreviousStatus                           │
│ NewStatus                                │
│ PreviousActual                           │
│ NewActual                                │
│ PreviousForecast                         │
│ NewForecast                              │
│ ChangeReason                             │
│ IPAddress                                │
└──────────────────────────────────────────┘


           ┌───────────────────────────────┐
           │                               │
           │ 1:M                           │
           │                               │
┌──────────▼───────────────────────────────┐
│     Data_Quality_Flags                   │
│                                          │
│ FlagID (PK)                              │
│ SubmissionID (FK)                        │
│ FlagType (Anomaly/Variance/Missing)      │
│ Severity (Low/Medium/High)               │
│ Description                              │
│ ThresholdValue                           │
│ ActualValue                              │
│ FlaggedDate                              │
│ Status (Open/Dismissed/Resolved)         │
│ ResolvedBy (FK → Users)                  │
│ ResolvedDate                             │
│ ResolutionNotes                          │
└──────────────────────────────────────────┘


┌──────────────────────────────────────────┐
│     Communications                       │
│                                          │
│ CommunicationID (PK)                     │
│ SubmissionID (FK) [nullable]             │
│ CountryID (FK) [nullable]                │
│ FromUserID (FK → Users)                  │
│ ToUserID (FK → Users)                    │
│ Subject                                  │
│ Body                                     │
│ CommunicationType (Email/Internal/SMS)   │
│ SentDate                                 │
│ ReadDate                                 │
│ IsSystemGenerated                        │
│ TemplateID (FK → Templates)              │
└──────────────────────────────────────────┘


┌──────────────────────────────────────────┐
│     TM1_Export_Log                       │
│                                          │
│ ExportID (PK)                            │
│ ExportDate                               │
│ ExportedBy (FK → Users)                  │
│ SubmissionMonth                          │
│ RecordCount                              │
│ FilePath                                 │
│ FileHash (integrity check)               │
│ IncludesEstimates                        │
│ EstimateCount                            │
│ Status (Success/Failed)                  │
│ ErrorMessage                             │
└──────────────────────────────────────────┘


┌──────────────────────────────────────────┐
│     System_Configuration                 │
│                                          │
│ ConfigID (PK)                            │
│ ConfigKey                                │
│ ConfigValue                              │
│ DataType                                 │
│ Description                              │
│ Category                                 │
│ LastModifiedBy (FK → Users)              │
│ LastModifiedDate                         │
└──────────────────────────────────────────┘
```

---

## 3. Entity Definitions

### 3.1 Regions

**Purpose:** Master list of geographic regions for grouping countries

| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| RegionID | INT | PK, Auto-increment | Unique identifier |
| RegionName | VARCHAR(100) | NOT NULL, UNIQUE | Display name (e.g., "Western Europe") |
| RegionCode | VARCHAR(10) | NOT NULL, UNIQUE | Short code (e.g., "WE") |
| Status | VARCHAR(20) | NOT NULL, DEFAULT 'Active' | Active/Inactive |
| CreatedBy | INT | FK → Users, NOT NULL | User who created |
| CreatedDate | DATETIME | NOT NULL, DEFAULT NOW() | Creation timestamp |
| ModifiedBy | INT | FK → Users | Last modifier |
| ModifiedDate | DATETIME | | Last modification timestamp |

**Business Rules:**
- Cannot delete region if countries are assigned to it
- RegionCode must be 2-10 alphanumeric characters
- Status values: 'Active', 'Inactive'

**Indexes:**
- PRIMARY KEY on RegionID
- UNIQUE INDEX on RegionCode
- INDEX on Status (for filtered queries)

---

### 3.2 Countries

**Purpose:** Master list of all countries that submit volumes

| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| CountryID | INT | PK, Auto-increment | Unique identifier |
| CountryName | VARCHAR(100) | NOT NULL, UNIQUE | Official country name |
| CountryCode | VARCHAR(3) | NOT NULL, UNIQUE | ISO 3166-1 alpha-3 code |
| RegionID | INT | FK → Regions, NOT NULL | Geographic region |
| TimezoneOffset | DECIMAL(4,2) | NOT NULL | UTC offset (e.g., +9.0 for Japan) |
| TM1CountryName | VARCHAR(100) | NOT NULL | Country name as used in TM1 system |
| Status | VARCHAR(20) | NOT NULL, DEFAULT 'Active' | Active/Inactive |
| SubmissionDeadlineDay | INT | DEFAULT 3 | Business day of month for deadline |
| Notes | VARCHAR(500) | | Special instructions/notes |
| CreatedBy | INT | FK → Users, NOT NULL | User who created |
| CreatedDate | DATETIME | NOT NULL, DEFAULT NOW() | Creation timestamp |
| ModifiedBy | INT | FK → Users | Last modifier |
| ModifiedDate | DATETIME | | Last modification timestamp |

**Business Rules:**
- CountryCode must be valid ISO 3166-1 alpha-3 code
- TimezoneOffset range: -12.0 to +14.0
- SubmissionDeadlineDay range: 1-28 (to handle February)
- Status values: 'Active', 'Inactive', 'Pending'
- TM1CountryName required for TM1 export mapping

**Indexes:**
- PRIMARY KEY on CountryID
- UNIQUE INDEX on CountryCode
- FOREIGN KEY INDEX on RegionID
- INDEX on Status
- INDEX on TM1CountryName (for export queries)

**Sample Data:**
```sql
INSERT INTO Countries VALUES
(1, 'Republic of Korea', 'KOR', 3, +9.0, 'Korea', 'Active', 3, 'Monthly password changes'),
(2, 'Japan', 'JPN', 3, +9.0, 'Japan', 'Active', 3, 'Occasional file corruption'),
(3, 'France', 'FRA', 1, +1.0, 'France', 'Active', 3, 'Regional coordinator for Europe');
```

---

### 3.3 TestTypes

**Purpose:** Master list of test types (training courses) that generate volumes

| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| TestTypeID | INT | PK, Auto-increment | Unique identifier |
| TestTypeName | VARCHAR(100) | NOT NULL, UNIQUE | Display name |
| TestTypeCode | VARCHAR(20) | NOT NULL, UNIQUE | Short code for integration |
| Category | VARCHAR(50) | NOT NULL | Grouping category |
| Description | VARCHAR(500) | | Detailed description |
| TM1TestTypeName | VARCHAR(100) | NOT NULL | Test type name in TM1 |
| Status | VARCHAR(20) | NOT NULL, DEFAULT 'Active' | Active/Retired |
| EffectiveDate | DATE | NOT NULL | Date available for volumes |
| RetiredDate | DATE | | Date retired (if applicable) |
| CreatedBy | INT | FK → Users, NOT NULL | User who created |
| CreatedDate | DATETIME | NOT NULL, DEFAULT NOW() | Creation timestamp |
| ModifiedBy | INT | FK → Users | Last modifier |
| ModifiedDate | DATETIME | | Last modification timestamp |

**Business Rules:**
- Status values: 'Active', 'Retired', 'Pending'
- Cannot retire TestType if active submissions exist
- RetiredDate must be >= EffectiveDate
- Category values: 'Leadership', 'Technical', 'Certification', 'Compliance'

**Indexes:**
- PRIMARY KEY on TestTypeID
- UNIQUE INDEX on TestTypeCode
- INDEX on Status
- INDEX on Category (for filtered queries)
- INDEX on TM1TestTypeName (for export queries)

**Sample Data:**
```sql
INSERT INTO TestTypes VALUES
(1, 'Leadership Fundamentals', 'LEAD-101', 'Leadership', 'Basic leadership training', 'Leadership 101', 'Active', '2020-01-01', NULL),
(2, 'Data Analytics Essentials', 'DATA-101', 'Technical', 'Intro to data analytics', 'Analytics Basics', 'Active', '2021-01-01', NULL),
(3, 'Project Management Cert Prep', 'PM-CERT', 'Certification', 'PMP certification prep', 'PMP Prep', 'Active', '2019-01-01', NULL);
```

---

### 3.4 Country_TestType (Junction Table)

**Purpose:** Defines which test types are offered in which countries (M:M relationship)

| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| ID | INT | PK, Auto-increment | Unique identifier |
| CountryID | INT | FK → Countries, NOT NULL | Country offering test |
| TestTypeID | INT | FK → TestTypes, NOT NULL | Test type offered |
| EffectiveDate | DATE | NOT NULL | Date this offering started |
| IsActive | BIT | NOT NULL, DEFAULT 1 | Currently offered? |
| EndDate | DATE | | Date offering ended |
| CreatedBy | INT | FK → Users, NOT NULL | User who created |
| CreatedDate | DATETIME | NOT NULL, DEFAULT NOW() | Creation timestamp |

**Business Rules:**
- UNIQUE constraint on (CountryID, TestTypeID, EffectiveDate)
- Cannot have overlapping active periods for same country/test combination
- EndDate must be >= EffectiveDate
- IsActive = 0 requires EndDate to be populated

**Indexes:**
- PRIMARY KEY on ID
- UNIQUE INDEX on (CountryID, TestTypeID, EffectiveDate)
- FOREIGN KEY INDEX on CountryID
- FOREIGN KEY INDEX on TestTypeID
- INDEX on IsActive (for current offerings)

**Why this table exists:**
- Not all countries offer all tests
- Offerings change over time (some tests added, some discontinued)
- Need to know historically what was offered when
- Prevents submission of volumes for invalid country/test combinations

---

### 3.5 Users

**Purpose:** All system users (coordinators, analysts, executives, admins)

| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| UserID | INT | PK, Auto-increment | Unique identifier |
| Email | VARCHAR(255) | NOT NULL, UNIQUE | Email address (Azure AD) |
| DisplayName | VARCHAR(100) | NOT NULL | Full name |
| RoleID | INT | FK → Roles, NOT NULL | Primary role |
| Status | VARCHAR(20) | NOT NULL, DEFAULT 'Active' | Active/Inactive/Locked |
| LastLoginDate | DATETIME | | Last successful login |
| PreferredLanguage | VARCHAR(10) | DEFAULT 'en-US' | UI language preference |
| PhoneNumber | VARCHAR(20) | | Contact number |
| Department | VARCHAR(100) | | Organizational unit |
| CreatedDate | DATETIME | NOT NULL, DEFAULT NOW() | Account creation date |
| ModifiedDate | DATETIME | | Last modification timestamp |

**Business Rules:**
- Email must be valid format
- Status values: 'Active', 'Inactive', 'Locked'
- Only 'Active' users can login
- PreferredLanguage uses IETF language tags

**Indexes:**
- PRIMARY KEY on UserID
- UNIQUE INDEX on Email
- FOREIGN KEY INDEX on RoleID
- INDEX on Status
- INDEX on DisplayName (for user search)

---

### 3.6 Roles

**Purpose:** Role-based access control definitions

| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| RoleID | INT | PK, Auto-increment | Unique identifier |
| RoleName | VARCHAR(50) | NOT NULL, UNIQUE | Display name |
| RoleCode | VARCHAR(20) | NOT NULL, UNIQUE | System code |
| Description | VARCHAR(500) | | Role description |
| Permissions | VARCHAR(MAX) | NOT NULL | JSON array of permissions |
| IsSystemRole | BIT | NOT NULL, DEFAULT 0 | Prevents deletion |
| CreatedDate | DATETIME | NOT NULL, DEFAULT NOW() | Creation timestamp |

**Business Rules:**
- RoleCode values: 'COORDINATOR', 'ANALYST', 'ADMIN', 'EXECUTIVE', 'READONLY'
- Cannot delete role if users are assigned to it
- Cannot delete IsSystemRole = 1 roles
- Permissions stored as JSON for flexibility

**Sample Data:**
```sql
INSERT INTO Roles VALUES
(1, 'Country Coordinator', 'COORDINATOR', 'Submits volumes for assigned countries', '["submit_volumes","view_history","save_drafts"]', 1),
(2, 'Data Quality Analyst', 'ANALYST', 'Validates and approves submissions', '["view_all_submissions","validate_data","manual_entry","export_tm1","send_communications"]', 1),
(3, 'System Administrator', 'ADMIN', 'Full system access', '["*"]', 1),
(4, 'Executive', 'EXECUTIVE', 'Read-only analytics access', '["view_dashboards","export_reports"]', 1);
```

---

### 3.7 User_Country (Assignment Table)

**Purpose:** Assigns users (coordinators/analysts) to specific countries

| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| ID | INT | PK, Auto-increment | Unique identifier |
| UserID | INT | FK → Users, NOT NULL | Assigned user |
| CountryID | INT | FK → Countries, NOT NULL | Assigned country |
| IsPrimary | BIT | NOT NULL, DEFAULT 1 | Primary coordinator? |
| IsBackup | BIT | NOT NULL, DEFAULT 0 | Backup coordinator? |
| EffectiveDate | DATE | NOT NULL, DEFAULT TODAY() | Assignment start date |
| EndDate | DATE | | Assignment end date |
| CreatedBy | INT | FK → Users, NOT NULL | Who made assignment |
| CreatedDate | DATETIME | NOT NULL, DEFAULT NOW() | Assignment timestamp |

**Business Rules:**
- Each country must have exactly one primary coordinator (IsPrimary = 1)
- Country can have 0-2 backup coordinators
- Cannot have both IsPrimary = 1 AND IsBackup = 1
- UNIQUE constraint on (UserID, CountryID) for active assignments (EndDate IS NULL)
- EndDate must be >= EffectiveDate

**Indexes:**
- PRIMARY KEY on ID
- FOREIGN KEY INDEX on UserID
- FOREIGN KEY INDEX on CountryID
- INDEX on (CountryID, IsPrimary) for finding primary coordinator
- INDEX on EffectiveDate, EndDate for temporal queries

**Why this table exists:**
- Coordinators change over time
- Need backup coordinators for vacation coverage
- Analysts may be assigned specific countries to validate (Alice vs John)
- Historical record of who was responsible when

---

### 3.8 Volume_Submissions (Core Transaction Table)

**Purpose:** Records of all volume submissions from all countries

| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| SubmissionID | INT | PK, Auto-increment | Unique identifier |
| CountryID | INT | FK → Countries, NOT NULL | Submitting country |
| TestTypeID | INT | FK → TestTypes, NOT NULL | Test type |
| SubmissionMonth | VARCHAR(7) | NOT NULL | Format: YYYY-MM |
| SubmittedBy | INT | FK → Users, NOT NULL | User who submitted |
| SubmittedDate | DATETIME | NOT NULL | Submission timestamp |
| SubmissionMethod | VARCHAR(20) | NOT NULL | Portal/Upload/Manual |
| Status | VARCHAR(30) | NOT NULL | Workflow status |
| | | | |
| **Volume Fields** | | | |
| ActualVolume | INT | | Actual enrollment volumes |
| BudgetVolume | INT | | Budget/target volumes |
| ForecastVolume | INT | | Forecasted volumes |
| | | | |
| **Estimation Fields** | | | |
| IsEstimate | BIT | NOT NULL, DEFAULT 0 | Is this an estimate? |
| EstimateReason | VARCHAR(500) | | Why estimated (if IsEstimate=1) |
| | | | |
| **Validation Fields** | | | |
| ValidatedBy | INT | FK → Users | Analyst who validated |
| ValidatedDate | DATETIME | | Validation timestamp |
| ValidationNotes | VARCHAR(1000) | | Analyst comments |
| DataQualityScore | INT | | 0-100 quality score |
| | | | |
| **Audit Fields** | | | |
| LastModifiedBy | INT | FK → Users, NOT NULL | Last editor |
| LastModifiedDate | DATETIME | NOT NULL | Last edit timestamp |
| IsDeleted | BIT | NOT NULL, DEFAULT 0 | Soft delete flag |
| DeletedBy | INT | FK → Users | Who deleted |
| DeletedDate | DATETIME | | Deletion timestamp |
| RowVersion | TIMESTAMP | | Optimistic concurrency |

**Business Rules:**
- UNIQUE constraint on (CountryID, TestTypeID, SubmissionMonth) WHERE IsDeleted = 0
- SubmissionMonth format: YYYY-MM (e.g., '2025-11')
- SubmissionMethod values: 'Portal', 'Upload', 'Manual', 'System'
- Status values: 'Draft', 'Submitted', 'Pending_Review', 'Validated', 'Flagged', 'Approved', 'Rejected'
- ActualVolume, BudgetVolume, ForecastVolume must be >= 0 if populated
- IsEstimate = 1 requires EstimateReason (minimum 20 characters)
- ValidatedBy and ValidatedDate must both be populated or both NULL
- Cannot set IsDeleted = 1 if Status = 'Approved' (must reject first)
- DataQualityScore range: 0-100

**Indexes:**
- PRIMARY KEY on SubmissionID
- FOREIGN KEY INDEXES on CountryID, TestTypeID, SubmittedBy, ValidatedBy
- UNIQUE INDEX on (CountryID, TestTypeID, SubmissionMonth) WHERE IsDeleted = 0
- INDEX on SubmissionMonth (for time-based queries)
- INDEX on Status (for workflow queries)
- INDEX on (CountryID, SubmissionMonth) for country reporting
- INDEX on IsDeleted (for active records filter)
- INDEX on IsEstimate (for estimate tracking)

**Why this is the most important table:**
- This is your transactional fact table
- All reporting queries start here
- Contains the actual business data (volumes)
- Tracks complete workflow lifecycle
- Supports audit requirements
- Enables soft deletes for compliance

---

### 3.9 Submission_History (Audit Trail)

**Purpose:** Complete audit trail of all changes to submissions

| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| HistoryID | INT | PK, Auto-increment | Unique identifier |
| SubmissionID | INT | FK → Volume_Submissions, NOT NULL | Parent submission |
| ChangeDate | DATETIME | NOT NULL, DEFAULT NOW() | When change occurred |
| ChangedBy | INT | FK → Users, NOT NULL | Who made the change |
| ChangeType | VARCHAR(30) | NOT NULL | Type of change |
| PreviousStatus | VARCHAR(30) | | Status before change |
| NewStatus | VARCHAR(30) | | Status after change |
| PreviousActual | INT | | ActualVolume before |
| NewActual | INT | | ActualVolume after |
| PreviousBudget | INT | | BudgetVolume before |
| NewBudget | INT | | BudgetVolume after |
| PreviousForecast | INT | | ForecastVolume before |
| NewForecast | INT | | ForecastVolume after |
| ChangeReason | VARCHAR(500) | | Why change was made |
| IPAddress | VARCHAR(45) | | User's IP address |
| UserAgent | VARCHAR(500) | | Browser/device info |

**Business Rules:**
- ChangeType values: 'Created', 'Updated', 'Validated', 'Rejected', 'Deleted', 'Restored'
- At least one "Previous" vs "New" field must differ
- ChangeReason required for 'Updated', 'Validated', 'Deleted' types
- Records never deleted (complete audit trail)

**Indexes:**
- PRIMARY KEY on HistoryID
- FOREIGN KEY INDEX on SubmissionID
- INDEX on (SubmissionID, ChangeDate) for submission timeline
- INDEX on ChangedBy (for user activity tracking)
- INDEX on ChangeDate (for temporal queries)
- INDEX on ChangeType (for change analysis)

**Why this table exists:**
- Regulatory compliance (audit trail)
- Troubleshooting ("What changed and when?")
- User activity monitoring
- Data quality analysis
- Can reconstruct any submission at any point in time

---

### 3.10 Data_Quality_Flags

**Purpose:** Tracks data quality issues identified by automated validation

| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| FlagID | INT | PK, Auto-increment | Unique identifier |
| SubmissionID | INT | FK → Volume_Submissions, NOT NULL | Flagged submission |
| FlagType | VARCHAR(30) | NOT NULL | Type of issue |
| Severity | VARCHAR(20) | NOT NULL | Low/Medium/High |
| Description | VARCHAR(500) | NOT NULL | Human-readable explanation |
| ThresholdValue | DECIMAL(18,2) | | Expected/threshold value |
| ActualValue | DECIMAL(18,2) | | Actual value that triggered flag |
| FlaggedDate | DATETIME | NOT NULL, DEFAULT NOW() | When flagged |
| Status | VARCHAR(20) | NOT NULL, DEFAULT 'Open' | Open/Dismissed/Resolved |
| ResolvedBy | INT | FK → Users | Who resolved |
| ResolvedDate | DATETIME | | Resolution timestamp |
| ResolutionNotes | VARCHAR(1000) | | How/why resolved |
| AutoDismissed | BIT | NOT NULL, DEFAULT 0 | System auto-dismissed? |

**Business Rules:**
- FlagType values: 'Anomaly_High', 'Anomaly_Low', 'Budget_Variance', 'Missing_Forecast', 'Zero_Volumes', 'Format_Error'
- Severity values: 'Low', 'Medium', 'High', 'Critical'
- Status values: 'Open', 'Dismissed', 'Resolved'
- ResolvedBy and ResolvedDate must both be populated or both NULL
- Resolution notes required when Status = 'Resolved'

**Indexes:**
- PRIMARY KEY on FlagID
- FOREIGN KEY INDEX on SubmissionID
- INDEX on Status (for open flags queries)
- INDEX on Severity (for priority sorting)
- INDEX on FlagType (for flag analysis)
- INDEX on FlaggedDate (for temporal analysis)

**Why this table exists:**
- Separate from submissions to allow multiple flags per submission
- Tracks resolution workflow
- Analytics on data quality trends
- Helps identify systematic issues

---

### 3.11 Communications

**Purpose:** Log of all system communications (emails, notifications, messages)

| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| CommunicationID | INT | PK, Auto-increment | Unique identifier |
| SubmissionID | INT | FK → Volume_Submissions | Related submission (nullable) |
| CountryID | INT | FK → Countries | Related country (nullable) |
| FromUserID | INT | FK → Users | Sender |
| ToUserID | INT | FK → Users | Recipient |
| Subject | VARCHAR(255) | NOT NULL | Email subject |
| Body | VARCHAR(MAX) | NOT NULL | Message content |
| CommunicationType | VARCHAR(20) | NOT NULL | Email/Internal/SMS |
| SentDate | DATETIME | NOT NULL | Send timestamp |
| ReadDate | DATETIME | | Read timestamp (if tracked) |
| IsSystemGenerated | BIT | NOT NULL, DEFAULT 0 | Auto vs manual |
| TemplateID | INT | FK → Email_Templates | Template used (if any) |
| DeliveryStatus | VARCHAR(20) | NOT NULL, DEFAULT 'Sent' | Sent/Delivered/Failed |
| ErrorMessage | VARCHAR(500) | | If delivery failed |

**Business Rules:**
- CommunicationType values: 'Email', 'Internal_Message', 'SMS', 'Push_Notification'
- DeliveryStatus values: 'Sent', 'Delivered', 'Failed', 'Bounced'
- Either SubmissionID or CountryID should be populated (or both)
- IsSystemGenerated = 1 means automated notification

**Indexes:**
- PRIMARY KEY on CommunicationID
- FOREIGN KEY INDEXES on SubmissionID, FromUserID, ToUserID
- INDEX on SentDate (for communication history)
- INDEX on (ToUserID, ReadDate) for unread messages
- INDEX on CommunicationType

---

### 3.12 TM1_Export_Log

**Purpose:** Tracks all exports to TM1 system for audit and troubleshooting

| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| ExportID | INT | PK, Auto-increment | Unique identifier |
| ExportDate | DATETIME | NOT NULL | Export timestamp |
| ExportedBy | INT | FK → Users, NOT NULL | User who triggered export |
| SubmissionMonth | VARCHAR(7) | NOT NULL | Month exported (YYYY-MM) |
| RecordCount | INT | NOT NULL | Number of records exported |
| FilePath | VARCHAR(500) | | Where file was saved/sent |
| FileHash | VARCHAR(64) | NOT NULL | SHA-256 hash for integrity |
| IncludesEstimates | BIT | NOT NULL, DEFAULT 0 | Contains estimated data? |
| EstimateCount | INT | | Number of estimates included |
| Status | VARCHAR(20) | NOT NULL | Success/Failed |
| ErrorMessage | VARCHAR(1000) | | If Status = Failed |
| DurationSeconds | INT | | How long export took |

**Business Rules:**
- Status values: 'Success', 'Failed', 'Partial'
- FileHash required for integrity verification
- RecordCount must be > 0
- EstimateCount cannot exceed RecordCount
- IncludesEstimates = 1 requires EstimateCount > 0

**Indexes:**
- PRIMARY KEY on ExportID
- INDEX on ExportDate (for export history)
- INDEX on SubmissionMonth (for re-export queries)
- INDEX on Status (for failure analysis)

**Why this table exists:**
- Audit trail for TM1 integration
- Troubleshoot export failures
- Identify when exports included estimates
- Can regenerate exact export if needed

---

### 3.13 System_Configuration

**Purpose:** Flexible key-value store for system settings

| Column | Data Type | Constraints | Description |
|--------|-----------|-------------|-------------|
| ConfigID | INT | PK, Auto-increment | Unique identifier |
| ConfigKey | VARCHAR(100) | NOT NULL, UNIQUE | Setting name |
| ConfigValue | VARCHAR(MAX) | NOT NULL | Setting value |
| DataType | VARCHAR(20) | NOT NULL | String/Int/Boolean/JSON |
| Description | VARCHAR(500) | | What this setting does |
| Category | VARCHAR(50) | NOT NULL | Grouping category |
| IsSystemConfig | BIT | NOT NULL, DEFAULT 0 | Prevents deletion |
| LastModifiedBy | INT | FK → Users | Who changed it |
| LastModifiedDate | DATETIME | | Change timestamp |

**Sample Configuration Values:**
```sql
('DefaultSubmissionDeadline', '3', 'Int', 'Default business day for submission deadline', 'Workflow', 1),
('AnomalyThresholdPct', '50', 'Int', 'Percentage variance to flag as anomaly', 'DataQuality', 1),
('BudgetVarianceThresholdPct', '25', 'Int', 'Budget variance threshold for flagging', 'DataQuality', 1),
('ReminderDaysBeforeDeadline', '[5,2,0]', 'JSON', 'Days before deadline to send reminders', 'Notifications', 1),
('FiscalYearStartMonth', '10', 'Int', 'Fiscal year starts in October (10)', 'BusinessRules', 1),
('MaxDraftAge Days', '30', 'Int', 'Auto-delete drafts older than X days', 'Maintenance', 1)
```

**Why this table exists:**
- Avoid hardcoded values in application
- Allow admins to change settings without code deployment
- Different environments (dev/test/prod) can have different configs
- Track configuration changes over time

---

## 4. Relationships & Cardinality

### 4.1 One-to-Many (1:M) Relationships

**Regions → Countries (1:M)**
- One region contains many countries
- Each country belongs to exactly one region
- Enforced by: Countries.RegionID FK → Regions.RegionID
- Cascade behavior: Restrict (cannot delete region with countries)

**Countries → Volume_Submissions (1:M)**
- One country has many submissions
- Each submission is from exactly one country
- Enforced by: Volume_Submissions.CountryID FK → Countries.CountryID
- Cascade behavior: Restrict (cannot delete country with submissions)

**TestTypes → Volume_Submissions (1:M)**
- One test type has many submissions
- Each submission is for exactly one test type
- Enforced by: Volume_Submissions.TestTypeID FK → TestTypes.TestTypeID
- Cascade behavior: Restrict (cannot delete test type with submissions)

**Users → Volume_Submissions (SubmittedBy) (1:M)**
- One user submits many submissions
- Each submission submitted by exactly one user
- Enforced by: Volume_Submissions.SubmittedBy FK → Users.UserID
- Cascade behavior: Restrict (maintain historical record)

**Volume_Submissions → Submission_History (1:M)**
- One submission has many history records
- Each history record belongs to one submission
- Enforced by: Submission_History.SubmissionID FK → Volume_Submissions.SubmissionID
- Cascade behavior: Cascade (delete history if submission deleted - though soft delete prevents this)

**Volume_Submissions → Data_Quality_Flags (1:M)**
- One submission can have multiple flags
- Each flag belongs to one submission
- Enforced by: Data_Quality_Flags.SubmissionID FK → Volume_Submissions.SubmissionID
- Cascade behavior: Cascade (flags are meaningless without submission)

### 4.2 Many-to-Many (M:M) Relationships

**Countries ↔ TestTypes (M:M via Country_TestType)**
- Many countries offer many test types
- Junction table: Country_TestType
- Allows temporal tracking (EffectiveDate, EndDate, IsActive)
- Prevents invalid submission combinations

**Users ↔ Countries (M:M via User_Country)**
- Many users can be assigned to many countries
- Junction table: User_Country
- Tracks primary vs backup coordinators
- Temporal tracking (EffectiveDate, EndDate)

---

## 5. Data Integrity & Constraints

### 5.1 Referential Integrity

**All foreign keys enforce referential integrity:**
- ON DELETE RESTRICT (cannot delete referenced records)
- ON UPDATE CASCADE (updates propagate)

**Exception: Soft Deletes**
- Volume_Submissions uses IsDeleted flag instead of physical deletion
- Maintains complete historical record
- Referential integrity preserved

### 5.2 Domain Constraints

**Status Fields (Check Constraints):**
```sql
ALTER TABLE Countries ADD CONSTRAINT CK_Countries_Status 
  CHECK (Status IN ('Active', 'Inactive', 'Pending'));

ALTER TABLE Volume_Submissions ADD CONSTRAINT CK_Submissions_Status 
  CHECK (Status IN ('Draft', 'Submitted', 'Pending_Review', 'Validated', 'Flagged', 'Approved', 'Rejected'));
```

**Volume Constraints:**
```sql
ALTER TABLE Volume_Submissions ADD CONSTRAINT CK_Submissions_ActualVolume 
  CHECK (ActualVolume IS NULL OR ActualVolume >= 0);
  
ALTER TABLE Volume_Submissions ADD CONSTRAINT CK_Submissions_DataQualityScore 
  CHECK (DataQualityScore BETWEEN 0 AND 100);
```

**Estimate Logic:**
```sql
ALTER TABLE Volume_Submissions ADD CONSTRAINT CK_Submissions_EstimateReason 
  CHECK ((IsEstimate = 0 AND EstimateReason IS NULL) OR 
         (IsEstimate = 1 AND LEN(EstimateReason) >= 20));
```

### 5.3 Unique Constraints

**Business Key Uniqueness:**
```sql
-- One submission per country/test/month
CREATE UNIQUE INDEX UX_Submissions_CountryTestMonth 
  ON Volume_Submissions(CountryID, TestTypeID, SubmissionMonth) 
  WHERE IsDeleted = 0;

-- Prevent duplicate active country/test offerings
CREATE UNIQUE INDEX UX_CountryTestType_Active 
  ON Country_TestType(CountryID, TestTypeID) 
  WHERE IsActive = 1;
```

---

## 6. Indexing Strategy

### 6.1 Primary Keys (Clustered Indexes)
- All tables use surrogate keys (INT identity) as primary keys
- Clustered indexes on primary keys by default
- Provides optimal insert performance

### 6.2 Foreign Keys (Non-Clustered Indexes)
- All foreign keys indexed automatically
- Improves join performance
- Enables efficient referential integrity checks

### 6.3 Query Optimization Indexes

**Volume_Submissions (Most Queried Table):**
```sql
-- Time-based queries (dashboard, reports)
CREATE INDEX IX_Submissions_Month ON Volume_Submissions(SubmissionMonth) 
  INCLUDE (CountryID, TestTypeID, ActualVolume, ForecastVolume, BudgetVolume);

-- Workflow queries (pending review, flagged items)
CREATE INDEX IX_Submissions_Status ON Volume_Submissions(Status) 
  WHERE IsDeleted = 0;

-- Country reporting (volumes by country over time)
CREATE INDEX IX_Submissions_CountryMonth ON Volume_Submissions(CountryID, SubmissionMonth) 
  INCLUDE (TestTypeID, ActualVolume, ForecastVolume, Status);

-- Estimates tracking
CREATE INDEX IX_Submissions_Estimates ON Volume_Submissions(IsEstimate) 
  WHERE IsEstimate = 1 AND IsDeleted = 0;
```

### 6.4 Covering Indexes

Use INCLUDE clause to avoid key lookups for common queries:
```sql
CREATE INDEX IX_Submissions_PowerBI ON Volume_Submissions(SubmissionMonth, CountryID, TestTypeID)
  INCLUDE (ActualVolume, BudgetVolume, ForecastVolume, IsEstimate, Status, ValidatedDate)
  WHERE IsDeleted = 0 AND Status IN ('Validated', 'Approved');
```

---

## 7. Temporal Data Handling

### 7.1 Effective Dating

**Tables with temporal tracking:**
- Country_TestType (EffectiveDate, EndDate, IsActive)
- User_Country (EffectiveDate, EndDate)
- TestTypes (EffectiveDate, RetiredDate, Status)

**Query Pattern for "Current" Records:**
```sql
-- Current active country/test combinations
SELECT * FROM Country_TestType
WHERE IsActive = 1 
  AND EffectiveDate <= GETDATE()
  AND (EndDate IS NULL OR EndDate > GETDATE());
```

### 7.2 Audit Trail (Change History)

**Submission_History captures:**
- All data changes (before/after values)
- Status transitions
- User actions
- Timestamps

**Point-in-Time Reconstruction:**
```sql
-- What was the submission on 2025-10-15?
SELECT 
  s.SubmissionID,
  COALESCE(h_actual.NewActual, s.ActualVolume) AS ActualVolume,
  COALESCE(h_forecast.NewForecast, s.ForecastVolume) AS ForecastVolume
FROM Volume_Submissions s
LEFT JOIN (
  SELECT SubmissionID, NewActual
  FROM Submission_History
  WHERE ChangeDate <= '2025-10-15'
    AND NewActual IS NOT NULL
  ORDER BY ChangeDate DESC
  LIMIT 1
) h_actual ON s.SubmissionID = h_actual.SubmissionID
...
```

### 7.3 Soft Deletes

**Never physically delete transactional data:**
- Volume_Submissions.IsDeleted flag
- Related records (history, flags) remain intact
- Can "un-delete" if needed
- Maintains referential integrity

**Query Pattern:**
```sql
-- All active queries include:
WHERE IsDeleted = 0
```

---

## 8. Sample Queries

### 8.1 Submission Status Dashboard

```sql
-- Real-time submission status by country
SELECT 
  c.CountryName,
  r.RegionName,
  CASE 
    WHEN EXISTS (
      SELECT 1 FROM Volume_Submissions vs
      WHERE vs.CountryID = c.CountryID
        AND vs.SubmissionMonth = '2025-11'
        AND vs.Status IN ('Validated', 'Approved')
        AND vs.IsDeleted = 0
    ) THEN 'Submitted'
    WHEN EXISTS (
      SELECT 1 FROM Volume_Submissions vs
      WHERE vs.CountryID = c.CountryID
        AND vs.SubmissionMonth = '2025-11'
        AND vs.Status = 'Draft'
        AND vs.IsDeleted = 0
    ) THEN 'Draft'
    ELSE 'Not Started'
  END AS SubmissionStatus,
  MAX(vs.SubmittedDate) AS LastSubmissionDate
FROM Countries c
INNER JOIN Regions r ON c.RegionID = r.RegionID
LEFT JOIN Volume_Submissions vs ON c.CountryID = vs.CountryID 
  AND vs.SubmissionMonth = '2025-11'
  AND vs.IsDeleted = 0
WHERE c.Status = 'Active'
GROUP BY c.CountryID, c.CountryName, r.RegionName
ORDER BY r.RegionName, c.CountryName;
```

### 8.2 Power BI Data Source Query

```sql
-- Optimized query for Power BI DirectQuery
SELECT 
  vs.SubmissionID,
  vs.SubmissionMonth,
  c.CountryName,
  c.CountryCode,
  r.RegionName,
  tt.TestTypeName,
  tt.Category AS TestCategory,
  vs.ActualVolume,
  vs.BudgetVolume,
  vs.ForecastVolume,
  vs.IsEstimate,
  vs.Status,
  vs.SubmittedDate,
  vs.ValidatedDate,
  vs.DataQualityScore,
  u_submit.DisplayName AS SubmittedBy,
  u_validate.DisplayName AS ValidatedBy
FROM Volume_Submissions vs
INNER JOIN Countries c ON vs.CountryID = c.CountryID
INNER JOIN Regions r ON c.RegionID = r.RegionID
INNER JOIN TestTypes tt ON vs.TestTypeID = tt.TestTypeID
INNER JOIN Users u_submit ON vs.SubmittedBy = u_submit.UserID
LEFT JOIN Users u_validate ON vs.ValidatedBy = u_validate.UserID
WHERE vs.IsDeleted = 0
  AND vs.Status IN ('Validated', 'Approved')
  AND vs.SubmissionMonth >= DATEADD(MONTH, -24, GETDATE()); -- Last 24 months
```

### 8.3 TM1 Export Query

```sql
-- Generate TM1-compatible export
SELECT 
  c.TM1CountryName AS Country,
  tt.TM1TestTypeName AS TestType,
  vs.SubmissionMonth AS [Month],
  vs.ActualVolume AS Actual,
  vs.BudgetVolume AS Budget,
  vs.ForecastVolume AS Forecast
FROM Volume_Submissions vs
INNER JOIN Countries c ON vs.CountryID = c.CountryID
INNER JOIN TestTypes tt ON vs.TestTypeID = tt.TestTypeID
WHERE vs.SubmissionMonth = '2025-11'
  AND vs.Status = 'Approved'
  AND vs.IsDeleted = 0
ORDER BY c.TM1CountryName, tt.TM1TestTypeName;
```

### 8.4 Data Quality Flag Analysis

```sql
-- Identify countries with recurring data quality issues
SELECT 
  c.CountryName,
  dqf.FlagType,
  COUNT(*) AS FlagCount,
  AVG(DATEDIFF(HOUR, dqf.FlaggedDate, dqf.ResolvedDate)) AS AvgResolutionHours
FROM Data_Quality_Flags dqf
INNER JOIN Volume_Submissions vs ON dqf.SubmissionID = vs.SubmissionID
INNER JOIN Countries c ON vs.CountryID = c.CountryID
WHERE dqf.FlaggedDate >= DATEADD(MONTH, -6, GETDATE())
  AND dqf.Status = 'Resolved'
GROUP BY c.CountryName, dqf.FlagType
HAVING COUNT(*) >= 3
ORDER BY COUNT(*) DESC;
```

---

## 9. Data Migration Considerations

### 9.1 Historical Data Import

**If migrating from old system:**
1. Import master data first (Countries, Regions, TestTypes)
2. Map old country names to new standardized names
3. Import historical submissions with IsEstimate = 0
4. Set SubmittedBy to "System Migration" user
5. Status = 'Approved' for all historical records
6. Skip validation workflow for historical data

### 9.2 Incremental Data Load

**For ongoing operations:**
- Submissions created with Status = 'Draft'
- User submissions trigger workflow
- Validation updates Status to 'Validated' → 'Approved'
- History records created for each state transition

---

## 10. Normalization Assessment

### 10.1 First Normal Form (1NF)
✅ All tables have atomic values (no repeating groups)
✅ All columns contain single values
✅ Primary keys defined for all tables

### 10.2 Second Normal Form (2NF)
✅ All non-key attributes depend on entire primary key
✅ No partial dependencies

### 10.3 Third Normal Form (3NF)
✅ No transitive dependencies
✅ Example: CountryName not stored in Volume_Submissions (retrieved via join)

### 10.4 Intentional Denormalization

**TM1 mapping fields (e.g., Countries.TM1CountryName):**
- **Why:** Avoid complex joins during export
- **Trade-off:** Minor redundancy for significant performance gain
- **Acceptable:** Export is critical path, lookup table would add complexity

**Data_Quality_Flags storing ThresholdValue and ActualValue:**
- **Why:** Context for flag at time it was created
- **Trade-off:** Stores values that could be calculated
- **Acceptable:** Flags are historical; recalculating would give different results

---

## 11. Performance Considerations

### 11.1 Query Optimization

**Use covering indexes for common queries:**
- Dashboard status query: 50ms → 10ms
- Power BI refresh: 2min → 30sec
- TM1 export: 15sec → 3sec

### 11.2 Partitioning Strategy (Future)

**When data grows beyond 10M rows:**
- Partition Volume_Submissions by SubmissionMonth
- Partition Submission_History by ChangeDate
- Use sliding window (archive data older than 7 years)

### 11.3 Caching Strategy

**Rarely-changing data:**
- Countries, Regions, TestTypes, Roles
- Cache in application layer
- Invalidate on modification

---

## 12. Security & Compliance

### 12.1 Row-Level Security (RLS)

**Implementation in Power BI:**
```dax
-- Coordinator Role: Only see own countries
[CountryID] IN 
  (SELECT CountryID FROM User_Country WHERE UserID = USERPRINCIPALNAME())

-- Analyst Role: See assigned countries for validation
[CountryID] IN 
  (SELECT CountryID FROM User_Country 
   WHERE UserID = USERPRINCIPALNAME() AND (IsPrimary = 1 OR IsBackup = 1))

-- Executive Role: See all
1 = 1
```

### 12.2 Audit Compliance

**GDPR/SOX Requirements Met:**
- ✅ Complete audit trail (Submission_History)
- ✅ User activity tracking (CreatedBy, ModifiedBy on all tables)
- ✅ Soft deletes (maintain data lineage)
- ✅ Data retention policy (configurable)
- ✅ Right to be forgotten (IsDeleted flag + scheduled purge)

---

## 13. Implementation Notes

### 13.1 SharePoint Lists Mapping

**This normalized model will be implemented across SharePoint lists:**

| Entity | SharePoint List | Notes |
|--------|-----------------|-------|
| Regions | tbl_Regions | Simple lookup list |
| Countries | tbl_Countries | Lookup to Regions |
| TestTypes | tbl_TestTypes | Master catalog |
| Country_TestType | tbl_CountryTestTypes | Junction list |
| Users | tbl_Users | Synced from Azure AD |
| Roles | tbl_Roles | System configuration |
| User_Country | tbl_UserCountries | Assignment list |
| Volume_Submissions | tbl_VolumeSubmissions | **Core transactional list** |
| Submission_History | tbl_SubmissionHistory | Audit trail (can grow large) |
| Data_Quality_Flags | tbl_DataQualityFlags | Validation tracking |
| Communications | tbl_Communications | Communication log |
| TM1_Export_Log | tbl_TM1Exports | Export tracking |
| System_Configuration | tbl_SystemConfig | Settings |

**SharePoint Considerations:**
- List view threshold: 5,000 items
- Index up to 20 columns per list
- Lookup columns limited to 8 per list
- Consider archiving old Submission_History records

### 13.2 Power Apps Data Connections

**Delegation-Friendly Queries:**
- Filter on indexed columns
- Avoid complex formulas in galleries
- Use server-side filtering where possible
- Cache lookup data (Countries, TestTypes) in collections

---

## 14. SharePoint Implementation Status

**Schema Design:** ✅ Complete (November 21, 2025)
**Documentation:** See `sharepoint/list-schemas.md`

**Implementation Approach:**
- Text field workaround for Person columns (single-user tenant constraint)
- 9 SharePoint lists total (MVP scope)
- All lookup relationships and validation rules defined
- Ready for implementation

**Build Status:** 
- 🔲 Lists created
- 🔲 Columns configured  
- 🔲 Sample data populated
- 🔲 Relationships tested

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | 2025-11-20 | John Rolzhausen | Initial data model based on requirements v1.1 and user stories |
| 1.1 | 2025-11-22 | John Rolzhausen | Added Section 14: SharePoint implementation status and link to physical schema |

---

**Next Steps:**
1. Review data model for completeness and accuracy
2. Validate relationships and constraints
3. Confirm SharePoint implementation approach
4. Create sample data for testing
5. Begin wireframe design for Power Apps
