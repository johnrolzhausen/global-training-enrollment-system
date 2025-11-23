# SharePoint Schema Specifications
## Global Training Enrollment System

**Version:** 1.0  
**Date:** November 22, 2025  
**Author:** John Rolzhausen  
**Status:** Ready for Implementation

---

## 1. Overview

This document provides the complete physical implementation specifications for all SharePoint lists in the Global Training Enrollment System. Each list includes exact column definitions, data types, validation rules, and indexing requirements ready for implementation.

**Design Principles:**
- Normalized schema with appropriate denormalization for performance
- Text field workarounds for Person columns (single-user tenant constraint)
- Comprehensive validation at both SharePoint and Power Apps layers
- Indexing strategy for query performance
- Sample data templates for testing

**Related Documentation:**
- Logical data model: See `docs/03-data-model.md`
- Requirements: See `docs/02-requirements.md`
- User stories: See `docs/04-user-stories.md`

---

## 2. Important: Text Field Workaround

### Single-User Tenant Constraint

Due to Microsoft 365 Business Standard single-user license limitations, Person columns cannot be used for multi-user scenarios in the development environment.

**Workaround Implemented:**
- Replace Person columns with Text fields (Email + Name)
- Simulate user context in Power Apps with dropdown selection
- Document this as development constraint

**Example:**
```
Instead of:
  SubmittedBy (Person column) → Azure AD

We use:
  SubmittedByEmail (Single line text)
  SubmittedByName (Single line text)
```

**Production Implementation Note:**
In a production environment with multiple licensed users, these text fields would be replaced with Person columns that integrate directly with Azure AD for automatic user identification and role-based access control.

**Employer Context:**
This demonstrates practical problem-solving within development constraints while maintaining functional requirements and documenting architectural decisions for future production deployment.

---

## 3. SharePoint Lists Summary

| # | List Name | Purpose | Estimated Rows | Priority |
|---|-----------|---------|----------------|----------|
| 1 | tbl_Regions | Geographic regions | 5-10 | MVP |
| 2 | tbl_Countries | Country master data | 45 | MVP |
| 3 | tbl_TestTypes | Test type catalog | 10-50 | MVP |
| 4 | tbl_CountryTestTypes | Country/test offerings | 200-500 | MVP |
| 5 | tbl_UserCountries | User assignments | 50-100 | MVP |
| 6 | tbl_VolumeSubmissions | Core transactions | 5,000-50,000 | MVP |
| 7 | tbl_SubmissionHistory | Audit trail | 20,000-200,000 | MVP |
| 8 | tbl_DataQualityFlags | Validation tracking | 500-5,000 | MVP |
| 9 | tbl_SystemConfig | System settings | 10-20 | MVP |

**Build Order:** Lists ordered from simplest to most complex for incremental validation.

---

## 4. List Schemas

---

### 4.1 tbl_Regions

**Purpose:** Master list of geographic regions for grouping countries.

**Create List:**
- Site: Your SharePoint site
- Template: Custom List
- Name: `tbl_Regions`
- Description: "Geographic regions for country grouping"

**Columns to Create:**

| Column Name | Type | Required | Settings | Validation |
|-------------|------|----------|----------|------------|
| Title | Single line text | Yes | Max length: 100, Default display field | - |
| RegionCode | Single line text | Yes | Max length: 10 | `=AND(LEN([RegionCode])>=2, LEN([RegionCode])<=10)` |
| Status | Choice | Yes | Default: Active | - |
| Notes | Multiple lines of text | No | Plain text, 3 lines | - |

**Choice Column Settings:**

**Status:**
```
Choices:
  Active
  Inactive

Display: Dropdown
Default: Active
Allow fill-in: No
```

**Indexed Columns:**
1. Title (automatic primary key)
2. RegionCode - Settings → Indexed → Yes, Enforce unique values
3. Status - Settings → Indexed → Yes

**Validation Formula (List level):**
None required - column-level validation sufficient.

**Sample Data to Add:**

| Title | RegionCode | Status | Notes |
|-------|------------|--------|-------|
| Asia Pacific | APAC | Active | Includes East Asia and Southeast Asia |
| Western Europe | WE | Active | EU countries and UK |
| North America | NA | Active | US, Canada, Mexico |
| Eastern Europe | EE | Active | Eastern EU and Russia |
| Latin America | LATAM | Active | Central and South America |

**Build Checklist:**
- [ ] List created with description
- [ ] All columns added with correct types
- [ ] RegionCode set as unique indexed
- [ ] Status indexed
- [ ] Choice values configured correctly
- [ ] Sample data added (5 rows)
- [ ] Test: Try to add duplicate RegionCode (should fail)

---

### 4.2 tbl_SystemConfig

**Purpose:** Key-value store for system configuration settings.

**Create List:**
- Template: Custom List
- Name: `tbl_SystemConfig`
- Description: "System configuration and settings"

**Columns to Create:**

| Column Name | Type | Required | Settings | Validation |
|-------------|------|----------|----------|------------|
| Title | Single line text | Yes | Max length: 100, Unique | ConfigKey (setting name) |
| ConfigValue | Multiple lines of text | Yes | Plain text, 3 lines | - |
| DataType | Choice | Yes | Default: String | - |
| Description | Multiple lines of text | Yes | Plain text, 4 lines | What this setting does |
| Category | Choice | Yes | Default: Workflow | - |
| IsSystemConfig | Yes/No | Yes | Default: No | Prevents deletion of critical settings |

**Choice Column Settings:**

**DataType:**
```
Choices:
  String
  Int
  Boolean
  JSON

Display: Dropdown
Default: String
```

**Category:**
```
Choices:
  Workflow
  DataQuality
  Notifications
  BusinessRules
  Maintenance
  UI

Display: Dropdown
Default: Workflow
```

**Indexed Columns:**
1. Title (automatic, enforce unique)

**Sample Data to Add:**

| Title | ConfigValue | DataType | Description | Category | IsSystemConfig |
|-------|-------------|----------|-------------|----------|----------------|
| DefaultSubmissionDeadline | 3 | Int | Default business day for monthly deadline | Workflow | Yes |
| AnomalyThresholdPct | 50 | Int | Percentage variance to flag as anomaly | DataQuality | Yes |
| BudgetVarianceThresholdPct | 25 | Int | Budget variance threshold for flagging | DataQuality | Yes |
| ReminderDaysBeforeDeadline | [5,2,0] | JSON | Days before deadline to send reminders | Notifications | Yes |
| FiscalYearStartMonth | 10 | Int | Fiscal year starts in October | BusinessRules | Yes |
| MaxDraftAgeDays | 30 | Int | Auto-delete drafts older than X days | Maintenance | No |
| EnableEmailNotifications | true | Boolean | Send email notifications | Notifications | No |

**Build Checklist:**
- [ ] List created
- [ ] All columns added
- [ ] Choice values configured
- [ ] Title set to enforce unique values
- [ ] Sample data added (7 rows minimum)
- [ ] Test: Verify DataType and Category dropdowns work

---

### 4.3 tbl_TestTypes

**Purpose:** Master catalog of test types (training courses) available for volume submissions.

**Create List:**
- Template: Custom List
- Name: `tbl_TestTypes`
- Description: "Master catalog of test types and training courses"

**Columns to Create:**

| Column Name | Type | Required | Settings | Validation |
|-------------|------|----------|----------|------------|
| Title | Single line text | Yes | Max length: 100 | Test type name |
| TestTypeCode | Single line text | Yes | Max length: 20, Unique | `=AND(LEN([TestTypeCode])>=2, LEN([TestTypeCode])<=20)` |
| Category | Choice | Yes | - | - |
| Description | Multiple lines of text | No | Plain text, 4 lines | - |
| TM1TestTypeName | Single line text | Yes | Max length: 100 | TM1 system name |
| Status | Choice | Yes | Default: Active | - |
| EffectiveDate | Date | Yes | Date only, Default: Today | - |
| RetiredDate | Date | No | Date only | `=OR(ISBLANK([RetiredDate]), [RetiredDate]>=[EffectiveDate])` |
| Notes | Multiple lines of text | No | Plain text, 3 lines | - |

**Choice Column Settings:**

**Category:**
```
Choices:
  Leadership
  Technical
  Certification
  Compliance

Display: Dropdown
Allow fill-in: No
```

**Status:**
```
Choices:
  Active
  Retired
  Pending

Display: Dropdown
Default: Active
```

**Indexed Columns:**
1. Title (automatic)
2. TestTypeCode - Settings → Indexed → Yes, Enforce unique values
3. Status - Settings → Indexed → Yes
4. Category - Settings → Indexed → Yes

**Sample Data to Add:**

| Title | TestTypeCode | Category | TM1TestTypeName | Status | EffectiveDate | RetiredDate | Description |
|-------|--------------|----------|-----------------|--------|---------------|-------------|-------------|
| Leadership Fundamentals | LEAD-101 | Leadership | Leadership 101 | Active | 2020-01-01 | | Core leadership training program |
| Data Analytics Essentials | DATA-101 | Technical | Analytics Basics | Active | 2021-06-01 | | Introduction to data analytics |
| Project Management Cert Prep | PM-CERT | Certification | PMP Prep | Active | 2019-01-01 | | PMP certification preparation |
| Advanced AI Training | AI-201 | Technical | AI Advanced | Pending | 2026-01-01 | | Launches Q1 2026 |
| Old Compliance Training | COMP-OLD | Compliance | Compliance-2018 | Retired | 2018-01-01 | 2023-12-31 | Replaced by new version |

**Build Checklist:**
- [ ] List created
- [ ] All columns added with correct types
- [ ] TestTypeCode set as unique indexed
- [ ] Status and Category indexed
- [ ] Date validation formula added for RetiredDate
- [ ] Choice values configured
- [ ] Sample data added (5 rows)
- [ ] Test: Verify RetiredDate must be >= EffectiveDate

---

### 4.4 tbl_Countries

**Purpose:** Master list of all countries that submit enrollment volumes.

**Create List:**
- Template: Custom List
- Name: `tbl_Countries`
- Description: "Master list of countries submitting volumes"

**Columns to Create:**

| Column Name | Type | Required | Settings | Validation |
|-------------|------|----------|----------|------------|
| Title | Single line text | Yes | Max length: 100, Unique | Country name |
| CountryCode | Single line text | Yes | Max length: 3, Unique | `=LEN([CountryCode])=3` |
| RegionID | Lookup | Yes | List: tbl_Regions, Column: Title | - |
| TimezoneOffset | Number | Yes | Decimals: 1, Min: -12, Max: 14 | - |
| TM1CountryName | Single line text | Yes | Max length: 100 | TM1 system name |
| Status | Choice | Yes | Default: Active | - |
| SubmissionDeadlineDay | Number | No | Decimals: 0, Min: 1, Max: 28, Default: 3 | - |
| Notes | Multiple lines of text | No | Plain text, 3 lines | - |

**Lookup Column Settings:**

**RegionID:**
```
Get information from: tbl_Regions
In this column: Title
Add a column to show: RegionCode (optional, for display)
Enforce relationship behavior: Restrict Delete
```

**Choice Column Settings:**

**Status:**
```
Choices:
  Active
  Inactive
  Pending

Display: Dropdown
Default: Active
```

**Indexed Columns:**
1. Title (automatic, enforce unique)
2. CountryCode - Settings → Indexed → Yes, Enforce unique values
3. RegionID - Settings → Indexed → Yes
4. Status - Settings → Indexed → Yes

**Sample Data to Add:**

| Title | CountryCode | RegionID | TimezoneOffset | TM1CountryName | Status | SubmissionDeadlineDay | Notes |
|-------|-------------|----------|----------------|----------------|--------|-----------------------|-------|
| Republic of Korea | KOR | Asia Pacific | 9.0 | Korea | Active | 3 | Monthly password changes required |
| Japan | JPN | Asia Pacific | 9.0 | Japan | Active | 3 | Occasional file corruption issues |
| France | FRA | Western Europe | 1.0 | France | Active | 3 | Regional coordinator for Europe |
| United Kingdom | GBR | Western Europe | 0.0 | United Kingdom | Active | 3 | Brexit-related changes |
| United States | USA | North America | -5.0 | United States | Active | 3 | Multiple time zones |

**Build Checklist:**
- [ ] List created
- [ ] All columns added
- [ ] CountryCode set as unique indexed, length = 3
- [ ] RegionID lookup configured to tbl_Regions
- [ ] TimezoneOffset decimals set to 1
- [ ] Status and CountryCode indexed
- [ ] Sample data added (5 rows)
- [ ] Test: Verify lookup to Regions works
- [ ] Test: Try invalid CountryCode length (should fail)

---

### 4.5 tbl_CountryTestTypes

**Purpose:** Junction table defining which test types are offered in which countries (many-to-many relationship).

**Create List:**
- Template: Custom List
- Name: `tbl_CountryTestTypes`
- Description: "Defines which test types are offered in each country"

**Columns to Create:**

| Column Name | Type | Required | Settings | Validation |
|-------------|------|----------|----------|------------|
| Title | Single line text | Yes | Max length: 100 | Auto-calculated: "CountryCode-TestTypeCode" |
| CountryID | Lookup | Yes | List: tbl_Countries, Column: Title | - |
| TestTypeID | Lookup | Yes | List: tbl_TestTypes, Column: Title | - |
| EffectiveDate | Date | Yes | Date only, Default: Today | - |
| IsActive | Yes/No | Yes | Default: Yes | - |
| EndDate | Date | No | Date only | `=OR(ISBLANK([EndDate]), [EndDate]>=[EffectiveDate])` |
| Notes | Multiple lines of text | No | Plain text, 2 lines | - |

**Lookup Column Settings:**

**CountryID:**
```
Get information from: tbl_Countries
In this column: Title
Add a column to show: CountryCode
Enforce relationship behavior: Restrict Delete
```

**TestTypeID:**
```
Get information from: tbl_TestTypes
In this column: Title
Add a column to show: TestTypeCode
Enforce relationship behavior: Restrict Delete
```

**Indexed Columns:**
1. CountryID - Settings → Indexed → Yes
2. TestTypeID - Settings → Indexed → Yes
3. IsActive - Settings → Indexed → Yes

**Sample Data to Add:**

| Title | CountryID | TestTypeID | EffectiveDate | IsActive | EndDate | Notes |
|-------|-----------|------------|---------------|----------|---------|-------|
| KOR-LEAD101 | Republic of Korea | Leadership Fundamentals | 2020-01-01 | Yes | | Core offering since 2020 |
| KOR-DATA101 | Republic of Korea | Data Analytics Essentials | 2021-06-01 | Yes | | Launched mid-2021 |
| KOR-COMPOLD | Republic of Korea | Old Compliance Training | 2018-01-01 | No | 2023-12-31 | Replaced by new version |
| JPN-LEAD101 | Japan | Leadership Fundamentals | 2020-01-01 | Yes | | |
| FRA-PMCERT | France | Project Management Cert Prep | 2019-01-01 | Yes | | |

**Build Checklist:**
- [ ] List created
- [ ] All columns added
- [ ] CountryID lookup configured to tbl_Countries (show CountryCode)
- [ ] TestTypeID lookup configured to tbl_TestTypes (show TestTypeCode)
- [ ] EndDate validation formula added
- [ ] All three indexes created
- [ ] Sample data added (5 rows)
- [ ] Test: Verify both lookups populate correctly
- [ ] Test: Calculate Title as "CountryCode-TestTypeCode" manually for now

---

### 4.6 tbl_UserCountries

**Purpose:** Assignment table tracking which users are responsible for which countries (coordinators and analysts).

**Create List:**
- Template: Custom List
- Name: `tbl_UserCountries`
- Description: "User country assignments for coordinators and analysts"

**Columns to Create:**

| Column Name | Type | Required | Settings | Validation |
|-------------|------|----------|----------|------------|
| Title | Single line text | Yes | Max length: 200 | Auto-calculated: "UserName - CountryName" |
| UserEmail | Single line text | Yes | Max length: 255 | Email format |
| UserName | Single line text | Yes | Max length: 100 | Display name |
| CountryID | Lookup | Yes | List: tbl_Countries, Column: Title | - |
| IsPrimary | Yes/No | Yes | Default: Yes | Primary coordinator flag |
| IsBackup | Yes/No | Yes | Default: No | Backup coordinator flag |
| EffectiveDate | Date | Yes | Date only, Default: Today | Assignment start |
| EndDate | Date | No | Date only | `=OR(ISBLANK([EndDate]), [EndDate]>=[EffectiveDate])` |
| Notes | Multiple lines of text | No | Plain text, 2 lines | - |

**Lookup Column Settings:**

**CountryID:**
```
Get information from: tbl_Countries
In this column: Title
Add a column to show: CountryCode
Enforce relationship behavior: Restrict Delete
```

**Indexed Columns:**
1. CountryID - Settings → Indexed → Yes
2. UserEmail - Settings → Indexed → Yes
3. IsPrimary - Settings → Indexed → Yes

**Sample Data to Add:**

| Title | UserEmail | UserName | CountryID | IsPrimary | IsBackup | EffectiveDate | EndDate | Notes |
|-------|-----------|----------|-----------|-----------|----------|---------------|---------|-------|
| Ji-Won Kim - Korea | jiwon.kim@test.com | Ji-Won Kim | Republic of Korea | Yes | No | 2023-01-01 | | Primary coordinator since 2023 |
| Yuki Tanaka - Korea | yuki.tanaka@test.com | Yuki Tanaka | Republic of Korea | No | Yes | 2024-06-01 | | Backup for vacation coverage |
| Alice Chen - Korea | alice.chen@test.com | Alice Chen | Republic of Korea | No | No | 2023-01-01 | | Analyst validation assignment |
| Alice Chen - Japan | alice.chen@test.com | Alice Chen | Japan | No | No | 2023-01-01 | | Analyst validation assignment |
| Marie Dubois - France | marie.dubois@test.com | Marie Dubois | France | Yes | No | 2022-01-01 | | Primary coordinator |

**Build Checklist:**
- [ ] List created
- [ ] All columns added
- [ ] CountryID lookup configured
- [ ] EndDate validation formula added
- [ ] All three indexes created
- [ ] Sample data added (5 rows)
- [ ] Test: One user can have multiple country assignments
- [ ] Test: EndDate must be >= EffectiveDate

---

### 4.7 tbl_DataQualityFlags

**Purpose:** Tracks data quality issues flagged by automated validation or manual review.

**Create List:**
- Template: Custom List
- Name: `tbl_DataQualityFlags`
- Description: "Data quality flags and resolution tracking"

**Columns to Create:**

| Column Name | Type | Required | Settings | Validation |
|-------------|------|----------|----------|------------|
| Title | Single line text | Yes | Auto-number | "FLAG-000001" format |
| SubmissionID | Lookup | Yes | List: tbl_VolumeSubmissions, Column: Title | - |
| FlagType | Choice | Yes | - | Type of data quality issue |
| Severity | Choice | Yes | Default: Medium | Issue severity level |
| Description | Multiple lines of text | Yes | Plain text, 4 lines | Human-readable explanation |
| ThresholdValue | Number | No | Decimals: 2 | Expected/threshold value |
| ActualValue | Number | No | Decimals: 2 | Actual value triggering flag |
| FlaggedDate | Date and Time | Yes | Include time, Default: Today | When flagged |
| Status | Choice | Yes | Default: Open | Flag resolution status |
| ResolvedByEmail | Single line text | No | Max length: 255 | Who resolved (text workaround) |
| ResolvedByName | Single line text | No | Max length: 100 | Display name |
| ResolvedDate | Date and Time | No | Include time | When resolved |
| ResolutionNotes | Multiple lines of text | No | Plain text, 4 lines | How/why resolved |
| AutoDismissed | Yes/No | Yes | Default: No | System auto-dismissed flag |

**Lookup Column Settings:**

**SubmissionID:**
```
Get information from: tbl_VolumeSubmissions
In this column: Title
Enforce relationship behavior: Cascade Delete (flags tied to submission)
```

**Choice Column Settings:**

**FlagType:**
```
Choices:
  Anomaly_High
  Anomaly_Low
  Budget_Variance
  Missing_Forecast
  Zero_Volumes
  Late_Submission
  Format_Error
  Duplicate_Submission

Display: Dropdown
```

**Severity:**
```
Choices:
  Low
  Medium
  High
  Critical

Display: Dropdown
Default: Medium
```

**Status:**
```
Choices:
  Open
  Dismissed
  Resolved

Display: Dropdown
Default: Open
```

**Indexed Columns:**
1. SubmissionID - Settings → Indexed → Yes
2. Status - Settings → Indexed → Yes
3. Severity - Settings → Indexed → Yes
4. FlaggedDate - Settings → Indexed → Yes

**Sample Data:**
*(Add after tbl_VolumeSubmissions is created with sample data)*

**Build Checklist:**
- [ ] List created
- [ ] All columns added
- [ ] SubmissionID lookup configured (will complete after VolumeSubmissions exists)
- [ ] All choice values configured
- [ ] Four indexes created
- [ ] Test: Verify Status and Severity dropdowns
- [ ] Note: Sample data to be added after submissions exist

---

### 4.8 tbl_SubmissionHistory

**Purpose:** Complete audit trail of all changes to volume submissions.

**Create List:**
- Template: Custom List
- Name: `tbl_SubmissionHistory`
- Description: "Audit trail for all submission changes"

**Columns to Create:**

| Column Name | Type | Required | Settings | Validation |
|-------------|------|----------|----------|------------|
| Title | Single line text | Yes | Auto-number | "CHG-000001" format |
| SubmissionID | Lookup | Yes | List: tbl_VolumeSubmissions, Column: Title | - |
| ChangeDate | Date and Time | Yes | Include time, Default: Now | When change occurred |
| ChangedByEmail | Single line text | Yes | Max length: 255 | Who changed (text workaround) |
| ChangedByName | Single line text | Yes | Max length: 100 | Display name |
| ChangeType | Choice | Yes | Default: Updated | Type of change |
| PreviousStatus | Single line text | No | Max length: 30 | Status before |
| NewStatus | Single line text | No | Max length: 30 | Status after |
| PreviousActual | Number | No | Decimals: 0 | ActualVolume before |
| NewActual | Number | No | Decimals: 0 | ActualVolume after |
| PreviousBudget | Number | No | Decimals: 0 | BudgetVolume before |
| NewBudget | Number | No | Decimals: 0 | BudgetVolume after |
| PreviousForecast | Number | No | Decimals: 0 | ForecastVolume before |
| NewForecast | Number | No | Decimals: 0 | ForecastVolume after |
| ChangeReason | Multiple lines of text | No | Plain text, 4 lines | Why change was made |
| IPAddress | Single line text | No | Max length: 45 | User IP (optional) |

**Lookup Column Settings:**

**SubmissionID:**
```
Get information from: tbl_VolumeSubmissions
In this column: Title
Enforce relationship behavior: Cascade Delete (history tied to submission)
```

**Choice Column Settings:**

**ChangeType:**
```
Choices:
  Created
  Updated
  Validated
  Rejected
  Deleted
  Restored

Display: Dropdown
Default: Updated
```

**Indexed Columns:**
1. SubmissionID - Settings → Indexed → Yes
2. ChangeDate - Settings → Indexed → Yes
3. ChangedByEmail - Settings → Indexed → Yes
4. ChangeType - Settings → Indexed → Yes

**Sample Data:**
*(Add after tbl_VolumeSubmissions is created with sample data)*

**Build Checklist:**
- [ ] List created
- [ ] All columns added
- [ ] SubmissionID lookup configured (complete after VolumeSubmissions exists)
- [ ] ChangeType choice values configured
- [ ] Four indexes created
- [ ] Note: Sample data to be added after submissions exist

---

### 4.9 tbl_VolumeSubmissions

**Purpose:** Core transactional table storing all volume submissions from all countries. This is the fact table of the system.

**Create List:**
- Template: Custom List
- Name: `tbl_VolumeSubmissions`
- Description: "Core transactional table for enrollment volume submissions"

**⚠️ IMPORTANT:** This is the largest and most complex list. Build carefully and verify each column.

**Columns to Create:**

#### Identification & Lookup Fields

| Column Name | Type | Required | Settings | Validation |
|-------------|------|----------|----------|------------|
| Title | Single line text | Yes | Max length: 100 | Auto-calculated: "CountryCode-TestTypeCode-YYYYMM" |
| CountryID | Lookup | Yes | List: tbl_Countries, Column: Title | - |
| TestTypeID | Lookup | Yes | List: tbl_TestTypes, Column: Title | - |
| SubmissionMonth | Single line text | Yes | Max length: 7 | `=AND(LEN([SubmissionMonth])=7, ISNUMBER(VALUE(LEFT([SubmissionMonth],4))))` |

#### Volume Data Fields

| Column Name | Type | Required | Settings | Validation |
|-------------|------|----------|----------|------------|
| ActualVolume | Number | No | Decimals: 0, Min: 0, Max: 999999 | `=OR(ISBLANK([ActualVolume]), [ActualVolume]>=0)` |
| BudgetVolume | Number | No | Decimals: 0, Min: 0, Max: 999999 | `=OR(ISBLANK([BudgetVolume]), [BudgetVolume]>=0)` |
| ForecastVolume | Number | No | Decimals: 0, Min: 0, Max: 999999 | `=OR(ISBLANK([ForecastVolume]), [ForecastVolume]>=0)` |

#### Submission Metadata Fields

| Column Name | Type | Required | Settings | Validation |
|-------------|------|----------|----------|------------|
| SubmittedByEmail | Single line text | No | Max length: 255 | Email of submitter (text workaround) |
| SubmittedByName | Single line text | No | Max length: 100 | Display name of submitter |
| SubmittedDate | Date and Time | No | Include time | Submission timestamp |
| SubmissionMethod | Choice | Yes | Default: Portal | Portal/Upload/Manual/System |

#### Workflow & Status Fields

| Column Name | Type | Required | Settings | Validation |
|-------------|------|----------|----------|------------|
| Status | Choice | Yes | Default: Draft | Current workflow state |
| ValidatedByEmail | Single line text | No | Max length: 255 | Validator email (text workaround) |
| ValidatedByName | Single line text | No | Max length: 100 | Validator display name |
| ValidatedDate | Date and Time | No | Include time | Validation timestamp |
| ValidationNotes | Multiple lines of text | No | Plain text, 6 lines | Analyst comments |
| DataQualityScore | Number | No | Decimals: 0, Min: 0, Max: 100 | `=OR(ISBLANK([DataQualityScore]), AND([DataQualityScore]>=0, [DataQualityScore]<=100))` |

#### Estimation Fields

| Column Name | Type | Required | Settings | Validation |
|-------------|------|----------|----------|------------|
| IsEstimate | Yes/No | Yes | Default: No | Is this estimated data? |
| EstimateReason | Multiple lines of text | No | Plain text, 4 lines | Why estimated (required if IsEstimate=Yes) |

#### Audit Trail Fields

| Column Name | Type | Required | Settings | Validation |
|-------------|------|----------|----------|------------|
| LastModifiedByEmail | Single line text | Yes | Max length: 255 | Last editor email (text workaround) |
| LastModifiedByName | Single line text | Yes | Max length: 100 | Last editor name |
| LastModifiedDate | Date and Time | Yes | Include time, Default: Now | Last edit timestamp |

#### Soft Delete Fields

| Column Name | Type | Required | Settings | Validation |
|-------------|------|----------|----------|------------|
| IsDeleted | Yes/No | Yes | Default: No | Soft delete flag |
| DeletedByEmail | Single line text | No | Max length: 255 | Who deleted (text workaround) |
| DeletedByName | Single line text | No | Max length: 100 | Deleter display name |
| DeletedDate | Date and Time | No | Include time | Deletion timestamp |

**Lookup Column Settings:**

**CountryID:**
```
Get information from: tbl_Countries
In this column: Title
Add a column to show: CountryCode
Enforce relationship behavior: Restrict Delete
```

**TestTypeID:**
```
Get information from: tbl_TestTypes
In this column: Title
Add a column to show: TestTypeCode
Enforce relationship behavior: Restrict Delete
```

**Choice Column Settings:**

**SubmissionMethod:**
```
Choices:
  Portal
  Upload
  Manual
  System

Display: Dropdown
Default: Portal
```

**Status:**
```
Choices:
  Draft
  Submitted
  Pending_Review
  Validated
  Flagged
  Approved
  Rejected

Display: Dropdown
Default: Draft
```

**Indexed Columns (CRITICAL FOR PERFORMANCE):**
1. SubmissionMonth - Settings → Indexed → Yes
2. CountryID - Settings → Indexed → Yes (automatic for lookups)
3. TestTypeID - Settings → Indexed → Yes (automatic for lookups)
4. Status - Settings → Indexed → Yes
5. IsDeleted - Settings → Indexed → Yes
6. IsEstimate - Settings → Indexed → Yes

**Sample Data to Add:**

| Title | CountryID | TestTypeID | SubmissionMonth | ActualVolume | BudgetVolume | ForecastVolume | SubmittedByEmail | SubmittedByName | SubmittedDate | SubmissionMethod | Status | IsEstimate | LastModifiedByEmail | LastModifiedByName | LastModifiedDate | IsDeleted |
|-------|-----------|------------|-----------------|--------------|--------------|----------------|------------------|-----------------|---------------|------------------|--------|------------|---------------------|--------------------|--------------------|-----------|
| KOR-LEAD101-202511 | Republic of Korea | Leadership Fundamentals | 2025-11 | 450 | 500 | 475 | jiwon.kim@test.com | Ji-Won Kim | 2025-11-03 09:15 | Portal | Approved | No | jiwon.kim@test.com | Ji-Won Kim | 2025-11-03 09:15 | No |
| JPN-DATA101-202511 | Japan | Data Analytics Essentials | 2025-11 | 320 | 350 | 340 | yuki.tanaka@test.com | Yuki Tanaka | 2025-11-03 14:22 | Portal | Validated | No | yuki.tanaka@test.com | Yuki Tanaka | 2025-11-03 14:22 | No |
| FRA-PMCERT-202511 | France | Project Management Cert Prep | 2025-11 | 125 | 150 | 130 | marie.dubois@test.com | Marie Dubois | 2025-11-04 08:45 | Portal | Flagged | No | marie.dubois@test.com | Marie Dubois | 2025-11-04 08:45 | No |

**Build Checklist:**
- [ ] List created
- [ ] ALL 26 columns added (verify count!)
- [ ] Both lookups configured correctly
- [ ] All choice values configured
- [ ] All validation formulas added and tested
- [ ] Six indexes created (SubmissionMonth, CountryID, TestTypeID, Status, IsDeleted, IsEstimate)
- [ ] Sample data added (3 rows minimum)
- [ ] Test: Verify SubmissionMonth format validation (must be YYYY-MM)
- [ ] Test: Verify DataQualityScore range (0-100)
- [ ] Test: Verify volume minimums (>= 0)
- [ ] Test: Verify both lookups populate dropdowns correctly
- [ ] Test: Verify Status workflow dropdown has all 7 values

---

## 5. Build Order & Validation Strategy

### Recommended Build Sequence

**Phase 1: Foundation (No Dependencies)**
1. tbl_Regions (5 min)
2. tbl_SystemConfig (10 min)

**Phase 2: Master Data (Simple Lookups)**
3. tbl_TestTypes (15 min)
4. tbl_Countries (15 min) - Depends on: Regions

**Phase 3: Relationships (Junction Tables)**
5. tbl_CountryTestTypes (10 min) - Depends on: Countries, TestTypes
6. tbl_UserCountries (10 min) - Depends on: Countries

**Phase 4: Core Transaction**
7. tbl_VolumeSubmissions (30 min) - Depends on: Countries, TestTypes

**Phase 5: Supporting Tables**
8. tbl_DataQualityFlags (15 min) - Depends on: VolumeSubmissions
9. tbl_SubmissionHistory (15 min) - Depends on: VolumeSubmissions

**Total Estimated Time: 2-2.5 hours**

### Validation After Each List

After creating each list:
1. ✅ Verify column count matches spec
2. ✅ Test validation formulas (try to break them)
3. ✅ Verify indexes are created
4. ✅ Add sample data
5. ✅ Test lookups populate correctly
6. ✅ Check choice dropdown values

### Testing Relationships

After all lists are built:
1. ✅ Countries → Regions lookup works
2. ✅ CountryTestTypes → Both Countries and TestTypes
3. ✅ UserCountries → Countries
4. ✅ VolumeSubmissions → Countries and TestTypes
5. ✅ DataQualityFlags → VolumeSubmissions
6. ✅ SubmissionHistory → VolumeSubmissions

---

## 6. Implementation Notes

### Text Field Workaround - User Columns

**Development Pattern:**
All "Person" columns implemented as two text fields:
- {Field}Email (Single line text, 255 chars)
- {Field}Name (Single line text, 100 chars)

**Affected Lists:**
- tbl_UserCountries (UserEmail, UserName)
- tbl_VolumeSubmissions (SubmittedByEmail/Name, ValidatedByEmail/Name, LastModifiedByEmail/Name, DeletedByEmail/Name)
- tbl_SubmissionHistory (ChangedByEmail/Name)
- tbl_DataQualityFlags (ResolvedByEmail/Name)

**Power Apps Implementation:**
User selection dropdown will populate these fields:
```javascript
Set(varCurrentUser, 
  Switch(
    drpUserSelect.Selected.Value,
    "Ji-Won Kim", {Email: "jiwon.kim@test.com", Name: "Ji-Won Kim"},
    "Yuki Tanaka", {Email: "yuki.tanaka@test.com", Name: "Yuki Tanaka"},
    "Alice Chen", {Email: "alice.chen@test.com", Name: "Alice Chen"},
    "John Rolzhausen", {Email: User().Email, Name: User().FullName}
  )
);
```

### Calculated Title Fields

Several lists use calculated Title fields for better readability:
- tbl_CountryTestTypes: "CountryCode-TestTypeCode"
- tbl_UserCountries: "UserName - CountryName"
- tbl_VolumeSubmissions: "CountryCode-TestTypeCode-YYYYMM"

**Implementation:** 
For MVP, manually create these when adding records. In Power Apps, auto-calculate using Concatenate() function.

### Performance Considerations

**Lists Expected to Grow Large:**
- tbl_VolumeSubmissions: 5,000-50,000 rows
- tbl_SubmissionHistory: 20,000-200,000 rows

**Critical Indexes Applied:**
Both lists have 4-6 indexed columns for query performance. SharePoint's 5,000 item view threshold is managed by:
- Indexed columns on all filter fields
- Power Apps delegation-friendly queries
- Power BI DirectQuery optimization

---
### Email Configuration for Development and Production

#### Test Email Accounts

**Security Consideration:**
This repository is public on GitHub. To protect personal and business email addresses from web scraping and spam bots, dedicated test Gmail accounts were created specifically for this portfolio project.

**Test Accounts Created:**
- jiwon.kim.demo@gmail.com (Korea Coordinator - Primary)
- yuki.tanaka.demo@gmail.com (Japan Coordinator - Backup)
- alice.chen.demo@gmail.com (Data Quality Analyst)
- marie.dubois.demo@gmail.com (France Coordinator - Primary)

**Benefits of This Approach:**
1. **Security:** Protects personal/business email addresses in public repository
2. **Realistic Testing:** Separate inboxes enable true multi-user workflow simulation
3. **Verification:** Can confirm notifications reach correct recipients by role
4. **Demonstration:** Shows understanding of proper test environment setup
5. **Privacy:** Project-specific accounts can be deactivated after portfolio completion

**Cross-Tenant Email Capability:**
Power Automate flows send emails FROM the tenant account (John@Rolzhausen.com) TO external Gmail addresses without restrictions. This demonstrates:
- Cross-tenant email capability (common in B2B scenarios)
- Proper separation of dev/test environments
- Understanding of external communication requirements

---

#### Shared Mailbox Strategy (Production Best Practice)

**Development Environment:**
All Power Automate notification flows send emails FROM: John@Rolzhausen.com (developer's tenant account)

**Production Implementation:**
In a production environment, automated system notifications would be sent from a dedicated shared mailbox rather than a personal account.

**Recommended Production Configuration:**
```
Shared Mailbox: EnrollmentNotifications@company.com
Purpose: System-generated workflow notifications
Access: Data Quality team members, administrators
Power Automate Action: "Send an email from a shared mailbox (V2)"
```

**Benefits of Shared Mailbox Approach:**

1. **Professional Appearance**
   - System identity vs personal communication
   - Builds user trust in automated notifications
   - Clear separation between system and individual emails

2. **Business Continuity**
   - Independent of individual employee accounts
   - Notifications persist through personnel changes
   - No disruption when team members transition roles

3. **Team Collaboration**
   - Multiple team members can monitor sent items
   - Shared visibility into all system communications
   - Collaborative response to user inquiries

4. **Audit Trail & Compliance**
   - Complete history of system communications
   - Regulatory compliance for notification tracking
   - Searchable archive of all automated emails

5. **Permission Management**
   - Granular control over mailbox access
   - Easy to add/remove team members
   - Separate from individual user permissions

**Microsoft 365 Licensing:**
Shared mailboxes are included in Microsoft 365 Business Standard at no additional cost (up to 50GB storage per shared mailbox). They can be created through the Microsoft 365 Admin Center under Teams & groups → Shared mailboxes.

**Technical Implementation:**

In Power Automate, the action configuration would be:
```
Action: Send an email from a shared mailbox (V2)
Shared Mailbox Address: EnrollmentNotifications@company.com
To: @{outputs('Get_UserEmail')?['body/UserEmail']}
Subject: Enrollment Submission Status Update
Body: [HTML email template]
Importance: Normal
```

**Why Not Implemented in Development:**
- Single-user tenant limits demonstration of multi-user shared mailbox access
- Development emails sent from personal account are sufficient for workflow testing
- Documentation of production approach demonstrates enterprise architecture knowledge
- Time invested in setup vs value gained in solo development environment

**Production Migration Path:**
1. Create shared mailbox in production Microsoft 365 tenant
2. Grant Send As permissions to service account running Power Automate flows
3. Update all notification flows to use "Send from shared mailbox" action
4. Configure mailbox permissions for team members requiring access
5. Set up retention policies per organizational compliance requirements

---

#### Email Sample Data Standards

All user email fields in the SharePoint lists use the test Gmail accounts to enable verification of notification flows during development.

**Affected Lists:**
- tbl_UserCountries: UserEmail field
- tbl_VolumeSubmissions: SubmittedByEmail, ValidatedByEmail, LastModifiedByEmail, DeletedByEmail
- tbl_SubmissionHistory: ChangedByEmail  
- tbl_DataQualityFlags: ResolvedByEmail

**Sample Email Addresses:**
- Primary Coordinators: jiwon.kim.demo@gmail.com, marie.dubois.demo@gmail.com
- Backup Coordinator: yuki.tanaka.demo@gmail.com
- Analyst: alice.chen.demo@gmail.com

**Production Replacement:**
In production deployment, these text fields would be replaced with Person columns that automatically populate email addresses from Azure AD user profiles, eliminating manual entry and ensuring data accuracy.
## 7. Post-Build Validation Checklist

After all 9 lists are created:

### Schema Validation
- [ ] All 9 lists created and named correctly
- [ ] Column counts match specifications
- [ ] All data types correct
- [ ] All required fields marked as required
- [ ] All validation formulas working
- [ ] All choice values configured
- [ ] All indexes created

### Relationship Validation
- [ ] All lookup columns point to correct lists
- [ ] Lookup dropdowns populate with values
- [ ] Relationship behaviors set correctly (Restrict vs Cascade)
- [ ] Foreign key integrity maintained

### Data Validation
- [ ] Sample data added to all lists
- [ ] Validation rules prevent bad data
- [ ] Unique constraints enforced
- [ ] Date logic validated (RetiredDate >= EffectiveDate, etc.)

### Performance Validation
- [ ] Critical indexes verified on large tables
- [ ] Query performance acceptable with sample data
- [ ] No unnecessary columns indexed

---

## 8. Next Steps

After SharePoint lists are built:

1. **Update Documentation (15 min)**
   - Mark lists as "Built" in this document
   - Update README.md progress
   - Add screenshots to GitHub
   - Commit: "Day 4: SharePoint lists created"

2. **Begin Power Apps Design (Day 5)**
   - Connect to SharePoint data sources
   - Design Submission Portal
   - Test data entry workflows

3. **Power Automate Flows (Day 5-6)**
   - Submission notifications
   - Data quality validation
   - Reminder workflows

4. **Power BI Dashboard (Week 2)**
   - Connect to SharePoint lists
   - Build data model
   - Create visualizations

---

## Document History

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | 2025-11-22 | John Rolzhausen | Initial SharePoint schema specifications for all 9 MVP lists |

---

**Status:** Ready for Implementation  
**Build Start:** November 22, 2025  
**Estimated Completion:** 2-3 hours

---

*This document serves as the authoritative specification for SharePoint list creation. All columns, validation rules, and indexes should be implemented exactly as specified.*
