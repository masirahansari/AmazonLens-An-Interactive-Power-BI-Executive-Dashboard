# Amazon India Sales Intelligence Dashboard
 
An interactive Power BI executive dashboard built on 1,351 Amazon India products. Raw, messy product data is cleaned with Python and turned into a one-page dashboard with KPI cards, slicers, dynamic insights and conditional formatting.
 
> Add a screenshot here: `![Dashboard](images/dashboard.png)`
 
## Highlights
 
- 1,465 raw rows cleaned down to 1,351 unique products
- 8 KPI cards, 3 slicers and 6 visuals on a single page
- Insight cards that rewrite themselves when slicers change
- Card colours that turn green or red depending on the current selection versus the overall data
## Key Findings
 
| Metric | Result |
|---|---|
| Average rating | 4.09 / 5 |
| Average discount | 46.69% |
| Median selling price | ₹899 |
| Total ratings | 24 million |
| Estimated revenue (proxy) | about ₹69 billion |
 
- Mobiles & Accessories contributes about 45% of estimated revenue.
- About 49% of products have a discount of 50% or more.
- Redmi leads brands by estimated revenue, ahead of Samsung and MI.
- Only about 7% of products are rated 4.5 or above.
## Tech Stack
 
- Python (pandas) for cleaning and feature engineering
- Power BI Desktop for modelling and visuals
- DAX for measures, calculated columns and conditional formatting
## Project Structure
 
```
.
├── data/
│   ├── amazon.csv              raw dataset
│   └── amazon_clean.csv        cleaned dataset used in Power BI
├── clean_data.py               cleaning script
├── Amazondata.pbix             Power BI report
├── Amazon_PowerBI_Project_Explanation.docx
└── README.md
```
 
## Data Cleaning
 
Run `clean_data.py` from the project root to regenerate the clean file.
 
1. Removed 114 duplicate `product_id` rows (same product listed in multiple categories)
2. Converted price, discount and rating count from text (`₹1,099`, `64%`, `24,269`) to numbers
3. Filled one invalid rating with the median and set two missing rating counts to 0
4. Split the category path into main, sub and leaf category and extracted the brand
5. Added `rupees_saved`, `est_revenue`, and price, discount and rating bands
## Key DAX Measures
 
```
Total Products = DISTINCTCOUNT(amazon[product_id])
Avg Rating = AVERAGE(amazon[rating])
Avg Discount % = AVERAGE(amazon[discount_pct]) / 100
Median Price = MEDIAN(amazon[discounted_price])
Est Revenue = SUMX(amazon, amazon[discounted_price] * amazon[rating_count])
Total Reviews = SUM(amazon[rating_count])
Total Savings = SUMX(amazon, amazon[rupees_saved] * amazon[rating_count])
Top Rated % = DIVIDE(CALCULATE([Total Products], amazon[rating] >= 4.5), [Total Products])
```
 
## How to Run
 
1. Install Power BI Desktop (Windows).
2. Open `Amazondata.pbix`.
3. If Power BI asks for the data source, point it to `data/amazon_clean.csv`.
If the downloaded file comes as a `.zip`, rename the extension to `.pbix` and open it.
 
## Limitations
 
- Estimated revenue is price × rating count, a proxy and not real sales.
- Product variants can share a rating count, which may overstate some revenue.
- The dataset has no date or region columns, so trends and maps are not included.
- Review text was not analysed.
## Future Work
 
- Pricing and discounts page with a discount vs rating scatter plot
- Brand performance page and page navigation
- Sentiment analysis on review text
- Publish to Power BI Service
