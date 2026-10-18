# Dataset design: Halvorn Memory Technologies

> **This data is simulated.** Halvorn Memory Technologies is a fictional company, modeled on how large memory chip manufacturers operate. None of the numbers come from a real company.

Week 1 uses the workbook in `Week 1 Supplier Performance/Data`. Data for later weeks is added to each week's own Data folder.

---

## The company

A mid-size memory chip maker with about **$1.3 to 1.4 billion in yearly revenue**.

| Part of the business | What it looks like in the data |
|---|---|
| Purchasing | 21 materials from 19 suppliers: silicon wafers, photoresist, specialty gases, wet chemicals, CMP consumables, sputtering targets, precursors, spare parts, package substrates, packaging |
| Wafer fab | One 300 mm fab, about 4,800 wafer starts per week, 41 front-end tools in 8 tool groups |
| Test | Wafer probe (5 testers) and final test (5 testers) |
| Inventory stages | Raw materials, wafers in fab (WIP), die bank, finished goods |
| Products | 10 memory products: DRAM (DDR5, LPDDR5X, DDR4, automotive), HBM3E stacks for AI, and NAND (TLC, QLC, automotive) |
| Customers | Data center, PC, mobile and automotive segments |

**How material flows:** materials are consumed when a lot of 25 wafers starts → the lot spends about 2 to 3 months in the fab → wafers are probed, and good die go to the die bank → die are assembled and tested → finished units are shipped to customers.

**Period:** weekly, from 2024-01-01 to 2025-12-28 (104 weeks). Week 1 is the first week of 2024, and week 53 is the first week of 2025.

---

## What each section is about (the business questions)

### 1. Supplier performance
- Which suppliers deliver late or send bad material, and what does that cost us?
- Are we paying more than planned (PPV), and why?
- Which supplier is really cheapest once quality, freight, risk and payment terms are included (TCO)?
- How much money did sourcing events save or avoid?

**Tables:** `purchase_orders`, `receiving_inspection`, `sourcing_events`, `rfq_bids`, `suppliers`, `materials`

### 2. Operations
- Which tools limit output (the bottleneck), and how much output does their downtime cost?
- Does preventive maintenance pay off (MTBF, MTTR, downtime)?
- How much revenue is lost to scrapped wafers and yield loss?
- How long does a wafer take to become sellable (cycle time, WIP, throughput, Little's Law)?
- Is the litho process capable (Cp/Cpk), and what does a drifting tool cost in yield?
- Did we build what we planned (plan attainment)?

**Tables:** `tool_weekly`, `downtime_events`, `wafer_lots`, `process_measurements`, `production_plan_weekly`, `quality_costs_monthly`, `tools`, `products`

### 3. Inventory
- How much cash is tied up at each stage (raw material, WIP, die bank, finished goods)?
- Turnover, days of inventory, days of supply: where is there too much or too little?
- Fill rate, stockouts, backorders: what revenue was delayed?
- How good is the forecast, and what does forecast error cost?
- Excess and obsolete stock, plus the risk that falling memory prices push stock value below cost.
- Working capital.

**Tables:** `inventory_weekly`, `demand_weekly`, `sales_weekly`, `market_prices_monthly`, `finance_monthly`, `products`, `materials`

### 4. Risk (across supply, operations and inventory)
- Single-source materials and how many products depend on them
- Revenue at risk if a material, supplier or tool fails
- Time to survive (stock on hand) vs. time to recover
- Supplier risk score
- Scenario testing (Monte Carlo): is a mitigation like a buffer or second source worth its cost?

**Tables:** all of the above, especially `materials` (primary/secondary supplier, usage per wafer), `suppliers` (time to recover, financial health) and `products` (revenue per wafer).

---

## Events built into the data

These are the stories your KPIs should uncover. Week numbers are report weeks (1 = first week of 2024).

| Week(s) | Event | Area |
|---|---|---|
| 17–20 | A bad photoresist lot passes incoming inspection. Lots started in weeks 18–20 lose die yield. | Supplier quality → yield |
| 27–38 | Litho tool LTH-DUV-04 drifts off target: its critical dimension shifts and its lots yield less. | Operations, Cp/Cpk |
| 33–41 | Photoresist supplier S03 (single source) has an outage. POs deliver only 40%, and the rest arrives 6 weeks late. | Supplier, risk |
| 40 → 78 | DDR4 demand winds down to end of life. Last orders in week 78. | Inventory, E&O |
| 47 | Panic buy: 34 weeks of photoresist ordered after the outage. Photoresist has a 180-day shelf life. | Inventory, risk |
| 53 | 2025 programs start: supplier development, litho preventive maintenance, better forecasting, margin-based capacity allocation, smaller finished goods buffers, standard cost reset | All |
| 58–70 | Litho spare kits from S14 arrive about 2 months late, so some repairs wait for parts. | Operations, risk |
| 60 → | HBM demand surges about 45% (AI demand). | Operations, inventory |
| 66 | Major failure on litho tool LTH-DUV-03 (96 hours down) | Operations |
| 72 | Second photoresist source (S04) qualified and starts taking about 35% of orders. | Supplier, risk |
| 80 → | NAND market price falls about 28%. | Inventory value risk |
| ~83–86 | Part of the panic-bought photoresist expires, and the fab loses wafer starts until an expedite order arrives. | Inventory, risk |

Some smaller supply problems also appear naturally, such as late deliveries from weaker suppliers. Finding them is part of the work.

---

## Data dictionary

Dates are ISO format (YYYY-MM-DD). Money is in US dollars. Table names are listed as files below. In Week 1, the six supplier tables are sheets in `Halvorn Supplier Data.xlsx` (for example `purchase_orders` is the `Purchase_Orders` sheet).

| File | One row per | Key columns |
|---|---|---|
| `products.csv` | product | family, segment, process_node, gross_die_per_wafer, mature_die_yield, die_per_unit (HBM = 8), base_asp_usd, wafer_std_cost, die_std_cost, unit_std_cost, final_test_yield, euv_layers |
| `suppliers.csv` | supplier | country_region, category, financial_health_1to5, est_time_to_recover_weeks |
| `materials.csv` | material | category, uom, std_cost_2024, quoted_lead_time_weeks, primary/secondary supplier (2024), used_in, qty_per_wafer_start, qty_per_unit_component, qty_per_unit_hbm, shelf_life_days |
| `tools.csv` | tool | tool_group, area, ideal_wafer_passes_per_hour, purchase_cost_musd |
| `market_prices_monthly.csv` | month | DRAM, NAND and HBM price index (1.0 = Sep 2023) |
| `demand_weekly.csv` | product per week | demand_units, forecast_units (forecast made 13 weeks earlier) |
| `production_plan_weekly.csv` | product per week | planned_wafer_starts, actual_wafer_starts |
| `purchase_orders.csv` | PO line | requisition_date, po_date, promised_date, receipt_date, order_qty, received_qty, unit_price, std_cost, po_type (Standard, Expedite, Strategic buy, Backorder balance) |
| `receiving_inspection.csv` | delivery | qty_received, qty_rejected, defect_type |
| `sourcing_events.csv` | sourcing event | event_type (RFI, RFQ, RFP, price increase negotiation), baseline vs. awarded price, volume, requested vs. negotiated increase |
| `rfq_bids.csv` | bid | unit_price, freight, lead time, supplier defect PPM, payment terms, one-time qualification cost, awarded |
| `tool_weekly.csv` | tool per week | scheduled, planned maintenance, unplanned down, engineering, productive and standby hours; wafer_passes, rework_passes |
| `downtime_events.csv` | breakdown | tool_id, start_datetime, duration_hours, failure_mode |
| `wafer_lots.csv` | lot of 25 wafers | start and fab-out time, litho and deposition tool used, wafers started/out, gross and good die, status |
| `process_measurements.csv` | measurement | lot, tool, parameter (critical dimension or film thickness), site, LSL, target, USL, value |
| `inventory_weekly.csv` | item per stage per week | stage (Raw material, Wafers in fab (WIP), Die bank, Finished goods), begin, receipts, issues, scrap/write-off, end, backorder, on order |
| `sales_weekly.csv` | product per week | units_shipped, avg_selling_price, revenue |
| `finance_monthly.csv` | month | accounts_receivable, accounts_payable |
| `quality_costs_monthly.csv` | month | prevention, appraisal and customer returns cost (internal failure comes from scrap and rework in the other tables) |

### Rules worth knowing
- **Inventory units differ by stage:** raw materials use their own unit (see `materials.uom`), WIP is in wafers, the die bank is in die, and finished goods are in units. An HBM unit is a stack of 8 die.
- **Valuing stock:** use `materials.std_cost_2024` for raw materials, `products.die_std_cost` for the die bank and `products.unit_std_cost` for finished goods. The WIP row is a wafer count only.
- **Tool hours:** `scheduled = planned maintenance + unplanned down + engineering + productive + standby`. Choosing which of these count against OEE is part of Week 2.
- **Purchase orders:** a "Backorder balance" row is the late remainder of a partly delivered PO (same PO number plus "-B"). Week 1 leaves these rows out of every PO based KPI.
- **Which year:** Week 1 places POs in a year by `po_date` (order year) for OTD, OTIF, lead time, PPV and the scorecard, and places deliveries in a year by `receipt_date` for defect rate (PPM). See KPI Definitions for details.
- **Standard costs reset** at the start of 2025, so use `std_cost` on each PO line for PPV.
- **Supplier quality tiers are not in the data.** Discovering who is reliable is part of the work.

---

## Using it in Tableau Public
- Week 1: open the packaged dashboard in `Week 1 Supplier Performance/Tableau`, or connect to the Excel workbook in its Data folder (Microsoft Excel connector).
- Later weeks: link tables with relationships on `product_id`, `material_id`, `supplier_id`, `tool_id` and `lot_id`.
- `inventory_weekly` uses `item_id`, which is a material_id for raw materials and a product_id for the other stages.
- Weekly tables share `week` and `week_start`, so you can line them up on one timeline.
