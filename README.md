# Supply Chain Case Study
### Supply chain KPIs for a memory chip manufacturer

A 4 week project that calculates supply chain KPIs and uses them to make decisions that cut cost, improve quality and support growth. Each week has an Excel workbook, an interactive Tableau dashboard and a LinkedIn post.

**Author:** Kunj Patel, M.S. Engineering Management, San José State University

## The company
**Halvorn Memory Technologies** is a mid size memory chip maker (DRAM, HBM and NAND) with about $1.3 to 1.4 billion in yearly revenue. It buys 21 materials from 19 suppliers, runs one 300 mm wafer fab and sells 10 products to data center, PC, mobile and automotive customers. The company and its data are simulated for this case study.

## The 4 weeks
| Week | Section | KPIs | Status |
|---|---|---|---|
| 1 | [Supplier Performance](Week%201%20Supplier%20Performance) | On Time Delivery, On Time In Full, lead time variability, defect rate (PPM), purchase price variance, total cost of ownership, cost savings and avoidance, supplier scorecard | Complete |
| 2 | Operations | Equipment effectiveness, downtime, yield, cycle time, capacity utilization, cost of poor quality | Coming soon |
| 3 | Inventory | Turnover, days of supply, safety stock, fill rate, forecast accuracy, excess and obsolete | Coming soon |
| 4 | Risk | Single source exposure, supplier risk score, revenue at risk, time to survive vs time to recover, scenario testing | Coming soon |

## Folder structure
```
Supply Chain Case Study
├── README.md
├── Docs
│   ├── KPI Definitions.md        how every KPI is calculated
│   └── Data Design.md            company, business questions and data dictionary
├── Week 1 Supplier Performance
│   ├── Data                      source data (Excel tables)
│   ├── Excel                     analysis workbook with every formula
│   ├── Tableau                   packaged dashboard (.twbx)
│   └── LinkedIn                  carousel, slides and KPI table
├── Week 2 Operations
├── Week 3 Inventory
└── Week 4 Risk
```

## How to use the files
1. Download the repository (Code > Download ZIP) and unzip it.
2. **Excel:** open `Week 1 Supplier Performance/Excel/Supplier Performance Analysis.xlsx`. All data, formulas and PivotTables are already inside the workbook, so it works as soon as it opens. To refresh the data, go to Data > Get Data > Data Source Settings > Change Source and pick `Halvorn Supplier Data.xlsx` from the Data folder.
3. **Tableau:** open `Week 1 Supplier Performance/Tableau/Supplier Performance Dashboard.twbx` with Tableau Public or Tableau Desktop. The data is packaged inside the file, so no connection setup is needed.

## Tools
Microsoft Excel (Power Query, PivotTables, Power Pivot) and Tableau Public. No coding is needed.
