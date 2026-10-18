# Week 1: Supplier Performance

**Business question:** Which suppliers cost the business money through late delivery, poor quality and price, and which bid is truly the cheapest?

## Files
| Folder | File | What it is |
|---|---|---|
| Data | Halvorn Supplier Data.xlsx | Purchase orders, receiving inspection, RFQ bids, sourcing events, suppliers and materials as Excel tables |
| Excel | Supplier Performance Analysis.xlsx | Power Query model with one sheet per KPI: OTD and OTIF, Lead Time, PPM, PPV, Savings, TCO Evaluator and Scorecard |
| Tableau | Supplier Performance Dashboard.twbx | Interactive dashboard. Year and supplier filters for every chart, a side by side comparison of any 2 suppliers, adjustable scorecard weights, and green to red performance colors with legends |
| LinkedIn | Supplier Performance Carousel.pdf, Slides, KPI Summary Table.png | 9 slide carousel (PDF and one PNG per slide) used in the LinkedIn post |

## Key findings
| KPI | 2024 | 2025 |
|---|---|---|
| On Time Delivery | 81.4% | 90.3% |
| On Time In Full | 78.4% | 86.1% |
| Purchase Price Variance | +$1.91M | -$7.15M |
| Suppliers above 1,000 PPM | 7 of 17 | 3 of 15 |
| Suppliers graded A or B (industry benchmark bands) | 2 of 17 | 4 of 15 |

- Nine sourcing events saved $11.48M and four negotiations avoided $1.05M.
- The lowest NF3 quote cost $1.40/kg more on total cost of ownership, about $315K per year.
- Lead time variability, not lead time length, drove safety stock: a supplier averaging 23 days needed 14 days of safety stock, while one averaging 62 days needed fewer than 2.

## Scorecard method
Weights: delivery (OTIF) 35%, quality (PPM) 35%, cost (PPV) 20%, lead time variability 10%.
Bands: OTIF 95/90/85/80%, PPM 250/500/750/1,000. Grades: A 89 and above, B 79, C 69, D below 69. Suppliers with fewer than 5 orders are marked "Not enough data", and any supplier above 1,000 PPM is flagged for corrective action.

**How years are counted:** delivery, lead time, PPV and the scorecard use the order year (`po_date`). Defect rate uses the receipt year. Backorder balance rows are left out of every PO based KPI.

## Links
- Tableau Public dashboard: [add link]
- LinkedIn post: [add link]
