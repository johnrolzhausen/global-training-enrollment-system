---

## Week 3 Day 2: Executive Summary Page - COMPLETE ✅

**Date:** December 2, 2025  
**Duration:** 6 hours  
**Status:** First dashboard page complete with 4 KPIs and supporting visuals

### Objectives
- Build first dashboard page with executive-level KPIs
- Implement time intelligence for year-over-year comparisons
- Create supporting visuals (trend chart, top countries, performance gauge)
- Establish design standards for remaining pages

---

### Deliverables Completed

**1. KPI Cards (4 Total)**
- ✅ Total Actual Volumes: 45K (2025 YTD)
- ✅ Total Budget Volumes: 45K 
- ✅ Year Over Year Growth: 2.2% (displayed in green)
- ✅ Approved Submissions: 66 (11 months × 6 countries)

**2. Supporting Visualizations**
- ✅ Line chart: 11-month trend showing seasonal patterns (Jan-Nov 2025)
- ✅ Top 5 Countries bar chart: Horizontal bars with data labels (Korea 17K, France 9K, USA 7K, Japan 5K, Germany 3K)
- ✅ Performance vs Budget gauge: Visual indicator showing -0.3% variance with color zones

**3. Filter Controls**
- ✅ Year slicer: Tile style, compact, positioned top-center
- ✅ Proper filtering across all visuals

**4. Data Foundation**
- ✅ 138 records of realistic test data loaded via Power Automate
- ✅ Custom CSV parser (no premium connectors required)
- ✅ Lookup field translation for Country and TestType

---

### Key Learnings

**Sort By Column Required for Chronological Display**
- DateTable[Month] column showed alphabetical sorting (Apr, Aug, Dec, Feb...)
- Solution: Created DateTable[MonthSortOrder] (1-12) and set Month to sort by it
- Result: Proper Jan, Feb, Mar... Dec ordering in all visuals
- Lesson: Never rely on text fields for date sequencing

**Time Intelligence Measures Work Across Filter Boundaries**
- YoY Growth % calculates correctly even when year slicer filters to single year
- CALCULATE with SAMEPERIODLASTYEAR reaches across filter context
- This enables comparisons even with aggressive filtering
- Critical for executive dashboards where users toggle between years

**Slicer Orientation Controlled by Dimensions, Not Settings**
- Expected: Slicer settings → Orientation → Horizontal/Vertical
- Reality: Power BI determines orientation based on visual width/height ratio
- Narrow and tall = vertical, wide and short = horizontal
- Solution: Resize visual container to force desired orientation

**The +0 Trick Converts BLANK to 0**
- Problem: Time intelligence measures returned BLANK when no prior year data exists
- Visual impact: Card shows "(Blank)" instead of meaningful value
- Solution: Add +0 at end of measure formula
- Result: BLANK converts to 0, card displays "0%" or "0" cleanly

**Power BI Auto-Display Units Threshold**
- Display units (K, M, etc.) only auto-apply at 10,000+
- Values < 10K show as whole numbers (7,475 not 7.5K)
- Can manually set display units in formatting pane
- Consistency across visuals requires manual configuration

---

### Metrics Achieved

**Performance Summary (2025 YTD):**
- Total Actual Volumes: 44,865
- Total Budget Volumes: 44,990
- Overall Variance: -0.3% (125 volumes under budget)
- Year-over-Year Growth: +2.2% (positive trend)

**Data Coverage:**
- Time Period: 11 months (January - November 2025)
- Countries: 6 (Korea, Japan, France, Germany, Italy, United States)
- Test Types: 6 courses
- Approved Submissions: 66 records

**Visual Elements:**
- KPI Cards: 4
- Charts: 2 (line chart, bar chart)
- Gauges: 1
- Filter Controls: 1 (year slicer)

---

### Technical Highlights

**DAX Measures Implemented (17 Total):**
- Volume measures: Total Actual Volumes, Total Budget Volumes, Total Forecast Volumes
- Variance measures: Variance Amount, Budget Attainment %
- Growth measures: YoY Growth %, YoY Growth Amount
- Time intelligence: Previous Year calculations using SAMEPERIODLASTYEAR
- Supporting measures: Approved submission count, rankings

**Data Model:**
- DateTable with daily granularity supporting monthly aggregations
- Proper relationships: DateTable[Date] → VolumeSubmissions[SubmissionDate]
- Sort by column configured for proper chronological display
- Fiscal year support (October start) built into DateTable

**Power Automate Integration:**
- Custom CSV parser handles data loading
- No premium connectors required (within M365 Business Standard)
- Lookup field translation performed during load
- 138 records loaded with proper Country and TestType references

---

### Files Created
- `power-bi/screenshots/01-executive-summary-final.png` - Final page screenshot
- `power-bi/GTE_Dashboard_v1.pbix` - Power BI file with complete Executive Summary

---

## Week 3 Day 3: Geographic Analysis Page - COMPLETE ✅

**Date:** December 2, 2025  
**Duration:** 5 hours  
**Status:** Second dashboard page complete with maps, matrices, and regional analysis

### Objectives
- Add geographic dimension to dashboard
- Implement Azure Maps for visual country representation
- Create hierarchical drill-down from region to country
- Add regional KPI cards and comparison charts

---

### Deliverables Completed

**1. Azure Maps Visualization**
- ✅ Bubble map showing all 6 countries with proportional sizing
- ✅ Bubble size reflects enrollment volumes (Korea largest, Germany smallest)
- ✅ Clean map view without clutter
- ✅ Interactive hover showing country details

**2. Hierarchical Matrix**
- ✅ Region → Country drill-down capability
- ✅ Three data columns: Actual Volumes, Budget Volumes, Variance to Budget %
- ✅ Conditional formatting on Variance column (green positive, red negative)
- ✅ Total row showing global aggregations
- ✅ Expand/collapse functionality for regions

**3. Regional KPI Cards**
- ✅ Asia Pacific card: 22.1K volumes
- ✅ Western Europe card: 15.3K volumes
- ✅ North America card: 7.5K volumes
- ✅ Consistent styling and sizing
- ✅ Text boxes for labels (workaround for recent Power BI changes)

**4. Regional Comparison Chart**
- ✅ Clustered column chart: Actual (blue) vs Budget (orange) by region
- ✅ Data labels on bars for readability
- ✅ Inner padding creating visual separation
- ✅ Legend renamed for clarity ("Actual" instead of "Total Actual Volumes")

**5. Year Slicer Integration**
- ✅ Positioned top-center for consistency with Executive Summary
- ✅ Filters all visuals on page correctly

---

### Key Learnings

**Azure Maps Setup Process Varies from Documentation**
- Microsoft docs showed different setup flow than current Power BI version
- Expected: Category and Size fields in specific locations
- Reality: Had to add fields through Build Visual pane, wait for map to populate
- Setup process varied but delivered working solution
- Demonstrates adaptability to platform changes

**Power BI Display Units Only Trigger at 10,000+**
- Values < 10K show as whole numbers (7,475 not 7.5K)
- Must manually configure display units if consistency needed
- Cards use auto-display units based on value magnitude
- Matrix and charts follow same threshold rules

**Recent Power BI Removed Direct Label Editing for Cards**
- Previous versions allowed editing category label text directly
- Current version: Category label is fixed to measure name
- Workaround: Toggle category label OFF, add separate text box below card
- Text box positioned manually for label-like appearance
- More control but requires extra step

**Inner Padding Critical for Side-by-Side Bars**
- Default: No gap between Actual and Budget bars in same category
- Problem: When values very close (22.1K vs 22.1K), bars appear merged
- Solution: Inner padding setting (25-30%) creates visible gap
- Improves readability without requiring data labels everywhere
- Standard practice for clustered column charts

**"Rename for This Visual" Essential for Clean Legends**
- Problem: Legend shows full measure name "Total Actual Volumes"
- Too long, crowds legend area
- Solution: Right-click field in Y-axis well → Rename for this visual
- Type "Actual" - only affects current visual, not measure globally
- Essential for professional appearance

---

### Metrics Summary

**Data Coverage:**
- Regions: 3 (Asia Pacific, Western Europe, North America)
- Countries: 6 (Korea, Japan, France, Germany, Italy, United States)
- Test Types: 6 courses
- Time Period: 24 months (Jan 2024 - Nov 2025)
- Total Records: 138 volume submissions

**Performance Metrics (2025 YTD):**
- Total Actual Volumes: 44,865
- Total Budget Volumes: 44,990
- Overall Variance: -0.3% (125 volumes under budget)
- Regional Variance Range: -0.7% (Western Europe) to -0.0% (Asia Pacific)
- Country Variance Range: -2.0% (Italy) to +0.2% (Korea - only country beating budget)

**Geographic Distribution:**
- Asia Pacific: 22,100 volumes (49.3% of total)
- Western Europe: 15,290 volumes (34.1% of total)
- North America: 7,475 volumes (16.7% of total)

**Visual Elements:**
- Interactive visualizations: 6 (map, matrix, 3 cards, chart)
- Filter controls: 1 (year slicer)
- Drill-down levels: 2 (region → country)
- Conditional formatting rules: 2 (positive/negative variance)
- Cross-filtering connections: Automatic across all visuals

---

### Business Insights

**Key Questions Answered:**
- Where are enrollments concentrated globally? → Asia Pacific dominates (49%)
- How do regions and countries compare? → Hierarchical matrix shows full breakdown
- Are we meeting budget targets by region? → Western Europe struggling most (-0.7%)
- Which countries need attention? → Italy significantly under budget (-2.0%)
- Which countries are performing well? → Korea only country beating budget (+0.2%)

**Geographic Concentration:**
- Top 2 regions (Asia Pacific + Western Europe) represent 83% of business
- North America underrepresented (only 17% despite large market)
- Potential growth opportunity in North American expansion

**Regional Performance:**
- All 3 regions slightly under budget (no region significantly overperforming)
- Variance range narrow (-0.7% to -0.0%) suggests consistent execution
- Korea's positive variance (+0.2%) only bright spot globally

---

### Files Created
- `power-bi/screenshots/02-geographic-analysis-final.png` - Final page screenshot
- `power-bi/GTE_Dashboard_v2.pbix` - Power BI file with 2 complete pages

---

## Week 3 Day 4: Trend Analysis & Test Type Analysis Pages - COMPLETE ✅

**Date:** December 3, 2025  
**Duration:** 6 hours  
**Status:** Pages 3 and 4 complete with trend analysis and product portfolio insights

### Objectives
- Build Trend Analysis page showing 24-month performance trajectory
- Build Test Type Analysis page showing product portfolio health
- Implement conditional formatting for instant insight recognition
- Configure edit interactions for context-appropriate filtering

---

### Deliverables Completed

**Page 3: Trend Analysis**

**1. Enrollment Trends: 24-Month View (Hero Chart)**
- ✅ Combo chart: Actual volumes (blue bars) + Budget (orange line)
- ✅ Full 24-month time series (Jan 2024 - Nov 2025)
- ✅ Single Y-axis for clean comparison
- ✅ Data labels on bars
- ✅ Clear seasonal patterns visible (summer dips both years)

**2. Performance vs Budget: Variance Trend**
- ✅ Column chart showing monthly variance (Actual - Budget)
- ✅ Conditional formatting: Green bars (positive), Red bars (negative)
- ✅ Zero baseline clearly visible
- ✅ 24-month view showing improvement trajectory (more green in 2025)

**3. YoY Growth KPI Card**
- ✅ Large, prominent 2.2% display in green
- ✅ Clear "YOY Growth" label
- ✅ Positioned alongside variance chart

**4. Edit Interactions Configured**
- ✅ Year slicer disabled for both trend charts (always show 24 months)
- ✅ Year slicer active for YoY Growth card (filters correctly)
- ✅ Maintains full context regardless of filter selection

**Page 4: Test Type Analysis**

**1. Test Type Performance: Actual vs Budget**
- ✅ Clustered column chart: Blue (Actual) + Orange (Budget) side-by-side
- ✅ All 6 test types displayed, sorted by volume descending
- ✅ Data labels on bars
- ✅ Inner padding creating visible gap
- ✅ Leadership Fundamentals dominates (17.1K)

**2. Test Type Market Share**
- ✅ Donut chart showing percentage distribution
- ✅ Leadership Fundamentals: 38.09% (largest slice)
- ✅ All 6 tests clearly labeled with percentages
- ✅ Distinct colors per test
- ✅ Legend identifies all products

**3. Test Type Trends: 24-Month View**
- ✅ Multi-line chart (6 lines, one per test)
- ✅ Full 24-month view (Jan 2024 - Nov 2025)
- ✅ Leadership line clearly highest (product dominance)
- ✅ Seasonality visible across all tests
- ✅ Markers on all lines for readability

**4. Performance Summary Matrix**
- ✅ All 6 test types as rows
- ✅ Four columns: Actual, Budget, Variance, YOY %
- ✅ Conditional formatting on Variance (green/red backgrounds)
- ✅ Conditional formatting on YOY % (all green - all tests growing)
- ✅ Total row with aggregations

---

### Key Learnings

**Design Iteration Based on Usability Testing**
- Started with three-line chart (Actual, Budget, Forecast)
- Removed Forecast (tracked Actual too closely, added visual clutter)
- Experimented with dual-bar format (Actual + Budget as side-by-side bars)
- User testing revealed: "Can I quickly answer 'Did we beat budget in July?'"
- Simplified to bar + line format for instant visual comparison
- Principle established: "More information in same space is useless if it can't be translated immediately"

**Information Density vs Comprehension**
- More data ≠ better dashboard
- Executive dashboard prioritizes speed of insight over data density
- Test: Can key question be answered in <5 seconds?
- If no, simplify the visual

**Edit Interactions Feature Critical for Trend Pages**
- Problem: Year slicer filtering trend charts defeats their purpose
- Solution: Edit Interactions → Select slicer → Click "None" on trend visuals
- Result: Trend charts always show full 24 months regardless of slicer
- Other pages: Year slicer filters normally for focused comparisons
- Lesson: Not all pages need same filter behavior

**Combo Chart Y-Axis Alignment Issues**
- Attempted dual Y-axis (bars left, line right)
- Power BI struggled with scale synchronization
- Budget line sometimes compressed or stretched oddly
- Single Y-axis works better when values in same range
- Dual Y-axis only appropriate for different magnitude metrics

**Power BI Card Visual Evolution**
- Recent versions improved card formatting options
- Can now control font size, color, alignment more granularly
- Category label still fixed to measure name (workaround: text box)
- Cards automatically scale font based on container size

**Conditional Formatting Strategy**
- Light backgrounds (#C6EFCE green, #FFC7CE red) better than dark
- Dark backgrounds overwhelm visual, hard to read numbers
- Light backgrounds provide instant color-coding without sacrificing readability
- Consistent color scheme critical: green = good, red = bad across ALL visuals

---

### Metrics Summary

**Trend Analysis Insights:**
- 24-month period: January 2024 - November 2025
- Seasonal pattern: Consistent summer dip (June-July) both years
- Performance trajectory: 2024 mostly under budget (red), 2025 more on/over budget (green)
- YoY Growth: +2.2% (positive trend)
- Improvement visible: More green bars in 2025 variance chart

**Test Type Portfolio Health:**
- Total test types: 6 active courses
- Market leader: Leadership Fundamentals (38.09% market share, 17,090 volumes)
- Portfolio concentration: Top 2 tests = 59% of business
- Growth rate: All tests growing 2.0%-2.5% YoY (no declining products)
- Budget performance: 5 of 6 slightly under budget, Leadership +40 volumes over

**Product Performance Rankings:**
1. Leadership Fundamentals: 17,090 actual (38% share) - OVER budget (+40)
2. Project Management Cert Prep: 9,345 actual (21% share) - Under budget (-5)
3. Executive Leadership: 7,475 actual (17% share) - Under budget (-5)
4. Data Analytics Essentials: 5,010 actual (11% share) - Under budget (-50)
5. Technical Skills Workshop: 3,465 actual (8% share) - Under budget (-55)
6. Compliance Training: 2,480 actual (6% share) - Under budget (-50)

**Strategic Insights:**
- Leadership portfolio anchor: 38% share, only product beating budget
- Portfolio balance: Reasonable distribution, not over-dependent on single product
- Growth trajectory: Entire portfolio growing (no sunset decisions needed)
- Seasonality universal: All tests show summer dip (predictable business cycle)

---

### Business Value

**Executive Summary Improvement:**
- Replaced "Approved Submissions" KPI with "Budget Attainment %" (99.7%)
- Rationale: Executives care about outcomes (budget performance) not process (submission counts)
- Approved Submissions is operational metric, not executive scorecard metric

**Trend Analysis Page Value:**
- Answers: "Are we improving over time?"
- Shows: Performance trajectory, seasonal patterns, variance trends
- Enables: Year-over-year comparisons, pattern identification
- Time saved: No manual Excel analysis of monthly data needed

**Test Type Analysis Page Value:**
- Answers: "Which products drive our business? Where should we invest?"
- Shows: Portfolio composition, product performance, growth rates
- Enables: Product strategy decisions, investment prioritization, sunset analysis
- Critical for: Marketing budget allocation, R&D investment, capacity planning

---

### Files Created
- `power-bi/screenshots/03-trend-analysis-final.png` - Trend Analysis page with 24-month views
- `power-bi/screenshots/04-test-type-analysis-final.png` - Test Type Analysis page with 4 visuals
- `power-bi/GTE_Dashboard_v3.pbix` - Power BI file with 4 complete pages

---

## Week 3 Day 5: Dashboard Polish & Finalization - COMPLETE ✅

**Date:** December 3, 2025  
**Duration:** 2 hours  
**Status:** All 4 pages finalized and production-ready

### Objectives
- Ensure visual consistency across all 4 dashboard pages
- Verify color palette and typography standards
- Configure edit interactions correctly on Trend Analysis
- Capture final screenshots for documentation
- Save production-ready Power BI file

---

### Deliverables Completed

**1. Cross-Page Consistency Review**
- ✅ Year slicer positioning standardized (top-right on Executive Summary, Geographic Analysis, Trend Analysis, Test Type Analysis)
- ✅ Color palette verified across all pages (Blue #004C97 for Actual, Orange #F2A900 for Budget)
- ✅ Typography consistency checked (Page titles 24pt bold, Visual titles 14pt bold, Data labels 9-10pt)
- ✅ Spacing and margins verified (consistent visual hierarchy)

**2. Edit Interactions Verification**
- ✅ Trend Analysis page: Year slicer disabled for both trend charts (hero chart and variance chart)
- ✅ All 24 months (Jan 2024 - Nov 2025) display correctly regardless of year filter selection
- ✅ YoY Growth card still responds to year filter as intended
- ✅ Other pages: Year slicer filters all visuals correctly

**3. Theme Colors Applied Consistently**
- ✅ Primary Blue #004C97: All "Actual" volume bars, lines, and references
- ✅ Primary Orange #F2A900: All "Budget" volume bars, lines, and references
- ✅ Green #70AD47 / #C6EFCE: Positive variances, exceeding budget, growth indicators
- ✅ Red #FF0000 / #FFC7CE: Negative variances, missing budget, decline indicators
- ✅ Conditional formatting working correctly on all matrices and charts

**4. Final Screenshots Captured**
- ✅ `01-executive-summary-final.png` - Full page with Budget Attainment % KPI
- ✅ `02-geographic-analysis-final.png` - Full page with Azure Maps and regional analysis
- ✅ `03-trend-analysis-final.png` - Full page with 24-month trend views
- ✅ `04-test-type-analysis-final.png` - Full page with product portfolio analysis

**5. Production File Saved**
- ✅ `GTE_Dashboard_v4_FINAL.pbix` - Ready for deployment or presentation

---

### Key Learnings

**Design System Consistency**
- Establishing a documented design system (colors, fonts, spacing) at the start ensures professional appearance across all pages
- Consistency builds trust with executives - they know how to read each page instantly
- Small details matter: matching shadow depths, alignment, and spacing convey quality

**Context-Aware Filter Behavior**
- Not all pages need the same slicer behavior - understanding page purpose drives configuration
- Trend Analysis requires full time series (24 months) for context, so year slicer must be disabled
- Other pages benefit from year filtering for focused comparisons (2024 vs 2025)
- Edit interactions are a critical Power BI feature for sophisticated dashboards

**Edit Interactions Must Be Reconfigured**
- Moving or copying slicers resets interaction settings to defaults
- Always verify edit interactions after layout changes
- This is easy to miss but breaks the dashboard experience if overlooked
- Document which pages have special interaction configurations

**Page-Specific Design Decisions**
- Executive Summary: Year slicer top-right creates visual balance with page title top-left
- Geographic Analysis: Year slicer top-right maintains consistency
- Trend Analysis: Year slicer position less critical since it doesn't filter main visuals
- Test Type Analysis: Follows Executive Summary pattern for consistency

**Screenshot Naming Convention**
- Numbered screenshots (01-, 02-, 03-, 04-) enforce sort order and show navigation flow
- Much better than alphabetical sorting which might not match logical page order
- Professional standard in enterprise dashboards

---

### Final Dashboard Statistics

**Pages:**
- 4 total pages (Executive Summary, Geographic Analysis, Trend Analysis, Test Type Analysis)
- Logical navigation flow from high-level summary → geographic detail → time analysis → product analysis

**Visuals:**
- 17 total visualizations across all pages
- Mix of KPI cards (4), charts (6), maps (1), matrices (2), gauges (1), multi-line charts (1), donut charts (1), regional cards (3)

**DAX Measures:**
- 19+ measures implemented
- Volume measures: Total Actual Volumes, Total Budget Volumes, Total Forecast Volumes
- Variance measures: Variance Amount, Budget Attainment %
- Growth measures: YoY Growth %, YoY Growth Amount
- Time intelligence: Previous Year calculations, date-based filtering
- Supporting measures: Country rankings, data freshness indicators

**Data Model:**
- Star schema with 4 tables
- Fact table: VolumeSubmissions (138 records)
- Dimension tables: Countries (6), TestTypes (6), DateTable (731 days / 24 months)
- Proper Many-to-One relationships enforced
- Clean, optimized for DirectQuery performance

**Advanced Features:**
- Azure Maps integration with proportional bubble sizing
- Conditional formatting (4 instances: Geographic matrix, Trend variance, Test Type matrix)
- Edit interactions configured (Trend Analysis page)
- Hierarchical drill-down (Region → Country in Geographic matrix)
- Time intelligence calculations (YoY comparisons, month-over-month)
- Cross-visual filtering and highlighting

**Data Coverage:**
- 6 countries across 3 regions (Asia Pacific, Western Europe, North America)
- 6 test types (Leadership Fundamentals, Project Management, Executive Leadership, Data Analytics, Technical Skills, Compliance Training)
- 24-month time series (January 2024 - November 2025)
- 138 volume submission records
- 44,865 total actual volumes, 44,990 total budget volumes

---

### Files Created This Session

**Screenshots (Final Versions):**
- `power-bi/screenshots/01-executive-summary-final.png`
- `power-bi/screenshots/02-geographic-analysis-final.png`
- `power-bi/screenshots/03-trend-analysis-final.png`
- `power-bi/screenshots/04-test-type-analysis-final.png`

**Power BI File:**
- `power-bi/GTE_Dashboard_v4_FINAL.pbix` - Production-ready dashboard

**Documentation:**
- Updated `power-bi/dashboard-documentation.md` with Week 3 Day 5 entry
- Updated `README.md` with Phase 3 completion and screenshot descriptions

---

### Business Value Delivered

**For CFO / Executive Leadership:**
- 30-second health check (Executive Summary) answers: "How are we doing overall?"
- Geographic visibility (Geographic Analysis) answers: "Which regions/countries need attention?"
- Performance trajectory (Trend Analysis) answers: "Are we improving? What's the trend?"
- Product insights (Test Type Analysis) answers: "Which tests drive our business? Should we invest or divest?"

**For Finance Team:**
- Real-time variance analysis (no waiting for month-end reports)
- Automated anomaly detection (conditional formatting highlights issues instantly)
- Drill-down capability (Region → Country → Test Type)
- Export-ready visuals for presentations

**For Data Quality Analysts:**
- Clear submission status visibility
- Standardized data entry reduces validation time
- Historical tracking enables pattern identification

---

### Reflections

**What Went Well:**
- Design iteration process yielded better results than first attempts
- Systematic polish checklist caught inconsistencies efficiently
- Edit interactions configuration preserved full context on Trend page
- Color consistency and typography standards created professional appearance

**What Could Be Improved:**
- Could have established design system document earlier (did it iteratively instead)
- Edit interactions should be documented immediately when configured (easy to forget)
- Screenshot naming convention should have been numbered from start

**Key Takeaway:**
"More information in the same space is useless if it can't be translated immediately" - This principle guided every design decision. The dashboard prioritizes insight extraction speed over data density. Executives can answer key questions in seconds, not minutes.

---

**Week 3 Day 5 Status: COMPLETE** ✅  
**Dashboard Pages: 4 of 4 FINALIZED** ✅  
**Production Ready: YES** ✅

---


