# Pain Points and Lessons Learned

## The Original System: SQL Dashboard (2016-2024)

For 8 years, I maintained an Excel-based dashboard that pulled enrollment data 
from our SQL Server database for three major test programs. It updated nightly 
via automated queries and presented volumes broken down by country, region, test type, 
and delivery method.  Volumes were classified as Registrations, Cancellations, Reschedules, No-shows,
and Net Volumes

**What made it successful:**
- Single source of truth (SQL database)
- Consistent data schema across all tests
- Automated refresh = zero manual work
- Used daily by CFO and finance team
- Enabled quick decision-making on capacity planning

## When Everything Changed: The Expansion (2023)

After a reorganization in the company requested enrollment data for additional 
training programs was made, not just the three in the database. Problem: those 
programs weren't centralized and volumes were collected manually via Excel spreadsheets
attached to manual emails.

**New Requirements:**
- Manual report that took up to 40 hours to compile needed to be automated.
- Manual entry to volume reporting tool from the report needed to be automated automated  

**Challenges:**
- Various excel sheets in different formats needed to be put into a centralized format.
- Some of the excel sheets contained password protection which needed to be removed from
  the attachments. Separate emails were sent containing the passwords. Removal needed to 
  be done manually.
- Countries were named differently across the different tests.
- Small changes in worksheet structure would break downstream processes which needed to 
  be fixed manually.
- Original architecture was via SQL Server Databases. Additional tests were not entered into
  the database.

**Solution proposed**
- Powerapps solution to enter volumes at least weekly to replace monthly reporting sent through
  email
- Powerautomate to download email attachment when received into a central Sharepoint
  repository.
- Salesforce reports containing many countries was downloaded on a nightly basis and sent to the
  Sharepoint repository.
- Power BI dataset collected data from the files in repository and combined into a single structured
  list
- Excel data source to the BI dataset created for reporting
- .csv file created to upload into volume reporting tool.

**Failures in the proposed solution**
- Powerapps solution for volume entry was rejected by management to prevent the need to change
  existing process for end users.
- Minor changes to formating of excel files would result in automated Power BI refreshes
- Password protection removal could not be handled by Power automate and needed to be done manually.
- Late submissions of excel sheets would result in the necessity to repeat manual fixes
- Manual updates would be needed frequently resulting in up to 3 hours of additional work.

**Redesign Principals**
- Power apps solution to enter volumes daily should be reinstated.
- Eliminate reporting via email attachments
- Standardized reporting of existing volumes entry should be created and emailed to enterers.
- Daily reporting that existed in the SQL Server volumes report should be enforced.
- Reminder email sent to Data entry clerks if missed entry for more than two days.
- Automated reports sent to stakeholders on a weekly basis.  Final report available by third
  business day.
- .csv template created to upload into volumes reporting tool or into SQL server database




