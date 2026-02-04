
📘 ReadMe — Sales & Operations Analytics Dataset
1. Data Issues Identified
Duplicates 
Multiple rows with identical OrderID, SKU, and OrderDate values.
Data Types 
Dates stored as text strings.
Numeric fields (UnitPrice, UnitCost, Quantity, DiscountPct) sometimes stored as text.
Missing Values 
City, Salesperson, and Channel fields occasionally blank.
Suspicious Values 
Negative UnitPrice values.
Discounts greater than 30%.
RequiredDate earlier than OrderDate.
Gross Profit or Net Profit showing negative values due to incorrect inputs.
Inconsistent Hierarchies 
Region → Country → City not always aligned (e.g., mismatched country-city pairs).
Derived Fields Missing 
LeadTimeDays not calculated.
MarginPct missing or incorrect.
Outliers 
Unrealistic UnitCost or UnitPrice (extremely high/low compared to category median).
DiscountPct = 0 for bulk orders where discounts are expected.

2. Cleaning Rules Applied
Duplicates 
Removed rows where OrderID, SKU, OrderDate, and CustomerSegment were identical.
Data Types 
Converted OrderDate and RequiredDate to proper Date format.
Converted UnitPrice, UnitCost, Quantity, DiscountPct to numeric.
Missing Values 
City: Imputed using most frequent city for that Country.
Salesperson: Assigned “Unknown” if missing, else imputed from similar Region/Channel.
Channel: Defaulted to “Distributor” if blank.
Suspicious Values 
Negative UnitPrice → replaced with median UnitPrice for SKU.
Discounts > 30% → capped at 30%.
RequiredDate earlier than OrderDate → corrected to OrderDate + 7 days.
Derived Columns 
LeadTimeDays = RequiredDate − OrderDate.
GrossRevenue = UnitPrice × Quantity × (1 − DiscountPct).
CostOfGoods = UnitCost × Quantity.
GrossProfit = GrossRevenue − CostOfGoods.
MarginPct = IF(GrossRevenue=0,0,GrossProfit/GrossRevenue).
Standardized Dimensions 
Month = MMM-YYYY from OrderDate.
Quarter = Qn-YYYY.
Region hierarchy standardized.
PriceBand = Low/Medium/High using revenue quantiles.

3. Assumptions Made
RequiredDate should always be ≥ OrderDate; minimum lead time assumed = 7 days.
Discounts above 30% are considered data-entry errors.
Missing City values are assumed to follow the most common city for that Country.
Salesperson blanks are treated as “Unknown” rather than deleted.
Channel defaults to “Distributor” when missing, as it is the most common fallback.
PriceBand classification based on GrossRevenue quantiles (Low = bottom 33%, Medium = middle 33%, High = top 33%).
Negative profits are retained but flagged for review (possible pricing errors).
Cohort analysis assumes first appearance of a Country in dataset = entry month.


