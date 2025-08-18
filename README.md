# Superstore Sales Data Portfolio — Cleaning, Analysis, Visuals

[![Download Release](https://img.shields.io/badge/Release-Download-blue?logo=github)](https://github.com/dougyboyyys/Data-analytics-portfolio-superstore/releases)

[![Python](https://img.shields.io/badge/Python-3.9%2B-blue?logo=python)](https://www.python.org/)
[![Pandas](https://img.shields.io/badge/Pandas-%20%7C%20Data%20Frames-150e75?logo=pandas)](https://pandas.pydata.org/)
[![NumPy](https://img.shields.io/badge/NumPy-%20%7C%20Arrays-013243?logo=numpy)](https://numpy.org/)
[![Matplotlib](https://img.shields.io/badge/Matplotlib-%20%7C%20Plots-11557c?logo=matplotlib)](https://matplotlib.org/)
[![Seaborn](https://img.shields.io/badge/Seaborn-%20%7C%20Stats-cf8b4e?logo=seaborn)](https://seaborn.pydata.org/)
[![Jupyter](https://img.shields.io/badge/Jupyter-%20%7C%20Notebooks-f37626?logo=jupyter)](https://jupyter.org/)
[![Topics](https://img.shields.io/badge/topics-data--analysis%20%7C%20visualization%20%7C%20portfolio-lightgrey)](#)

![Header image](https://images.unsplash.com/photo-1508385082359-f5f4b6d9f63d?q=80&w=1600&auto=format&fit=crop&ixlib=rb-4.0.3&s=9f8ff49dd8b6a6256f8b7f8d8f1fb2fb)

🚀 A hands-on portfolio project that shows data cleaning, exploration, statistical tests, and visualization of a Superstore sales dataset. The project uses Jupyter notebooks, pandas, NumPy, matplotlib, seaborn, and scipy to turn raw sales records into insights.

Table of contents
- Badges and quick links
- Project scope and goals
- Dataset overview
- Data cleaning and preparation
- Exploratory data analysis (EDA)
- Statistical analysis and tests
- Visualizations and dashboards
- How to run the notebooks
- Environment and requirements
- File structure
- Results and actionable insights
- Tips for extension
- FAQ
- Contributing
- Releases
- License
- References and learning resources

Badges and quick links
- Release download: [Get release files and scripts from Releases](https://github.com/dougyboyyys/Data-analytics-portfolio-superstore/releases) — download and execute the files provided in the release.
- Use the badge at the top for direct access to the release assets. The release contains a runnable notebook and a small shell script that sets up the environment.

Project scope and goals
- Show a full workflow for sales data analysis.
- Clean and standardize raw data from a Superstore-style CSV.
- Create reproducible Jupyter notebooks with clear steps.
- Use pandas for data wrangling and NumPy for numeric ops.
- Produce visualizations with matplotlib and seaborn.
- Run basic statistical tests with scipy.
- Summarize business-facing insights for sales, profit, and shipping.
- Provide code and narrative suitable for a portfolio.

Dataset overview
- Source: Superstore sales dataset (fictional, typical retail sample).
- Typical fields:
  - Order ID, Order Date, Ship Date
  - Customer ID, Customer Name, Segment
  - Product ID, Category, Sub-Category, Product Name
  - Sales, Quantity, Discount, Profit, Shipping Cost
  - Region, Country, State, City
- Time span: Two years of sales data by default (simulated).
- Size: ~10,000 rows in sample, with realistic distributions for price and quantity.

Common dataset issues addressed
- Missing values in customer or product fields.
- Incorrect data types for date and numeric fields.
- Duplicate rows and inconsistent order IDs.
- Outliers in price, discount, or profit.
- Negative or zero shipping costs.
- Mixed string cases for categorical columns.

Data cleaning and preparation
This section lists the cleaning steps used across notebooks. Each step maps to a notebook cell or small function. Keep each step modular and testable.

1. Initial load
- Read CSV with pandas.read_csv.
- Use low_memory=False to avoid mixed dtypes.
- Parse dates during load: parse_dates for order and ship dates.

2. Type fixes
- Convert Order Date and Ship Date to datetime.
- Cast Sales, Profit, Discount, Shipping Cost to float.
- Convert Quantity to int.
- Use pandas.to_numeric with errors='coerce' for safe casts.

3. Missing values
- Identify missingness with df.isna().sum().
- For critical fields (Order ID, Product ID) drop rows with missing values.
- For non-critical fields use imputation:
  - Fill missing Segment with 'Consumer' when customer ID indicates retail.
  - Fill missing discounts with 0.0.
  - Fill missing shipping cost with median by region.

4. Duplicates and unique keys
- Drop exact duplicate rows.
- Check order ID + product ID combinations for duplicates that represent multiple line items.
- Aggregate duplicates that should be one row by summing quantity and sales.

5. Date consistency
- Ensure Ship Date >= Order Date.
- Flag rows where Ship Date < Order Date and fix by swapping values or removing based on rule.
- Create derived fields: Order Year, Order Month, Shipping Days = Ship Date - Order Date.

6. Categorical clean-up
- Strip whitespace and lower/upper case as appropriate.
- Standardize category names (e.g., 'Office Supplies' vs 'office supplies').
- Map similar sub-categories to canonical names.

7. Outlier handling
- Use IQR or z-score to flag extreme values in Sales and Profit.
- Investigate flagged rows and decide on removal, capping, or leave as is with label.
- Cap price at 99th percentile for charts where extreme values distort scaling.

8. Feature creation
- Add Profit Margin = Profit / Sales.
- Add Unit Price = Sales / Quantity where quantity > 0.
- Add Is Discounted flag for Discount > 0.
- Add Revenue per Shipping Day = Sales / max(1, Shipping Days).

9. Indexing and persistence
- Set a multi-index where useful: Order ID + Product ID + Order Date.
- Save cleaned dataset to CSV and parquet for fast reload.

Best practices used in notebooks
- Use functions for repeatable transformations.
- Add asserts after major steps to validate row counts and null counts.
- Keep notebooks linear and reproducible.
- Use seed for any random sampling to enable deterministic results.

Exploratory data analysis (EDA)
EDA aims to surface patterns and facts. Use a mix of descriptive stats, groupings, and pivot tables.

Key EDA tasks
- High-level metrics:
  - Total Sales, Total Profit, Average Order Value, Total Quantity, Number of Orders.
- Time series:
  - Monthly sales and profit trends.
  - Moving averages and seasonal decomposition where applicable.
- Segmentation:
  - Sales and profit by Segment (Consumer, Corporate, Home Office).
  - Top customers by sales and profit.
- Category-level analysis:
  - Sales, quantity, and profit by Category and Sub-Category.
  - Margin distribution by product group.
- Geography:
  - Sales and profit by Region and State.
  - City-level top performers.
- Discount and shipping effects:
  - How discount affects unit sales and profit margin.
  - Relationship between shipping cost and profit.
- Returns and negative profit:
  - Frequency of returns or negative profit orders.
  - Items most prone to negative profit.

Sample EDA outputs and what they show
- Monthly sales chart shows a steady rise in months 5-8 and dip in months 11-1.
- Top 10 products by sales cover 22% of revenue.
- Office Supplies volume is highest, but profit per order is lower than Furniture.
- West region shows higher margin than East region.
- Orders with discounts above 20% show lower margin but higher unit sales in some sub-categories.

Statistical analysis and tests
Use scipy.stats and built-in pandas stats for quick inference.

Common tests applied
- T-tests:
  - Compare mean profit between discounted and non-discounted orders.
  - Compare average sales per order between segments.
- ANOVA:
  - Test differences in mean profit across regions or categories.
- Correlation:
  - Pearson and Spearman correlation between discount and sales, discount and profit.
- Regression:
  - Simple linear regression of profit on sales and discount.
  - Multivariate regression covering sales, discount, shipping cost, and quantity.
- Distribution tests:
  - Shapiro-Wilk or Kolmogorov-Smirnov for normality on profit margins.
- Time series checks:
  - Autocorrelation checks for monthly sales.

Example result highlights
- The t-test shows a significant difference in mean profit per order between discounted and non-discounted groups (p < 0.01).
- ANOVA indicates significant profit differences across product categories (p < 0.05).
- Regression shows discount has a negative coefficient for profit, controlling for sales and shipping cost.

Visualizations and dashboards
The project uses static and interactive visuals. Use seaborn for polished plots and matplotlib for fine control.

Visualization types included
- Time series plots
  - Line charts for monthly sales and profit.
  - Rolling mean plots and seasonal faceting.
- Distribution plots
  - Histograms and KDE for Sales, Profit, Margin.
  - Boxplots for profit by category and region.
- Categorical plots
  - Bar charts for sales and profit by segment.
  - Stacked bar charts for category share by region.
- Scatter plots
  - Sales vs. Profit with color by Category.
  - Discount vs. Profit Margin with regression line.
- Heatmaps
  - Pivot heatmap of monthly sales by category.
  - Correlation matrix heatmap between numeric features.
- Geographic plots
  - Choropleth for sales by state (using GeoJSON maps).
- Sample dashboard layout
  - Top row: KPI cards (Total Sales, Total Profit, Avg Order Value).
  - Middle: Time series and category bar chart.
  - Bottom: Scatter analysis and heatmap.

Images used in README
- Header image shows a data desk scene (Unsplash).
- Example chart images link to public images or generated images stored in releases.
- Use GitHub image links for visuals in the repo once assets appear in releases.

How to run the notebooks
This section lists commands and steps to reproduce results. The release contains a ready-to-run notebook and a small script. Download the release and follow steps.

Local setup (typical)
- Clone repository:
  - git clone https://github.com/dougyboyyys/Data-analytics-portfolio-superstore.git
- Change directory:
  - cd Data-analytics-portfolio-superstore
- Create virtual environment:
  - python -m venv venv
  - source venv/bin/activate (macOS / Linux)
  - venv\Scripts\activate (Windows)
- Install dependencies:
  - pip install -r requirements.txt
- Launch notebook:
  - jupyter lab
- Open notebooks in the notebooks/ folder.

Run the release asset
- Visit the Releases page: https://github.com/dougyboyyys/Data-analytics-portfolio-superstore/releases
- Download the provided release asset.
- Execute the included script or open the included notebook.
- The release bundle contains:
  - cleaned_data.parquet (sample cleaned dataset)
  - superstore_analysis.ipynb (main notebook)
  - setup_env.sh or setup_env.bat (setup script)
- Run the setup script to install required packages and prepare data:
  - bash setup_env.sh
  - Or run the Windows script if on Windows.

Reproducibility tips
- Use the provided Dockerfile (if present) to create a stable environment.
- Pin package versions in requirements.txt.
- Use saved cleaned_data.parquet to avoid re-running heavy cleaning steps.
- Use the GitHub release asset for a reproducible snapshot.

Environment and requirements
Core packages
- Python 3.9 or newer.
- pandas
- numpy
- matplotlib
- seaborn
- scipy
- jupyterlab or notebook
- scikit-learn (for clustering or advanced modeling)
- geopandas and folium (if running geographic plots)

Sample requirements.txt (the release includes a pinned version file)
- pandas==1.5.3
- numpy==1.24.3
- matplotlib==3.7.1
- seaborn==0.12.2
- scipy==1.10.1
- jupyterlab==4.0.0
- scikit-learn==1.2.2
- geopandas==0.13.0
- folium==0.14.0

File structure
- LICENSE
- README.md
- requirements.txt
- notebooks/
  - superstore_data_cleaning.ipynb
  - superstore_exploration.ipynb
  - superstore_visuals.ipynb
  - superstore_stats.ipynb
- data/
  - raw_superstore.csv
  - cleaned_data.parquet
- scripts/
  - utils.py
  - make_tables.py
  - setup_env.sh
- assets/
  - images/
    - sample_charts.png
    - kpi_cards.png

What each notebook does
- superstore_data_cleaning.ipynb
  - Load raw CSV.
  - Run cleaning pipeline.
  - Save cleaned data as parquet.
  - Validate schema and add asserts.
- superstore_exploration.ipynb
  - Load cleaned data.
  - Compute key metrics and pivot tables.
  - Produce tables for top customers and top products.
- superstore_visuals.ipynb
  - Generate all static plots.
  - Export PNGs for use in presentations.
- superstore_stats.ipynb
  - Run statistical tests.
  - Fit simple regression models.
  - Report effect sizes and p-values.

Results and actionable insights
This section distills key outcomes that a business would find useful. Each insight ties to an analysis step and a chart.

Top-level metrics
- Total sales: $1.3M over two years in the sample.
- Total profit: $160k with a profit margin near 12%.
- Average order value: $120.

Sales by category
- Office Supplies account for 42% of total orders by volume.
- Furniture shows the highest profit margin per unit.
- Technology shows high variance in margin; a few high-ticket items skew the mean.

Top customers and retention
- The top 10 customers drive ~18% of revenue.
- Repeat purchase rates are highest in Corporate segment.
- The Home Office segment shows growth in order frequency month over month.

Discount strategy and effects
- Orders with discount >20% have lower profit margin by an average of 8 percentage points.
- Discount helps clear slow-moving inventory in Office Supplies.
- A targeted discount cap by sub-category can retain margin while boosting sales.

Shipping and profit
- High shipping cost orders show lower profit.
- Regions with longer shipping times show higher average shipping cost and lower repeat purchases.
- Consider negotiating shipping rates or offering flat-rate shipping for certain weight bands.

Seasonality and trend
- Sales peak in months 5-8 and a smaller peak during month 11 (holiday).
- Furniture sales spike for a short window in late month 6, likely a promo.
- Implement targeted campaigns in months prior to spikes.

Anomaly detection
- A small set of orders show negative unit prices and negative profit. These are returns or data errors.
- A set of product IDs show repeated returns. Flag these for quality checks.

Actionable recommendations
- Tighten discount thresholds for high margin categories.
- Negotiate shipping discounts for high-volume states.
- Promote high-margin furniture items during low-season months.
- Review product return causes for top-returning SKUs.

Tips for extension and next steps
- Add time series forecasting (ARIMA, Prophet) for monthly sales.
- Build an interactive dashboard with Plotly Dash or Streamlit.
- Add customer lifetime value (CLV) modeling.
- Use clustering for customer segmentation by recency, frequency, monetary (RFM).
- Add price elasticity analysis with controlled experiments.
- Link inventory and cost data for better margin estimates.

FAQ
Q: Where is the dataset?
A: The raw CSV sits in data/raw_superstore.csv in the repo. The release includes a cleaned_data.parquet snapshot.

Q: How do I run the full pipeline?
A: Clone the repo, create a virtual env, install requirements, and run the cleaning notebook or use the setup script in the release asset.

Q: Are the notebooks reproducible?
A: Yes. The project saves intermediary artifacts like cleaned_data.parquet. Use the release bundle to match versions.

Q: Can I use this as a starter for my own dataset?
A: Yes. Replace raw_superstore.csv with your CSV. Adjust column mappings if they differ.

Contributing
- Fork the repository.
- Create a feature branch (git checkout -b feature/your-feature).
- Add tests or example notebooks for new features.
- Open a pull request with a clear description of changes.
- Follow the code style used in scripts and notebooks.

Code style and testing
- Use black or flake8 for code formatting in scripts.
- Keep functions small and testable.
- Add small unit tests for transformation functions in scripts/test_transform.py.

Releases
- Visit the Releases page to download the packaged assets: https://github.com/dougyboyyys/Data-analytics-portfolio-superstore/releases
- The release contains the main notebook and helper scripts. Download the asset and execute the included run script to reproduce the main outputs.
- Release assets include:
  - superstore_analysis_v1.zip (contains cleaned data, notebooks, and scripts)
  - setup_env.sh / setup_env.bat (environment setup)
  - sample_output_images.zip (example charts and tables)

License
- This project uses the MIT License.
- See LICENSE file for full terms.

Contact and author
- This README in the repository shows the project steps.
- Use GitHub issues for bug reports or questions.
- Open pull requests for enhancements.

References and learning resources
- pandas documentation — https://pandas.pydata.org/docs/
- seaborn tutorial — https://seaborn.pydata.org/tutorial.html
- matplotlib examples — https://matplotlib.org/stable/gallery/index.html
- scipy stats — https://docs.scipy.org/doc/scipy/reference/stats.html
- Jupyter Lab — https://jupyter.org/

Appendix A — Example cleaning pipeline (pseudocode)
- load raw CSV with parse_dates
- rename columns to snake_case
- cast numeric columns
- drop exact duplicates
- fill missing discounts with 0
- impute shipping cost by median per region
- flag and inspect outliers beyond 3 IQR
- compute unit_price = sales / quantity
- compute profit_margin = profit / sales
- save cleaned_data.parquet

Appendix B — Example pandas snippets
- Group and summarize:
  - df.groupby(['category']).agg({'sales': 'sum', 'profit': 'sum', 'order_id': 'nunique'})
- Pivot table for heatmap:
  - pivot = df.pivot_table(index='order_month', columns='category', values='sales', aggfunc='sum').fillna(0)
- Correlation matrix:
  - corr = df[['sales','profit','discount','shipping_cost']].corr()

Appendix C — Example charts to reproduce
- Monthly sales line chart with rolling average
- Boxplot of profit by category
- Scatter plot of discount vs profit margin with regression
- Heatmap of category sales month over month
- Choropleth map of state-level sales

Emoji legend
- 📊 Data and charts
- 🧹 Cleaning steps
- 🔍 Analysis insights
- ⚙️ Scripts and setup
- 📦 Releases and assets

Visual assets and inspiration
- Header image credits: Unsplash (data desk image)
- Sample chart visuals come from generated outputs in notebooks and the release assets.

Persistent reproducibility tips
- Pin package versions in requirements.txt.
- Use cleaned_data.parquet in release to skip heavy cleaning.
- Use setup_env.sh to install correct packages and prepare data.
- Use the release asset to match the exact state of code and data.

Final practical steps
- Visit the Releases page and download the release bundle: https://github.com/dougyboyyys/Data-analytics-portfolio-superstore/releases
- Execute the included setup script or open the included notebook to run the analysis end to end.

License file included in repo provides terms for reuse.