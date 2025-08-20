# ETL & Data Prep (Tableau)

## Clean invalid values
**Transactions ➜ Data ➜ Edit Data Source Filters ➜ Add**
- Field: `Sales Amount`
- Condition: **At least** `1` (excludes 0 and -1)

## Limit to India markets
**Data ➜ Edit Data Source Filters ➜ Add**
- Field: `markets_code`
- Unselect: `Mark097`, `Mark999` (e.g., New York and Paris)

## Currency normalization (USD ➜ INR)
**Transactions ➜ (Dropdown above `Currency`) ➜ Create Calculated Field**

Name: `Normalized Amount`

```tableau
IF [Currency] = 'USD' THEN [Sales Amount] * 88
ELSE [Sales Amount]
END
```

> Use your preferred exchange rate or a date-wise table if available.

## Modeling
- Fact: **Transactions**
- Dimensions: Markets, Customers, Products, Date (Year/Month fields ready for slicing)

