# 🛍️ H&M Deadstock Detection & Markdown Strategy

**A retail inventory analytics project using transactional data to identify unsold stock, optimize markdown decisions, and inform inventory actions.**

---

## 📌 Table of Contents  
- [📖 Project Overview](#-project-overview)  
- [🧠 Problem Statement](#-problem-statement)  
- [📂 Dataset](#-dataset)  
- [⚙️ Tech Stack](#️-tech-stack)  
- [🔍 Key Features](#-key-features)  
- [📊 Analysis & Results](#-analysis--results)  
- [📈 Visual Insights](#-visual-insights)  
- [📋 Inventory Decision Table](#-inventory-decision-table)  
- [🧾 Sample Output](#-sample-output)  
- [📌 Conclusion](#-conclusion)  
- [🚀 How to Run](#-how-to-run)  

---

## 📖 Project Overview  
This project analyzes transactional data from H&M to **detect deadstock** (products that are not selling) and **simulate markdown strategies**. By leveraging simple yet effective time-series analysis, this project aims to reduce inventory waste, improve sell-through, and generate actionable insights.

---

## 🧠 Problem Statement  
In fast fashion retail, **deadstock** (unsold or stagnant inventory) leads to huge losses. Retailers like H&M must frequently discount or remove slow-selling SKUs. But:

- How do we define deadstock from historical sales?
- When should a product be marked down, held, or discarded?
- Can we use past performance to inform future pricing or clearance decisions?

---

## 📂 Dataset  
From the **[H&M Personalized Fashion Recommendations](https://www.kaggle.com/competitions/h-and-m-personalized-fashion-recommendations)** competition on Kaggle:

- `transactions_train.csv`: 31.7M transaction logs with article_id, customer_id, price, and timestamp.
- `articles.csv`: Metadata about 105k+ products including product type, season, etc.

🔹 For performance, we use **a filtered 200k subset** of transactions (approx. 1 year).

---

## ⚙️ Tech Stack  
- **Python** (Pandas, NumPy, Matplotlib, Seaborn)  
- **Jupyter / Colab Notebooks**  
- **DataFrame operations**, **rolling averages**, and **custom classification logic**  
- *(Optional Tableau/PowerBI dashboard support)*

---

## 🔍 Key Features  

✅ Preprocessing:  
- Cleaned and merged `transactions` and `articles` data.  
- Extracted `year`, `week`, and `month` from transaction dates.  

✅ Deadstock Detection:  
- Used a **2-week moving average** of weekly sales.  
- Flagged SKUs with `avg < 5 units` sold as **deadstock**.  

✅ Weeks Since Last Sale:  
- Calculated how many weeks have passed since the last sale for each SKU.

✅ Markdown Recommendation:  
- SKUs not sold for **8+ weeks** and flagged as deadstock are marked for markdown.

✅ Action Plan Generation:
- Based on sales and aging logic, SKUs were tagged with actions:
  - **Continue Normal Sale**
  - **Hold & Monitor**
  - **Heavy Markdown**
  - **Discard / Donate**

---

## 📊 Analysis & Results
📈 DATA SUMMARY

1. Total SKUs analyzed: 65,698
2. Deadstock SKUs (low sales): 54,379
3. SKUs with no sales in 8+ weeks: 27,874
4. SKUs recommended for markdown: 27,874

## 📈 Visual Insights  

- 📉 **Weekly Sales Trends**: For markdown SKUs  
- 📊 **Top Deadstock Product Types**:  
Sweater, Trousers, Dress, T-shirt, Blouse, Top, Bra, Jacket, Underwear bottom, Shirt


- 🧭 **Heatmap of Weekly Sales Activity** (pivoted `article_id` × `week`)  
- *(Optional Forecasting using ARIMA or Prophet — not included to avoid installing new libraries)*  

---

## 📋 Inventory Decision Table  

| is_deadstock | weeks_since_sold | Action              |
|--------------|------------------|---------------------|
| True         | > 8              | Discard / Donate    |
| True         | 4 – 8            | Heavy Markdown      |
| True         | < 4              | Hold & Monitor      |
| False        | —                | Continue Normal Sale|

**🔽 Action Breakdown:**
📋 Inventory Action Decision Breakdown:

- Discard / Donate: 27,874
- Hold & Monitor: 20,165
- Heavy Markdown: 6,340
- Continue Normal Sale: 11,319


---

## 🧾 Sample Output  

### ➡️ Action: Discard / Donate  

| article_id | 2_week_avg | weeks_since_sold | action             |
|------------|------------|------------------|--------------------|
| 111565003  | 0.0        | 25               | Discard / Donate   |
| 112679052  | 0.0        | 31               | Discard / Donate   |
| 114428026  | 0.0        | 18               | Discard / Donate   |

### ➡️ Action: Hold & Monitor  

| article_id | 2_week_avg | weeks_since_sold | action             |
|------------|------------|------------------|--------------------|
| 110065002  | 1.5        | 0                | Hold & Monitor     |
| 114428030  | 2.0        | 0                | Hold & Monitor     |

### ➡️ Action: Continue Normal Sale  

| article_id | 2_week_avg | weeks_since_sold | action               |
|------------|------------|------------------|----------------------|
| 108775015  | 10.0       | 0                | Continue Normal Sale |
| 108775044  | 19.0       | 0                | Continue Normal Sale |

### ➡️ Action: Heavy Markdown  

| article_id | 2_week_avg | weeks_since_sold | action           |
|------------|------------|------------------|------------------|
| 108775051  | 0.0        | 5                | Heavy Markdown   |
| 112679048  | 0.0        | 5                | Heavy Markdown   |

---

## 📌 Conclusion  

This analysis presents a lightweight, practical approach for inventory teams to:

- Quickly detect deadstock SKUs.
- Simulate markdown decisions based on sales and aging.
- Strategically handle inventory by product type.

📌 *This is a production-ready pipeline for real-world markdown management without requiring deep ML or external tools.*

---

## 🚀 How to Run  

1. Clone the repository or open the Colab notebook.  
2. Upload your `kaggle.json` to fetch the data.  
3. Run the notebook step by step.  
4. Outputs are auto-exported as CSVs:
   - `sku_weekly_demand.csv`
   - `deadstock_details.csv`
   - `inventory_decision_table.csv`



