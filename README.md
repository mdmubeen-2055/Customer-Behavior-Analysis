import pandas as pd
import numpy as np

# Load the dataset
df = pd.read_csv('data/customer_shopping_behavior.csv')

# Handling Missing Values: Review Rating has 37 missing values
# We use the median to maintain distribution integrity
df['Review Rating'] = df['Review Rating'].fillna(df['Review Rating'].median())

# Feature Engineering: Categorizing Age into meaningful business segments
bins = [18, 30, 45, 60, 100]
labels = ['Young Adult', 'Middle-aged', 'Adult', 'Senior']
df['Age_Group'] = pd.cut(df['Age'], bins=bins, labels=labels, right=False)

# Export cleaned data for SQL processing
df.to_csv('data/cleaned_customer_data.csv', index=False)
print("Data cleaning complete. Cleaned file saved.")

/* Q1: Revenue by Subscription Status 
Helps determine if the subscription model is driving higher ticket sizes.
*/
SELECT 
    subscription_status,
    COUNT(customer_id) AS total_customers,
    ROUND(AVG(purchase_amount), 2) AS avg_spend,
    ROUND(SUM(purchase_amount), 2) AS total_revenue
FROM customer
GROUP BY subscription_status;

/* Q2: High-Value Customer Segmentation
Identifying 'Loyal' customers vs 'New' customers.
*/
SELECT 
    CASE 
        WHEN previous_purchases = 1 THEN 'New'
        WHEN previous_purchases BETWEEN 2 AND 10 THEN 'Returning'
        ELSE 'Loyal'
    END AS customer_segment,
    COUNT(*) AS total_count,
    ROUND(AVG(purchase_amount), 2) AS avg_segment_spend
FROM customer
GROUP BY 1;
# Customer Shopping Behavior Analysis 🛒

## 📋 Executive Summary
This project analyzes 3,900 customer transactions to optimize marketing strategies. By combining **Python** for ETL, **SQL** for deep-dive analysis, and **Power BI** for visualization, I uncovered that:
* **Subscribers** spend 15% more on average than non-subscribers.
* **Clothing** is the highest revenue-generating category.
* **Young Adults** are the most frequent shoppers but have lower average ratings.

## 🚀 How to Use
1. **Python**: Run the notebook in `/notebooks` to clean the raw CSV.
2. **SQL**: Import the cleaned CSV into your SQL tool and run scripts in `/scripts`.
3. **Power BI**: Open the `.pbix` file to explore the interactive visual report.

## 📜 License
This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
