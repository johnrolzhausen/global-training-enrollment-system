Technical Architecture

DRAFT - Executive Summary
The Global Training Enrollment System is an integrated Power Platform solution that transforms enrollment data collection from a fragmented, manual email-based process into a standardized, automated workflow. The system serves 45+ training centers across multiple countries, enabling real-time data collection, systematic validation, and interactive executive reporting.
Business Problem Solved:
Global training organizations with distributed centers face critical data consolidation challenges: inconsistent Excel submissions via email, manual data cleanup requiring 15-20 hours monthly, delayed reporting with multi-week lags, and no visibility into submission status or data quality. This solution eliminates manual processing, provides real-time executive visibility, and ensures data quality through systematic validation workflows.
Technology Stack:

Data Layer: SharePoint Online (9 lists with normalized schema)
Application Layer: Power Apps (3 canvas apps for submission, validation, administration)
Integration Layer: Power Automate (automated workflows for notifications, validation, email processing)
Analytics Layer: Power BI (interactive dashboards with DirectQuery)
Security: Azure Active Directory (authentication, role-based access control)

User Base:

45 training center coordinators (data submission)
3 data quality analysts (validation and correction)
2 system administrators (configuration and user management)
5+ executives and finance team (analytics consumption)
