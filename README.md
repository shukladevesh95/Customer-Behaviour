# 🛍️ Customer Shopping Behavior Analysis

An end-to-end data analytics project that takes a raw retail customer dataset through **Python (cleaning & feature engineering) → MySQL (storage & querying) → Power BI (interactive dashboard)**.

The goal is to understand *who* the customers are, *what* they buy, and *how* factors like age, gender, subscription status, and shipping choice relate to spending and satisfaction.

---

## 📂 Repository Structure

```
Customer_Behaviour/
├── customer_shopping_behavior.csv          # Raw dataset (3,900 rows, 18 columns)
├── Python + SQL + BI(Project_1).ipynb      # Cleaning, feature engineering, MySQL load
├── Customer_behaviour.dashboard.pbix       # Power BI dashboard
└── README.md
```

---

## 📊 Dataset

**3,900 customer transactions** with 18 attributes:

| Group | Columns |
|---|---|
| Customer | `Customer ID`, `Age`, `Gender`, `Location` (50 US states) |
| Product | `Item Purchased` (25 items), `Category` (Clothing, Footwear, Outerwear, Accessories), `Size`, `Color`, `Season` |
| Purchase | `Purchase Amount (USD)`, `Payment Method`, `Shipping Type`, `Discount Applied`, `Promo Code Used` |
| Behaviour | `Review Rating`, `Subscription Status`, `Previous Purchases`, `Frequency of Purchases` |

---

## 🛠️ Tools & Technologies

- **Python** – `pandas` for cleaning and feature engineering
- **MySQL** – storage and SQL querying (via `SQLAlchemy` + `PyMySQL`)
- **Power BI** – interactive dashboard
- **Jupyter Notebook** – analysis workflow

---

## 🔄 Project Workflow

### 1. Data Exploration (Python)
- Inspected structure with `df.head()`, `df.info()`, `df.describe()`
- Checked for missing values and data types

### 2. Data Cleaning
- **Missing values:** `Review Rating` had **37 nulls**, imputed with the **median rating of each product category** (more robust than a global mean and respects category differences).
- **Column standardisation:** converted all column names to `snake_case` and renamed `purchase_amount_(usd)` → `purchase_amount`.
- **Redundant column removed:** `promo_code_used` was identical to `discount_applied` in every row (verified programmatically), so it was dropped.

### 3. Feature Engineering
- **`age_group`** – customers split into four equal-sized quartile bins (`Young Adult`, `Adult`, `Middle-aged`, `Senior`) using `pd.qcut`.
- **`purchase_frequency_days`** – converted text frequency (Weekly, Fortnightly, Monthly, Quarterly, Annually, etc.) into a numeric number of days between purchases, making it easier to analyse and aggregate.

### 4. Loading into MySQL
The cleaned DataFrame is written to a MySQL database (`customer_behaviour`, table `customer`) using SQLAlchemy, enabling SQL-based analysis and serving as the source for Power BI.

### 5. Power BI Dashboard
An interactive one-page dashboard containing:

- **KPI cards:** Number of Customers, Average Purchase Amount, Average Review Rating
- **Subscription status** breakdown (donut chart)
- **Revenue and customer count by Category** (column charts)
- **Revenue and customer count by Age Group** (bar charts)
- **Slicers:** Category, Shipping Type, Gender, Subscription Status

---

## 🔍 Quick Facts from the Data

- Average purchase amount ≈ **$59.76**; average review rating ≈ **3.75 / 5**
- Customer ages range from **18 to 70** (mean ≈ 44)
- **Clothing** is the largest category (1,737 of 3,900 purchases)
- **68%** of customers are male (2,652 of 3,900), and male customers account for roughly twice the total revenue of female customers
- Subscribers (1,053) and non-subscribers (2,847) have almost identical average spend (~$59.5 vs ~$59.9), so subscription does not appear to drive higher basket size on its own

> Explore the Power BI dashboard for the full interactive view and filtering.

---

## 🚀 How to Run

### Prerequisites
- Python 3.8+
- MySQL Server (optional, only needed for the database step)
- Power BI Desktop (to open the `.pbix` file)

### Setup

```bash
# 1. Clone the repo
git clone https://github.com/<your-username>/<your-repo>.git
cd <your-repo>

# 2. Install dependencies
pip install pandas jupyter pymysql sqlalchemy

# 3. Launch the notebook
jupyter notebook "Python + SQL + BI(Project_1).ipynb"
```

### Before you run the notebook
1. **Update the CSV path.** The notebook currently reads from a local Windows path; change it to:
   ```python
   df = pd.read_csv("customer_shopping_behavior.csv")
   ```
2. **Create the MySQL database** and update credentials:
   ```sql
   CREATE DATABASE customer_behaviour;
   ```
   Use environment variables rather than hard-coding your username and password.
3. Open `Customer_behaviour.dashboard.pbix` in Power BI Desktop and point the data source to your MySQL instance (or the CSV) if needed.

---

## 💡 Key Skills Demonstrated

- Data cleaning and missing-value imputation
- Feature engineering with `pandas`
- Python-to-SQL integration
- Dashboard design and storytelling in Power BI

---

## 🔮 Possible Next Steps

- Add SQL queries to the repo (e.g. revenue by category, top customers, discount impact)
- Analyse the effect of discounts and shipping type on spend and ratings
- Build customer segments (RFM / clustering)
- Add dashboard screenshots to this README

---

*Dataset: customer shopping behaviour data used for learning/portfolio purposes.*
