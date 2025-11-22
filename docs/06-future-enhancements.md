# Future Enhancements & Ideas

## Phase 1 Enhancements
- [ ] Create automated Excel upload to Power Apps to provide options for Manual Entry of Spreadsheet Upload
- [ ] Spreadsheet upload would require creation of a Standardized Excel sheet.

## Phase 2 Enhancements (After MVP)
- [ ] Multi-language support for submission portal
- [ ] Mobile app for on-the-go submissions
- [ ] Predictive analytics for volume forecasting

### Forecast Versioning (Quarterly Snapshots)
**Business Value:** Enable CFO-level variance analysis comparing actual performance to multiple forecast versions (Original, Q2, Q3, Q4 forecasts).

**Current State (MVP):**
- Single forecast column representing next month prediction
- Basic variance analysis: Actual vs Budget, Actual vs Prior Forecast

**Enhanced Design:**
- Quarterly forecast versions: October (original), January (Q2), April (Q3), July (Q4)
- Separate Forecast_Versions table or additional columns
- Power BI dashboard showing:
  - Forecast accuracy by version
  - Forecast drift analysis (how forecasts changed Q-over-Q)
  - Best/worst forecasters by country

**Implementation Approach:**
- Option A: Add columns (Oct_Forecast, Jan_Forecast, Apr_Forecast, Jul_Forecast)
- Option B: Create Forecast_Versions + Forecast_Detail tables (normalized)
- Recommendation: Option B for scalability

**Inspired by:** Financial planning practices from PeopleSoft/TM1 implementations where quarterly forecast revisions enabled accurate variance analysis and improved forecasting discipline.

**Estimated Effort:** 1 week (data model changes, Power Apps UI updates, Power BI DAX measures)

## "Someday/Maybe" Ideas
- [ ] Integration with additional forecasting tools
- [ ] Automated creation of Excel reports containing data currently created manually but in a centralized structure
      
## Future Enhancement: Test Center Level Detail

**Description:** Allow submissions at test center level within countries, 
with subtest breakdowns for courses with multiple components.

**Business Value:**
- More granular regional analysis
- Identify high/low performing centers
- Track subtest popularity

**Implementation Complexity:**
- Additional dimension table (TestCenters)
- Junction table (Country_TestCenter)
- Modified submission UX (2-step: center selection, then volumes)
- Aggregation logic in Power BI
- Increased data entry time per submission

**Decision:** Deferred to Phase 2 pending stakeholder demand

**Estimated Effort:** 2 weeks development

## Ideas Considered & Rejected (with reasons)
- 
