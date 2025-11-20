# Pain Points and Lessons Learned

## The Original System: SQL Dashboard (2016-2024)

For 8 years, I maintained an Excel-based dashboard that pulled enrollment data 
from our SQL Server database for three major test programs. It updated nightly 
via automated queries and presented volumes broken down by country, region, test type, 
and delivery method. Actual Volumes were classified as Registrations, Cancellations, Reschedules, No-shows,
and Net Volumes. Budget and Forecast volumes were also calculated but only for Net Volumes 

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
- Manual entry to volume reporting tool from the report needed to be automated  

**Challenges:**
- Various excel sheets in different formats needed to be put into a centralized format.
- Some of the excel sheets contained password protection which needed to be removed from
  the attachments. Separate emails were sent containing the passwords. These were sent by 
  the same individual who provided the data but up to an hour later. Removal of passwords
  needed to be done manually.
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
- Manual updates would be needed on the third day of each month resulting in up to 3 hours of additional work
  on the day that files were due.

**Redesign Principals**
- Power apps solution to enter volumes daily should be reinstated.
- Eliminate reporting via email attachments
- Standardized reporting of existing volumes entry should be created and emailed to enterers.
- Daily reporting that existed in the SQL Server volumes report should be enforced.
- Reminder email sent to Data entry clerks if missed entry for more than two days.
- Automated reports sent to stakeholders on a weekly basis.  Final report available by third
  business day.
- .csv template created to upload into volumes reporting tool or into SQL server database

**Additional Context**
Volumes Types
- Test centers did not submit volumes for all 5 metrics separately.  Volumes were issued only for Net Volumes.
- Actual Volumes were submitted on a monthly basis by Country and Test Type
- Budget Volumes were updated on an annual basis by Country and Test Type
- Forecast Volumes were updated on a monthly basis by Country and Test type.
- Actual, Budget and Forecast volumes were submitted on the same excel file each month but updated based on above.
- None of the metrics were more prone to errors in the original Excel based volumes dashboard.
- Separate metrics were not used in the new test requirements so they were not more prone to errors.

Test Centers
- Test centers were internal company offices and all test centers within a country were submitted in total to
  the corporate offices in the USA by a representative of that country. Exact number of testing centers was
  therefore unknown to employees in the USA.
- There were few countries that consistently reported their volumes late. (Peru, Brazil,Vietnam)
 
Submitted volumes datasources - how they were received
- Most countries would submit their volumes to a representative in the US corporate offices who would manually
  import them near the end of the month into Salesforce.
- Volumes for European countries were submitted by the international office in France. They were submitted on an Excel file
  which contained volumes for all European countries and grouped by region.
- Volumes for Japan were submitted by month on an Excel file attached to email.  The file was password protected, but the password 
  only changed once a year. The bigger issue was that the file would periodically come in corrupted and the data had to be
  copied into a new file to be pulled into Power BI.
- Volumes for Korea were submitted by month on an Excel file attached to email.  The file was password protected.  The password came on 
  a separate file about an hour after the excel file was received and was changed each month.
- Volumes for Israel came in on a simple one line excel file with one line for Actual, Forecast and Budget by Month

Salesforce Component
- The volumes in Salesforce would come in from countries that reported volumes weekly and were entered by an analyst of
  the test areas being reported.
- Salesforce volumes were entered throughout the month, but the .csv report was scheduled nightly in an attempt to import them to
  Power BI as they came in for potential mid month reporting.
- Salesforce data was the most clean and reliable. The format never changed and the import into Power BI never failed.
- Power queries for other sources were designed to replicate the format of Salesforce.
- We were unable to pull data from the API because of license restrictions. 
- Since I do not have a Salesforce account, I won't be able to pull data from it's API for the redesign. Since it is manually
  updated in Salesforce, the same effort would be required to enter into a power apps solution.  I believe it would be best to 
  eliminate the Salesforce Component completely.

Cognos TM1 Volume Reporting Tool
- The company used TM1 for their Budgets and Forecasts. My department did have access to update budgets annually and forecasts on
  quarterly basis.
- Prior to the new Power BI solution we developed, Test volumes were reported in TM1 by Test Type by month, countries were not
  included and were updated monthly.  By using the .csv we were able to quickly upload the file and include Country as well as Test
  Type.
- We cannot eliminate the TM1 Reporting tool as it is the tool used for all Budgets and Forecasts, not just volumes.

Stakeholders usage
- The old excel based volume report was used on a daily basis by the CFO to analyze trends and question analysts on the reasons
  for drops in revenue. He may not have needed it that often, but he wanted it and we were able to provide it.
- The additional volumes could only be provided on a monthly basis when all volumes were received. The new CFO was able to use them
  for his "Big Sheets" (New P&L reports that contained additional metrics such as volumes and headcount that could not be pulled from
  our PeopleSoft accounting software). These were produced after the month end close so there was no push to get them faster.
- The volumes for the additional countries could potentially be provided more often if daily input to a power app were enforced.
  
  
  
    




