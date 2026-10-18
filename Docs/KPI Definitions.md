# KPI definitions

How each KPI is calculated in this project, which table it comes from, and the business decision it supports.
All data is simulated (fictional company: Halvorn Memory Technologies).

---

## Week 1: Supplier Performance

Main tables: `purchase_orders`, `receiving_inspection`, `sourcing_events`, `rfq_bids`, `suppliers`, `materials`.

**Before you start:**
- In `purchase_orders`, leave out rows where `po_type = "Backorder balance"` for every PO based KPI (OTD, OTIF, lead time and PPV). They are the late remainder of a partly delivered PO, not a separate order.
- **How a year is assigned:** OTD, OTIF, lead time, PPV and the scorecard place each PO in the year of its `po_date` (order year), because the price, standard cost and promised date are all fixed when the order is placed. Defect rate (PPM) places each delivery in the year of its `receipt_date`, because defects are found when material is received.

| KPI | Formula | Business decision |
|---|---|---|
| **On Time Delivery (OTD %)** | POs with `receipt_date <= promised_date` ÷ all POs | Which suppliers to develop, penalize or replace |
| **On Time In Full (OTIF %)** | POs that are on time **and** `received_qty >= order_qty` ÷ all POs | Stricter view of reliability. This is what production actually feels. |
| **Lead time (days)** | `receipt_date - po_date` | Planning parameter. Longer lead time means more stock is needed. |
| **Lead time variability** | Standard deviation of lead time; also the coefficient of variation = std dev ÷ average | Unpredictable suppliers force extra safety stock, which costs cash |
| **Supplier defect rate (PPM)** | `qty_rejected ÷ qty_received × 1,000,000` | Cost of poor supplier quality; where to push for improvement |
| **Purchase price variance (PPV)** | `(unit_price - std_cost) × received_qty`. Positive = paid more than standard (unfavorable). PPV % = PPV ÷ (std_cost × received_qty). | Are we overpaying, and why: inflation, expedites, or contract terms? |
| **Total cost of ownership (TCO) per unit** | price + freight + quality cost + delivery risk cost − payment terms benefit + one-time cost ÷ annual volume (see below) | Which bid is really cheapest |
| **Cost savings ($)** | (baseline price − awarded price) × estimated annual volume, for RFQ/RFP events with an award | Dollar value created by sourcing |
| **Cost avoidance ($)** | (requested increase % − negotiated increase %) × baseline price × annual volume, for price increase negotiations | Money not spent because of negotiation |
| **Supplier scorecard (0 to 100)** | Weighted score of delivery, quality, cost and lead time reliability (see below) | One number to rank suppliers and share business |

### TCO formula used for RFQ evaluation (per unit)
| Part | Calculation |
|---|---|
| Price | `unit_price` |
| Freight | `freight_per_unit` |
| Quality cost | price × `supplier_defect_ppm` ÷ 1,000,000 × 25 (a rejected unit costs about 25× its price in scrap, rework and disruption) |
| Delivery risk cost | price × (1 − supplier OTD) × 0.20 (use the supplier's measured OTD from your Week 1 work) |
| Payment terms benefit | price × 8% × (`payment_terms_days` − 30) ÷ 365 (longer terms keep cash in the company) |
| One-time cost | `one_time_qualification_cost` ÷ estimated annual volume |

### Supplier scorecard (industry based bands, editable in the Excel tool)
Weights: Delivery 35%, Quality 35%, Cost 20%, Lead time reliability 10% (quality and delivery weighted highest for critical materials).

| Category | KPI | 100 pts | 75 pts | 50 pts | 25 pts | 0 pts | Basis |
|---|---|---|---|---|---|---|---|
| Delivery | OTIF % (at least) | 95% | 90% | 85% | 80% | below 80% | Published industry supplier scorecard OTD bands |
| Quality | PPM (at most) | 250 | 500 | 750 | 999 | 1,000 or more | Published industry supplier scorecard PPM bands (manufacturers) |
| Cost | PPV % (at most) | 0% | 1% | 2% | 3% | above 3% | Halvorn assumption |
| Lead time reliability | CV (at most) | 10% | 20% | 30% | 40% | above 40% | Halvorn assumption |

**Overall score** = weighted average of the points. **Grades:** A ≥ 89, B ≥ 79, C ≥ 69, D below 69 (published rating bands). Suppliers with fewer than 5 POs are "Not enough data". Any supplier above **1,000 PPM** is flagged for corrective action.

Sources: Stanley Black & Decker Healthcare Supplier Performance Scorecards; Tacto procurement glossary (Quality PPM); Intel supplier awards (80% and 95% score thresholds, for reference).

---

## Week 2: Operations
_Added at the start of Week 2._

## Week 3: Inventory
_Added at the start of Week 3._

## Week 4: Risk
_Added at the start of Week 4._
