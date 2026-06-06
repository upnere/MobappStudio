# Google Play Store: A Market Analysis for My MobApp Studio

*An exploratory data science experiment to size the mobile app market and guide our next app decision.*

---

## 1. Context & Business Question

My MobApp Studio is allocating resources to build a new mobile app, launching first on the **Google Play Store**. Before committing, management asked for a market report answering:

- How big is the market - in number of downloads and in dollars?
- The same, broken down per category (in percentages)?
- For each category, the ratio of downloads per app?
- Any additional insight useful for the decision.

This post treats those questions as a **scientific experiment**: we state assumptions, run each analysis step explicitly, report the results, and conclude with next steps.

---

## 2. Hypothesis & Assumptions

**Main hypothesis:**
> {{State your testable hypothesis here. Example: "App downloads are highly concentrated in a few categories, and the categories with the most downloads are not the same as the categories with the highest prices — meaning the largest market is dominated by free apps."}}

**Assumptions we make about the data:**
- The dataset is a representative snapshot of the Google Play Store at the time of collection (2026-06-06).
- `Installs` is reported in bucketed thresholds (e.g. "1,000+"), so totals are approximate lower bounds, not exact figures.
- Revenue is approximated as `Price × Installs` for paid apps; in-app purchases and ads are **not** captured, so dollar figures understate real revenue.
- Duplicate app rows are data-collection artefacts and should be removed before aggregating.

---

## 3. Data & Methodology

**Dataset:** Google Play Store apps dataset (`googleplaystore.csv`) - 10841 raw rows, 13 columns.

**Tools:** Python, pandas, matplotlib.

**Cleaning steps** (implemented in `clean_dataset()`):
- Removed duplicate rows and duplicate apps.
- Converted `Installs` ("10,000+") and `Price` ("$4.99") to numeric.
- Converted `Reviews` ("3.0M") and `Size` ("19M") to numeric; "Varies with device" → missing.
- Dropped the known corrupted row.
- Filled missing `Rating` with the median; left `Size` missing values as-is (see note).

> **Note on imputation:** filling `Rating` with the median keeps rows without inventing implausible data, but it creates an artificial spike at the median and slightly lowers variance — we keep this in mind when interpreting the rating distribution.

After cleaning: **9659 apps** across **5 categories**.

---

## 4. Experiment & Results

Each subsection states what we measured, presents the results, and interprets them.

### 4.1 Market size - total downloads and dollars

- **Total downloads:** 75,322,526,427
- **Estimated total dollar value (paid apps only):** $291,140,168.79

*Interpretation:* <!-- TODO: 2–3 sentences. e.g. how dominant free apps are, what the paid market is worth. -->

### 4.2 Downloads and value per category (percentages)

Top 8 of installs per category:

| Category | Installs | % of total |
|----------|---------:|-----------:|
|GAME | 1.344792e+10 | 17.85 |
|COMMUNICATION | 1.103828e+10 | 14.65 |
|TOOLS | 8.102772e+09 | 10.76 |
|FAMILY | 6.237543e+09 | 8.28 |
|PRODUCTIVITY | 5.793091e+09 | 7.69 |
|SOCIAL | 5.487868e+09 | 7.29 |
|PHOTOGRAPHY | 4.658148e+09 | 6.18 |
|VIDEO_PLAYERS | 3.931903e+09 | 5.22 |

**Pie chart — installs per category:**

![Installs per category](images/installs_per_category_pie.png)
<!-- TODO: generate and add image -->

*Interpretation:* <!-- TODO -->

### 4.3 Downloads per app, by category (concentration)

Ratio = total installs in category ÷ number of apps in category. Apps with > 10,000,000

| Category | Apps | Installs/app |
|----------|-----:|-------------:|
|COMMUNICATION | 315 | 35042147.0|
|VIDEO_PLAYERS | 164 | 23975017.0|
|SOCIAL | 239 | 22961790.0|
|PHOTOGRAPHY | 281 | 16577038.0|
|PRODUCTIVITY | 374 | 15489549.0|
|GAME | 945 | 14230608.0|
|TRAVEL_AND_LOCAL | 219 | 13218663.0|
|ENTERTAINMENT | 86 | 11449535.0|
<!-- TODO -->

*Interpretation:* <!-- TODO: which categories are crowded vs. high-yield-per-app. This is the "is it worth competing here" signal. -->

### 4.4 Most popular paid apps in the Family category

**Bar chart:**

![Top paid Family apps](images/top_paid_family_bar.png)
<!-- TODO -->

*Interpretation:* <!-- TODO -->

### 4.5 Most popular genres among paid Family apps

**Pie chart (by number of installs):**

![Paid Family genres](images/paid_family_genres_pie.png)
<!-- TODO -->

*Interpretation:* <!-- TODO -->

### 4.6 Mean price per category

**Bar chart:**

![Mean price per category](images/mean_price_per_category_bar.png)
<!-- TODO -->

*Interpretation:* <!-- TODO -->

### 4.7 Most expensive app per category

App > $100

| Category | App | Price |
|----------|-----|------:|
|LIFESTYLE | I'm Rich - Trump Edition | 400.00|
|FINANCE | I Am Rich Premium | 399.99|
|FAMILY | I am Rich Plus | 399.99|
|MEDICAL | EP Cook Book | 200.00|
|PRODUCTIVITY | cronometra-br | 154.99|
|EVENTS | BP Fitness Lead Scanner | 109.99|
<!-- TODO -->

*Interpretation:* <!-- TODO -->

---

## 5. Conclusion

<!-- TODO: 1–2 paragraphs. Did the data support the hypothesis from Section 2? -->
- Was the hypothesis confirmed or rejected?
- The biggest market by downloads is {{...}}; the best opportunity by downloads-per-app is {{...}}.
- A concrete recommendation for the new app: which category and pricing model the data points toward, and why.

---

## 6. Limitations & Next Steps

**Limitations:**
- `Installs` are bucketed → totals are approximate.
- Revenue ignores in-app purchases and ads → paid-only dollar figures undercount.
- Snapshot in time → no trend information.

**What to experiment next:**
- Bring in `Reviews` and `Rating` to model what drives downloads (correlation/regression).
- Compare against an App Store dataset to validate cross-platform.
- Analyse release dates/update frequency vs installs to study app longevity.

---

## Appendix: Reproducing the analysis

The full notebook (`my_mobapp_studio.ipynb`) implements:
`load_dataset()`, `print_summarize_dataset()`, `clean_dataset()`, `print_histograms()`, `compute_correlations_matrix()`, `print_scatter_matrix()`, plus the aggregation cells that produce every chart above.

```bash
# place googleplaystore.csv next to the notebook, then run all cells
jupyter notebook my_mobapp_studio.ipynb
```
