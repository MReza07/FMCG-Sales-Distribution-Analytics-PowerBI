# FMCG Sales & Distribution Analytics (Power BI)

An end-to-end Power BI project that analyses an FMCG company's sales, distributor delivery performance (OTIF) and inventory fill rate across four dashboard pages. Built on an ERP-style export with a multi-fact star schema, DAX measures, a custom theme, and a Python forecast visual.

> **Data note:** *(Edit before publishing: state whether the data is synthetic/anonymised. Do not publish real customer or outlet names.)*

---

## Dashboard preview

| Executive Overview | Sales & Territory |
|---|---|
| ![Executive Overview](screenshots/01_executive_overview.png) | ![Sales & Territory](screenshots/02_sales_territory.png) |

| Distributor Performance | Inventory & Fill Rate |
|---|---|
| ![Distributor Performance](screenshots/03_distributor_performance.png) | ![Inventory & Fill Rate](screenshots/04_inventory_fill_rate.png) |

**Data model**

![Data model](screenshots/05_data_model.png)

---

## Business questions

1. Are we hitting the NSV target, and where are we falling short?
2. Which distributors deliver On Time and In Full (OTIF), and which lag?
3. Which zones, reps and cities drive sales and call productivity?
4. Where is supply lost, and what is associated with stock-outs?

## Key findings

| Area | Finding |
|---|---|
| Sales | NSV of 14.25M against a 16.65M target (85.6% achieved) |
| Channels | GT contributes 55.5% of NSV, MT 28.0%, BWI 16.5% |
| Delivery | OTIF is 67.6% against an 85% target; In Full 85.4%, On Time 78.9% |
| Distributors | OTIF ranges from 61.2% (Rajshahi Trading House) to 71.5% (Rangpur Distribution Co) |
| Field force | Strike rate 78.4% (target 80%) on 1,658 calls; 291 of 300 outlets billed |
| Inventory | Fill rate 93.1% (target 95%), stock-out rate 3.3%, 24 days of inventory |
| Stock-outs | All 605 stock-out days occurred on days with no stock receipt, yet only 3.4% of no-receipt days ended in a stock-out. Missing receipts are necessary but not sufficient. Unusually high-demand days were about 18x more likely to end in a stock-out |

## Dashboard pages

| Page | What it shows |
|---|---|
| **Executive Overview** | NSV, orders, gross margin, average order value, billed outlets; NSV vs target, MoM growth, target achievement, top SKUs, zone and channel mix, and a 3-month NSV forecast with anomaly flags |
| **Sales & Territory** | Regional NSV map, sales-rep growth vs target scatter, zone-wise scorecard with strike rate colouring |
| **Distributor Performance** | OTIF, In Full, On Time, credit days, monthly OTIF trend against target, distributor scorecard with conditional formatting |
| **Inventory & Fill Rate** | Fill rate, stock-out %, closing stock, days of inventory; decomposition tree of fill-rate loss; key influencers for stock-out days |

## Technical highlights

- **Data model:** star schema with shared dimensions (`Calendar`, `dim_outlet_route`, `dim_rep`, `dim_product`) and several fact tables (`fact_calls`, `fact_invoice`, `fact_sale_order`, `fact_sale_line`, `fact_stock_daily`, `fact_rep_target`, `otif_log`). Facts relate only through dimensions, which avoids ambiguous filter paths.
- **Power Query:** `otif_log` was enriched with the outlet key by merging on the order number, so OTIF responds to distributor, zone and channel slicers.
- **DAX:** OTIF, In Full and On Time %, target variance text and colour measures, dynamic header subtitle that follows slicers, strike rate and fill rate status colours.
- **Design:** custom theme JSON (navy and gold), reusable header banner, page navigator with a selected state, "Clear all slicers" button.
- **Advanced visuals:** decomposition tree, key influencers (on a text `Stock Status` column), Python forecast (Holt linear trend, 3 months) with anomaly detection at 1.28 standard deviations.

## Repository structure

```
.
├── FMCG_OTIF_Dashboard.pbix
├── FMCG_Professional_Theme.json
├── README.md
├── dax_measures.md
├── python/
│   └── nsv_forecast.py
├── docs/
│   └── data_dictionary.md
└── screenshots/
    ├── 01_executive_overview.png
    ├── 02_sales_territory.png
    ├── 03_distributor_performance.png
    ├── 04_inventory_fill_rate.png
    └── 05_data_model.png
```

## How to run

1. Install **Power BI Desktop** (latest version).
2. Open `FMCG_OTIF_Dashboard.pbix`.
3. The report reads an Excel file. In **Transform data → Data source settings**, point the source to your local copy of the data file.
4. For the Python forecast visual, install Python and run:
   ```
   pip install pandas matplotlib statsmodels
   ```
   Then set the Python home directory in **File → Options → Python scripting**.
5. Apply the theme with **View → Themes → Browse for themes** and choose `FMCG_Professional_Theme.json`.

## Limitations

- The forecast is a simple Holt linear trend on about 10 months of history. Treat it as indicative, not a planning-grade forecast.
- Channel, distributor and zone slicers do not filter stock data, because the stock table is linked only to product and date. The inventory page therefore uses month, category and product slicers.
- Key influencers results on stock-outs reflect association in this dataset, not proven cause.

## Tools

Power BI Desktop · DAX · Power Query (M) · Python (pandas, matplotlib, statsmodels) · Excel

## Author

**Your Name**
[LinkedIn](https://www.linkedin.com/) · [GitHub](https://github.com/)
