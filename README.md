# Quantium – Chips Category Review

Work completed for the [Quantium Data Analytics Job Simulation](https://www.theforage.com/simulations/quantium/data-analytics-rqkb) on Forage (February 2026).

Quantium's retail analytics team was supporting the Category Manager for chips at a supermarket client. Two questions: who buys chips and how, and whether a new store layout trialled in three stores should be rolled out. The datasets provided by Forage are for simulation use only and are not included here.

**Notebook:** [`chips_category_review.ipynb`](chips_category_review.ipynb)

## Data

- `QVI_transaction_data.xlsx`: 264,836 transaction lines over July 2018 to June 2019, with date, store, loyalty card, product, quantity and sales value.
- `QVI_purchase_behaviour.csv`: 72,637 loyalty cards with life stage and premium status (Budget, Mainstream, Premium).

## Approach

**1. Data preparation and checks.** Converted Excel serial dates, checked nulls, duplicates and the date range (one missing day, 25 December, confirmed as a store closure rather than a data gap). Found and removed nine salsa products that were not chips, a loyalty card buying 200 packs at a time that looked like a commercial buyer, and one duplicate line, with a cleaning log reconciling every row removed. Extracted pack size and brand from product names and merged brand spelling variants.

**2. Customer segments.** Built a segment summary by life stage and premium status: sales, share, customers, transactions and packs per customer, spend per customer and price per pack. Separated whether a segment's spend comes from more buyers, more visits, bigger baskets or higher prices. Tested whether Mainstream singles and couples pay more per pack with a Welch t-test, and used an affinity index to show which brands and pack sizes the key segment over-indexes on. Checked price per 100g across pack sizes.

**3. Trial store assessment.** Built store-month metrics, limited the pool to stores with a full year of data, and ranked candidate control stores for each trial store (77, 86, 88) on a blend of pre-trial correlation and magnitude distance for sales and customer counts. Scaled each control to the trial store's pre-trial level, measured the percentage difference in each trial month, and tested it against the pre-trial standard deviation (t-test, one-sided 95%). Repeated the test on customer numbers and decomposed any uplift into customers, visit frequency, basket size and price.

**4. Findings and recommendations** written for the Category Manager.

## Results

- Chip sales ($1.81m from 71,287 customers) are spread widely; no segment exceeds 9% of the total. The largest by sales is Older Families (Budget); the largest by buyers is Young Singles/Couples (Mainstream).
- Families buy about nine packs per customer, roughly double Young Singles/Couples, so their spend is driven by basket size.
- Mainstream singles and couples pay $4.11 per pack against $3.67 for Budget and Premium (Welch t = 37.9, p < 0.001, `t.test`). They over-index on Kettle, Doritos, Pringles and Tyrrells and on 270g to 380g packs, which are also the best value per gram.
- Trial results against scaled controls: store 77 +28% sales (significant in March and April, driven by about 25% more customers), store 88 +14% (significant in March and April), store 86 +12% (significant in March only). Recommendation: roll the layout out selectively, starting with stores resembling 77 and 88, and keep measuring 86.

## Tools

R (tidyverse, readxl, ggplot2, scales)
