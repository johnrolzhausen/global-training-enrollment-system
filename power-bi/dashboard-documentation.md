# Week 3 Day 2: Executive Summary Page - COMPLETE

## Deliverables
- ✅ 4 KPI cards (Actual, Budget, YoY Growth, Approved Submissions)
- ✅ Line chart with seasonal trend analysis
- ✅ Top 5 countries bar chart with data labels
- ✅ Performance vs Budget gauge
- ✅ Year slicer (tile style, compact, functional)
- ✅ 138 records of realistic test data loaded via Power Automate

## Key Learnings
- Sort by column required for proper chronological display
- Time intelligence measures work across filter boundaries
- Slicer orientation controlled by visual dimensions, not explicit setting
- +0 trick converts BLANK to 0 in measures

## Metrics Achieved
- Total Actual Volumes: 45K (2025)
- YoY Growth: 2.2% (positive growth in green)
- Approved Submissions: 66 (11 months × 6 countries)
- Performance vs Budget: -0.3% (realistic variance)

## Technical Highlights
- Custom Power Automate CSV parser (no premium connectors)
- Lookup field translation for Country and TestType
- DateTable with daily granularity supporting monthly data
- 17 DAX measures including time intelligence calculations


## Files Created
- `power-bi/screenshots/executive-summary-page-final.png` - Final page screenshot
- `GTE_Dashboard_v1.pbix` - Power BI file with complete Executive Summary
---

# Week 3 Day 3: Geographic Analysis Page - COMPLETE

## Deliverables
- ✅ Azure Maps visualization with proportional bubble sizing (6 countries)
- ✅ Hierarchical matrix with Region → Country drill-down capability
- ✅ Conditional formatting on variance column (red/green background colors)
- ✅ 3 regional KPI cards (Asia Pacific, Western Europe, North America)
- ✅ Regional comparison clustered column chart (Actual vs Budget)
- ✅ Year slicer integration (positioned top-center for consistency)
- ✅ Professional layout with balanced visual hierarchy

**Key Questions Answered:**
   - Where are enrollments concentrated globally? (Map)
   - How do regions and countries compare? (Matrix, Cards)
   - Are we meeting budget targets by region? (Bar Chart, Conditional Formatting)
   - Which countries need attention? (Matrix with variance highlighting)

## Key Learnings
   - Setup process varied from standard docs but delivered working solution
   - Demonstrates adaptability to platform changes
   - Power BI auto-display units only trigger at 10,000+
   - Values < 10K show as whole numbers (7,475 not 7.5K)
   - Recent Power BI versions removed direct text editing for card category labels
   - Workaround: Toggle category label OFF, use text box positioned below card
   - Inner padding setting creates visual gap between bars in same group
   - Critical when actual vs budget values are very close (22.1K vs 22.1K)
   - 25-30% inner padding provides clear visual separation
   - Improves readability without requiring data labels
   - "Rename for this visual" option in Build visual → Y-axis field well
   - Only renames within current visual, not globally
   - Essential for clean legend text ("Actual" vs "Total Actual Volumes")

## Metrics Summary

### Data Coverage
- **Regions:** 3 (Asia Pacific, Western Europe, North America)
- **Countries:** 6 (Korea, Japan, France, Germany, Italy, USA)
- **Test Types:** 5 courses
- **Time Period:** 2024-2025 (24 months)
- **Total Records:** 138 volume submissions

### Performance Metrics (2025 YTD)
- **Total Actual Volumes:** 44,865
- **Total Budget Volumes:** 44,990
- **Overall Variance:** -0.3% (125 volumes under budget)
- **Regional Variance Range:** -0.7% (Western Europe) to -0.0% (Asia Pacific)
- **Country Variance Range:** -2.0% (Italy) to +0.2% (Korea)

### Visual Elements
- **Interactive elements:** 6 (map, matrix, 3 cards, bar chart)
- **Filter controls:** 1 (year slicer)
- **Drill-down levels:** 2 (region → country)
- **Conditional formatting rules:** 2 (positive/negative variance)
- **Cross-filtering connections:** Automatic (all visuals share dimensions)


## Files Created
- `power-bi/screenshots/geographic-analysis-page-final.png` - Final page screenshot
- `GTE_Dashboard_v2.pbix` - Power BI file with 2 complete pages

# Week 3 Day 2: Executive Summary Page - COMPLETE

## Deliverables
- ✅ 4 KPI cards (Actual, Budget, YoY Growth, Approved Submissions)
- ✅ Line chart with seasonal trend analysis
- ✅ Top 5 countries bar chart with data labels
- ✅ Performance vs Budget gauge
- ✅ Year slicer (tile style, compact, functional)
- ✅ 138 records of realistic test data loaded via Power Automate

## Key Learnings
- Sort by column required for proper chronological display
- Time intelligence measures work across filter boundaries
- Slicer orientation controlled by visual dimensions, not explicit setting
- +0 trick converts BLANK to 0 in measures

## Metrics Achieved
- Total Actual Volumes: 45K (2025)
- YoY Growth: 2.2% (positive growth in green)
- Approved Submissions: 66 (11 months × 6 countries)
- Performance vs Budget: -0.3% (realistic variance)

## Technical Highlights
- Custom Power Automate CSV parser (no premium connectors)
- Lookup field translation for Country and TestType
- DateTable with daily granularity supporting monthly data
- 17 DAX measures including time intelligence calculations


## Files Created
- `power-bi/screenshots/executive-summary-page-final.png` - Final page screenshot
- `GTE_Dashboard_v1.pbix` - Power BI file with complete Executive Summary
---

# Week 3 Day 3: Geographic Analysis Page - COMPLETE

## Deliverables
- ✅ Azure Maps visualization with proportional bubble sizing (6 countries)
- ✅ Hierarchical matrix with Region → Country drill-down capability
- ✅ Conditional formatting on variance column (red/green background colors)
- ✅ 3 regional KPI cards (Asia Pacific, Western Europe, North America)
- ✅ Regional comparison clustered column chart (Actual vs Budget)
- ✅ Year slicer integration (positioned top-center for consistency)
- ✅ Professional layout with balanced visual hierarchy

**Key Questions Answered:**
   - Where are enrollments concentrated globally? (Map)
   - How do regions and countries compare? (Matrix, Cards)
   - Are we meeting budget targets by region? (Bar Chart, Conditional Formatting)
   - Which countries need attention? (Matrix with variance highlighting)

## Key Learnings
   - Setup process varied from standard docs but delivered working solution
   - Demonstrates adaptability to platform changes
   - Power BI auto-display units only trigger at 10,000+
   - Values < 10K show as whole numbers (7,475 not 7.5K)
   - Recent Power BI versions removed direct text editing for card category labels
   - Workaround: Toggle category label OFF, use text box positioned below card
   - Inner padding setting creates visual gap between bars in same group
   - Critical when actual vs budget values are very close (22.1K vs 22.1K)
   - 25-30% inner padding provides clear visual separation
   - Improves readability without requiring data labels
   - "Rename for this visual" option in Build visual → Y-axis field well
   - Only renames within current visual, not globally
   - Essential for clean legend text ("Actual" vs "Total Actual Volumes")

## Metrics Summary

### Data Coverage
- **Regions:** 3 (Asia Pacific, Western Europe, North America)
- **Countries:** 6 (Korea, Japan, France, Germany, Italy, USA)
- **Test Types:** 5 courses
- **Time Period:** 2024-2025 (24 months)
- **Total Records:** 138 volume submissions

### Performance Metrics (2025 YTD)
- **Total Actual Volumes:** 44,865
- **Total Budget Volumes:** 44,990
- **Overall Variance:** -0.3% (125 volumes under budget)
- **Regional Variance Range:** -0.7% (Western Europe) to -0.0% (Asia Pacific)
- **Country Variance Range:** -2.0% (Italy) to +0.2% (Korea)

### Visual Elements
- **Interactive elements:** 6 (map, matrix, 3 cards, bar chart)
- **Filter controls:** 1 (year slicer)
- **Drill-down levels:** 2 (region → country)
- **Conditional formatting rules:** 2 (positive/negative variance)
- **Cross-filtering connections:** Automatic (all visuals share dimensions)


## Files Created
- `power-bi/screenshots/geographic-analysis-page-final.png` - Final page screenshot
- `GTE_Dashboard_v2.pbix` - Power BI file with 2 complete pages


