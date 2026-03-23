# 🛒 Amazon Sales Data Analysis — Business Performance Case Study

> A complete end-to-end data analysis project using Python, Pandas, Seaborn, and SQLite to uncover actionable business insights from Amazon India product data.

---

## 📌 Project Overview

This project analyzes **1,465 Amazon product listings** across multiple categories to understand how pricing, discounts, and product characteristics influence customer engagement and purchasing behavior.

The goal is to transform raw transactional data into **actionable business recommendations** — covering sales strategy, discount optimization, category focus, and inventory planning.

---

## 📊 Dataset Summary

| Attribute | Details |
|-----------|---------|
| **Source** | [Amazon Product Dataset — Kaggle](https://www.kaggle.com/datasets/karkavelrajaj/amazon-sales-dataset) |
| **Rows** | 1,465 product records |
| **Columns** | 16 features |
| **Unique Products** | 1,351 |
| **Categories** | 9 main categories |
| **Price Range** | ₹39 – ₹77,990 |
| **Avg Discount** | 47.7% |
| **Avg Rating** | 4.1 / 5 |
| **Total Reviews** | 26.7 million |

### Columns
| Column | Description |
|--------|-------------|
| `product_id` | Unique product identifier |
| `product_name` | Full product name |
| `category` | Hierarchical category path |
| `discounted_price` | Price after discount (₹) |
| `actual_price` | Original price (₹) |
| `discount_percentage` | Discount offered (%) |
| `rating` | Average customer rating (1–5) |
| `rating_count` | Number of customer reviews |
| `about_product` | Product description |
| `review_title` / `review_content` | Customer review text |
| `user_id` / `user_name` | Reviewer identifiers |
| `img_link` / `product_link` | Product URLs |

---

## 🎯 Business Questions Answered

- Which **product categories** drive the most customer engagement?
- How do **price segments** affect review volume and satisfaction?
- Do **heavy discounts** correlate with lower product quality?
- Which products are **high-risk** (high discount + low rating)?
- What are the **top 10 most reviewed** products on the platform?

---

## 🔧 Project Workflow

```
Raw Data → Cleaning → Feature Engineering → EDA → Visualization → SQL → Business Insights
```

### 1. 📥 Data Loading & Exploration
- Loaded CSV with 1,465 rows and 16 columns
- Identified data types, nulls, duplicates

### 2. 🧹 Data Cleaning
- Stripped `₹` and `,` from price columns → converted to `float`
- Removed `%` from discount column → converted to `float`
- Cleaned `rating_count` commas → converted to `int`
- Filled missing ratings with **median** (not 0 — preserves scale integrity)
- Extracted **top-level category** from `|`-delimited category string
- Dropped 7 unused columns (`img_link`, `product_link`, `review_content`, etc.)

### 3. ⚙️ Feature Engineering
| New Feature | Description |
|-------------|-------------|
| `discount_amount` | Actual ₹ savings (actual − discounted) |
| `high_discount` | Flag: 1 if discount ≥ 40%, else 0 |
| `price_range` | Bucketed price tiers (0–500, 500–1000, …, 10000+) |
| `rating_buckets` | Rating groups: <2, 2–3, 3–4, 4–5 |
| `net_saving_percent` | True savings as % of original price |

### 4. 📈 Exploratory Data Analysis
- Category-level performance (review volume, avg rating, avg discount)
- Product-level ranking by reviews, rating, and discount
- Price-range segmentation analysis
- Bad quality product detection (high discount + low rating)

### 5. 📉 Visualizations
- Scatter plot: Price vs Rating
- Bar charts: Products, Reviews, and Avg Rating by Price Range
- FacetGrid histograms: Discount distribution per price range
- Heatmap: Customer engagement by Category × Price Range

### 6. 🗄️ SQL Analysis (SQLite)
Designed a normalized relational schema and ran business queries:

```sql
-- Products table
CREATE TABLE products (
  product_id TEXT PRIMARY KEY,
  product_name TEXT,
  category TEXT,
  discounted_price REAL,
  actual_price REAL,
  discount_percentage REAL
);

-- Reviews table
CREATE TABLE reviews (
  review_id TEXT PRIMARY KEY,
  product_id TEXT,
  rating REAL,
  rating_count INTEGER,
  FOREIGN KEY (product_id) REFERENCES products(product_id)
);
```

**SQL Queries Run:**
- Top categories by total review volume
- Best-rated categories (minimum review threshold)
- Top 10 most reviewed products
- Top 10 highest discounted products
- Bad quality products: discount ≥ 70% AND rating ≤ 3.5

---

## 💡 Key Findings

- **Electronics and Computers & Accessories** dominate with 80%+ of total reviews
- **Budget products (₹0–500)** generate the highest transaction volume
- **Premium products (₹5,000+)** have the highest average ratings despite fewer reviews
- **Home Improvement** has the highest average discount (57.5%)
- Products with **90%+ discounts** still maintain 4.0+ ratings — discounting is a competitive strategy, not a quality cover-up
- **17 products** combine ≥70% discount with ≤3.5 rating — flagged as high-risk listings

---

## 📋 Business Recommendations

| Recommendation | Rationale |
|----------------|-----------|
| 🎯 Focus inventory on ₹0–1,500 range | Highest volume and engagement |
| 📉 Use discounts strategically | Avoid heavy discounts on poorly rated products |
| ⭐ Promote Office Products & Toys | Highest avg ratings — strong satisfaction signals |
| 💎 Market premium items on quality, not price | They perform well without heavy discounts |
| ⚠️ Review flagged bad-quality products | High discount + low rating = brand risk |
| 🗄️ Scale with the SQLite schema | Ready for dashboards, reporting, and ML pipelines |

---

## 🛠️ Tech Stack

![Python](https://img.shields.io/badge/Python-3.10-blue?style=flat-square&logo=python)
![Pandas](https://img.shields.io/badge/Pandas-2.x-150458?style=flat-square&logo=pandas)
![Seaborn](https://img.shields.io/badge/Seaborn-0.13-4C72B0?style=flat-square)
![Matplotlib](https://img.shields.io/badge/Matplotlib-3.x-orange?style=flat-square)
![SQLite](https://img.shields.io/badge/SQLite-3-003B57?style=flat-square&logo=sqlite)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-F37626?style=flat-square&logo=jupyter)

---

## 🚀 Getting Started

### 1. Clone the repository
```bash
git clone https://github.com/Jeetup-co/Amazon_sales_analysis.git
cd Amazon_sales_analysis
```

### 2. Install dependencies
```bash
pip install pandas numpy matplotlib seaborn
```

### 3. Run the notebook
```bash
jupyter notebook amazon_sales_business_analysis.ipynb
```
> Or open directly in [Google Colab](https://colab.research.google.com/)

---

## 📁 Repository Structure

```
Amazon_sales_analysis/
│
├── amazon.csv                              # Raw dataset (1,465 records)
├── amazon_sales_business_analysis.ipynb   # Main analysis notebook
└── README.md                              # Project documentation
```

---

## 👤 Author

**Jeetup-co**
- GitHub: [@Jeetup-co](https://github.com/Jeetup-co)

---

## 📄 License

This project is open source and available under the [MIT License](LICENSE).

---

*Dataset source: [Amazon Sales Dataset on Kaggle](https://www.kaggle.com/datasets/karkavelrajaj/amazon-sales-dataset)*
