# Electric Vehicle Market Performance and Insights

Tableau capstone project by Vishwas Choyal, EN25CS3011066. Individual project for Data Analytics with Tableau.

The dashboard uses nine visualizations to examine vehicle segments, manufacturer performance, battery technology, manufacturing locations and customer satisfaction. Select a vehicle category in the first chart to filter the other charts. Clear the selection to return to the full dataset.

## Open the dashboard

Open **EV Market Insights.twbx** in Tableau Public or Tableau Desktop. This packaged workbook includes its data extract. The `.twb` file contains the editable workbook definition and uses the extract in `data/ev_market.hyper`.

- [Live Tableau dashboard](https://public.tableau.com/views/EVMarketInsights_17934697727980/EVMarketInsights)
- [Three-point scenario story](https://public.tableau.com/views/EVMarketInsights_17934697727980/ScenarioStory)
- [English narrated demo on Google Drive](https://drive.google.com/file/d/1XmrLik-fGaaCRTDb6RiIGywPqFDB0BBD/view?usp=sharing)
- [Project repository](https://github.com/vishwaschoyal/ev-market-insights-tableau)

The demo runs for 6 minutes 35 seconds and uses synthesized English narration over recorded Tableau views and a live category-filter demonstration.

## Data and scope

Source: [Electric Vehicle Market Classification Dataset by tharishreddy22 on Kaggle](https://www.kaggle.com/datasets/tharishreddy22/electric-vehicle-market-classification-dataset). The publisher lists an MIT license.

The publisher identifies the dataset as procedurally generated synthetic data. All manufacturers and models are fictional. Findings describe this learning dataset and must not be treated as real market shares, investment recommendations or engineering evidence.

Both source splits are combined for descriptive analysis: 200,000 training rows and 100,000 test rows. No prediction model is trained, so combining the splits does not compromise a model evaluation. One exact duplicate is removed, leaving **299,999 records** with model years **2018–2026**, **15 categories**, **200 manufacturers** and **10 manufacturing countries**.

`model_year` is a model cohort, not a transaction date. A chart across model years cannot establish actual year-on-year sales growth. `annual_sales_units` is aggregated as reported in each source record. The 2,323,303,050-unit sum is a dataset aggregate across cohorts, not verified annual worldwide EV sales.

## Data preparation

1. Preserve the downloaded source splits and add split provenance.
2. Combine the two disjoint splits and trim text fields.
3. Remove one exact duplicate across the original 37 columns.
4. Keep missing measurements blank. Do not replace unknown sales or ratings with zero.
5. Check sales, price, range and capacity for negative values, and ratings for values outside 0–5. No invalid values were found in these checks.
6. Add a unique record key and build a Tableau Hyper extract.

The raw data contains 3,017 missing sales values and 5,908 missing customer ratings. `SUM` and `AVG` omit those nulls. The dashboard shows observed-data summaries, rather than imputing unobserved results. See `data/data_quality.json` for the complete audit.

## Scenario coverage

| Scenario | Questions addressed | Views |
|---|---|---|
| Emily Johnson, market strategy director | Which segments contribute sales? How do satisfaction, range and battery mix differ? Which model cohorts show different segment compositions? | 1, 2, 3, 5, 7, 9 |
| David Chen, business intelligence manager | Which manufacturers lead? How do reported sales shares, ratings, battery mix and manufacturing countries compare? | 4, 5, 6, with ratings and sales share in tooltips |
| Dr Sarah Williams, sustainable mobility researcher | How do battery cycle life, capacity and range compare? Where are vehicles manufactured? How do categories differ in range and satisfaction? | 3, 5, 6, 7, 8 |

## Nine visualizations

1. **Sales by vehicle category** — horizontal bars, `SUM(annual_sales_units)` by category. This is the category filter for linked charts.
2. **Sales by model year** — line chart across model cohorts. Use it for cohort comparison, not a time-series growth claim.
3. **Customer satisfaction by category** — average source-record ratings out of five. These are not weighted by customer sample sizes, which are unavailable.
4. **Top 10 manufacturers by reported sales** — bars for the ten manufacturers ranked on the full dataset. Tooltips include average ratings and each manufacturer's share of total dataset sales. The top-ten membership remains fixed under category selections.
5. **Battery technology sales mix** — reported sales by battery chemistry. This measures sales-weighted adoption within the dataset.
6. **Sales by manufacturing country** — sales associated with the manufacturing country. This is not factory output or a vehicle's sales destination.
7. **Driving range by category** — average range in kilometres. Vehicle types have different purposes and cannot be ranked fairly using range alone.
8. **Battery life by technology** — average battery cycle life by battery type, with average range and capacity in tooltips. This compares synthetic technology characteristics. Range and capacity can be affected by segment mix.
9. **Category sales across model years** — heatmap comparing category and model-cohort combinations. Use the shared color scale to identify large and small cells.

## Calculations

```text
Dataset records = SUM([Record Count])
Record Count = 1
Reported sales = SUM([annual_sales_units])
Mean rating = AVG([customer_rating])
Mean range = AVG([range_km])
Share of dataset sales =
  SUM([annual_sales_units]) /
  MIN({ FIXED : SUM([annual_sales_units]) })
```

The fixed denominator makes manufacturer shares comparable with the complete dataset. When a category filter is selected, the numerator changes while that overall denominator remains fixed. The result is then the selected category's contribution to overall dataset sales, not a recomputed within-category market share.

## Validation

The workbook was opened in Tableau Public 2026.3. The category filter was tested by selecting Hatchback: the model-year trend, battery mix and manufacturer chart updated. Clearing the selection restored the full dataset. The Hatchback tooltip showed 231,033,059 reported units across 25,713 records. The complete dataset total was reconciled independently in Python.

## Project files

The final project report is supplied in Word and PDF. `Completed Templates.zip` contains the completed phase Word files and PDFs, including the linked canvases. `All Phase Templates.pdf` combines the phase PDFs for convenient review. Original template branding and attribution are retained. The Snowflake alternatives are marked not applicable because this project implements the Tableau track.

The portable TWBX embeds the data required to open the dashboard. The large clean CSV and standalone Hyper extract are supplied with the local project output; use the TWBX when downloading from GitHub. Source dataset links and validation summaries accompany the workbook. Course progress and mentor approval are separate portal outcomes and are not implied by the dashboard publication.
