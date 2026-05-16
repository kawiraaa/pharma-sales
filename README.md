# Better Health Pharmacy Sales Analysis

## executive summary

This project analyzes Q4 2025 sales performance for Better Health Pharmacy using SQL, Python (Pandas), and Looker Studio.

The analysis focused on sales trends, branch performance, product demand, and customer payment behavior. After cleaning the data, SQL was used to generate insights, and Looker Studio was used for visualization.

Key findings show that the pharmacy generated over **1.1M KES** in revenue, with diabetes medication as the top revenue driver. Fridays recorded the highest sales, Wednesdays had the highest transaction volume, Nairobi CBD was the best-performing branch, and M-Pesa was the most used payment method.

Recommendations include maintaining stock for high-demand products, optimizing staffing during peak days, improving lower-performing branches, and encouraging impulse purchases through better product placement.

The insights support better decisions in inventory management, staffing, and overall sales strategy.
### dashboard overview
![](dashboard_1.png)
![](dashboard_2.png)

--- 

## problem statement

Better Health Pharmacy is a chain of pharmacies operating in Nairobi County that provides pharmaceutical products and healthcare supplies to the community.

At the end of Q4 2025, the management requires a clear sales report to evaluate the pharmacy’s performance. However, the sales data has not been properly organized or analyzed, making it difficult to identify sales trends and product performance.

The objective of this project is to analyze the pharmacy’s Q4 2025 sales data and generate meaningful insights about sales performance during the quarter.

The analysis will help stakeholders understand sales trends and support better decision-making in inventory management and sales planning.


---
## dataset
## Dataset Description

The dataset used in this project was synthetically generated to mimic real-world pharmacy sales transactions. The data was created for analytical purposes and does not contain real customer or business information.

It contains sales transaction records from Better Health Pharmacy for Q4 2025. Each row in the dataset represents a single sales transaction made at one of the pharmacy branches. 
The original dataset contains 6697 rows and 12 columns
## Features in the Dataset

| Feature | Description |
|---|---|
| Transaction_ID | A unique identifier assigned to each sales transaction. |
| Date | The date when the transaction was made. |
| Day_of_Week | The day on which the transaction occurred (e.g., Monday, Tuesday). |
| Branch_ID | A unique identifier for each pharmacy branch. |
| Branch_Location | The physical location of the pharmacy branch where the sale occurred. |
| Product_ID | A unique identifier assigned to each product or drug. |
| Drug_Name | The name of the pharmaceutical product sold. |
| Category | The classification of the product, such as antibiotics, painkillers, or vitamins. |
| Quantity | The number of units purchased in the transaction. |
| Unit_Price_KES | The price of a single unit of the product in Kenyan Shillings (KES). |
| Total_Sale_KES | The total sales amount generated from the transaction in Kenyan Shillings (KES). |
| Payment_Method| The method used by the customer to make payment, such as cash, card, or mobile money. |

---
## Tools Used

| Tool | Purpose |
|---|---|
| Python (Pandas) | Data cleaning and preprocessing |
| SQL | Sales analysis and business querying |
| Looker Studio | Data visualization and dashboard creation |

--- 
## Skills Demonstrated

- Data Cleaning and Preprocessing
- Exploratory Data Analysis (EDA)
- SQL Querying
- Business Intelligence Reporting
- Data Visualization
- Dashboard Development
- Business Insight Generation
- Data Storytelling

---

## Project Workflow

1. Data Collection
2. Data Cleaning and Preprocessing
3. Exploratory Data Analysis
4. SQL-Based Business Analysis
5. Dashboard Development in Looker Studio
6. Insight Generation
7. Business Recommendations
---

# Data Cleaning Process (Using Pandas)

## 1. Initial Dataset Inspection

The dataset was first inspected to understand its structure and quality.

- Total rows:6,698
- Total columns: 12

This initial step helped identify potential issues such as duplicate records, missing values, incorrect data types, and inconsistent categorical entries.

---

# 2. Duplicate Records

Duplicate rows were checked using the Pandas duplicate detection function.

```python
df.duplicated().sum()
```

- Number of duplicate rows found:152

These duplicate records were removed to ensure each observation in the dataset was unique.

```python
df = df.drop_duplicates()
```

Removing duplicates reduced the risk of biased analysis caused by repeated transactions.

---

# 3. Missing Values Analysis

A check for missing values revealed that the dataset contained **1,002 missing values** distributed across multiple columns.

```python
df.isnull().sum()
```

Instead of applying a single global method, **each column was handled individually** based on its data type and the nature of the data.

---

# 4. Data Type Corrections

Some columns had incorrect data types. For example:

- The **Date column** was originally stored as a string (`object`) and was converted to a proper datetime format.

```python
df['date'] = pd.to_datetime(df['date'])
```

Correct data types are important for accurate analysis and enable time-based operations such as grouping or filtering by date.

---

# 5. Cleaning Text Columns

Categorical text columns (e.g., **day of the week**, **payment method**) were checked for inconsistencies.

To identify inconsistencies, unique values were examined:

```python
df['column_name'].unique()
```

Issues such as spelling variations or formatting differences were found (for example: `"m pesa"` vs `"m-pesa"`).

To standardize the text data:

```python
df['column_name'] = df['column_name'].str.lower().str.strip()
```

This step:

- Converted text to **lowercase**
- Removed **extra spaces**
- Improved consistency across categorical values

---

# 6. Handling Missing Values in Categorical Columns

For categorical columns, missing values were replaced with the **most frequent value (mode)** in the column.

```python
df['column_name'].fillna(df['column_name'].mode()[0], inplace=True)
```

This method preserves the distribution of the data without introducing unrealistic values.

---

# 7. Handling Missing Values in Unit Price

For the **Unit Price** column, missing values were filled by comparing prices of the same drugs.

The prices of similar drugs were analyzed and used to estimate reasonable values for the missing entries.

This approach ensured the filled values remained **consistent with existing drug pricing patterns** in the dataset.

---

# 8. Handling Missing Values in Quantity

For the **Quantity** column, missing values were replaced using the **mean quantity for the respective drug**.

This was done to ensure that imputed values reflected typical purchase quantities.

Example approach:

```python
df['quantity'] = df.groupby('drug')['quantity'].transform(lambda x: x.fillna(x.mean()))
```

Using the mean within each drug group helps maintain realistic transaction patterns.

---

# 9. Final Outcome

After completing the cleaning process:

- Duplicate rows were removed.
- Missing values were handled appropriately depending on the column.
- Data types were corrected.
- Text inconsistencies were standardized.

These steps improved the **accuracy, consistency, and reliability** of the dataset, making it suitable for further analysis and visualization

## analysis using sql
The following SQL queries were used to analyze the sales performance of Better Health Pharmacy for Q4 2025. These queries focus on key business metrics such as total sales, product performance, branch performance, and customer behavior.

---

## 1. Overall Performance (Revenue, Quantity, Transactions)
**insight**:The pharmacy maintained strong health throughout Q4 with over 1.1M KES in revenue, high transaction volume (6,440 transactions), and strong product movement (13,030 units sold)
###  a) total sales 

```sql
SELECT SUM(Total_Sale_KES)
FROM pharma;
```

### results
![](total_sales.png)


### b).Total Quantity Sold

```sql
SELECT SUM(Quantity)
FROM pharma;
```
### results
![](sum_quantity.png)

### c). Total Number of Transactions

```sql
SELECT COUNT(DISTINCT Transaction_ID)
FROM pharma;
```
### results
![](count_transcations.png)

---
## 2. Sales Trends(monthly and weekly)
Insight: Revenue is incredibly stable across October, November, and December, showing no major seasonal dips. Fridays generated the highest revenue, while Wednesdays recorded the highest customer traffic based on transaction volume.

### a). Sales Trend Over Time

```sql
SELECT MONTHNAME(Date) AS month_name,
SUM(Total_Sale_KES)
FROM pharma
GROUP BY month_name
ORDER BY month_name DESC;
```
### results
![](month.png)


### b). Sales by Day of the Week

```sql
SELECT Day_of_Week,
SUM(Total_Sale_KES) AS total_sales,
COUNT(DISTINCT Transaction_ID) AS transaction_count
FROM pharma
GROUP BY Day_of_Week;
```
### results
![](day_of_week.png)

---

## 3. Branch Performance Analysis
Insight: Nairobi CBD is the primary revenue driver (631,413 ksh), though Embakasi maintains a very healthy transaction volume. This indicates a consistent customer base in both locations, with higher-value baskets likely occurring in the CBD.

```sql
SELECT Branch_Location AS branch,
SUM(Total_Sale_KES) AS total_sales,
COUNT(DISTINCT Transaction_ID) AS transaction_count
FROM pharma
GROUP BY branch;
```
### results
![](branches.png)

---
## 4. Product & Category Performance
Insight: Chronic care medication, particularly diabetes treatment products, contributed the largest share of total revenue.

### a). Product Performance 

```sql
SELECT Drug_Name,
SUM(Total_Sale_KES) AS total_sales,
SUM(Quantity) AS total_quantity
FROM pharma
GROUP BY Drug_Name
ORDER BY total_sales DESC;
```
### results
![](drugs.png)

### b). Category Performance

```sql
SELECT Category,
SUM(Total_Sale_KES) AS total_sales
FROM pharma
GROUP BY Category
ORDER BY total_sales DESC;
```
### results
![](category.png)

---

## 5. Payment Method Analysis
- M-Pesa accounted for approximately 65% of total sales revenue, making it the dominant payment method among customers.
```sql
SELECT Payment_Method,
SUM(Total_Sale_KES) AS total_sales
FROM pharma
GROUP BY Payment_Method;
```
### results
![](payments.png)

---
## Key Insights

- Total Q4 revenue exceeded 1.1M KES.
- Diabetes medication generated the highest revenue among all product categories.
- Fridays recorded the highest sales revenue.
- Wednesdays had the highest transaction volume.
- Nairobi CBD branch was the top-performing branch.
- M-Pesa was the most frequently used payment method.
- Antibiotics recorded the highest quantity sold.

---
## Interactive Performance Dashboard
To see these trends dynamically, I built an interactive data dashboard in Looker Studio. This allows stakeholders to filter the data by branch location and track daily cash flow patterns instantly.

**[Click Here to View the Live Interactive Dashboard](https://datastudio.google.com/reporting/a618b763-e295-4551-9522-9f3943a51b36)**

---
## business recommendations

### **2. Prioritize Diabetes Stock**
- Maintain consistent stock availability for insulin and other diabetes medications, as these products contribute a significant portion of total revenue.

### **3. Bulk-Buy Fast Movers**
- Buy **Azithromycin** in larger bulks from suppliers. It is the most popular physical item, so lowering its cost instantly boosts profit margins.

### **4. Optimize Staffing & Stock Delivery**
- Put extra staff on **Wednesdays** to handle the heavy customer crowds and ensure high-value stock is fully replenished before the high-revenue **Friday** rush.

### **5. Grow the Embakasi Branch**
- Launch a small customer loyalty program or local promotion in Embakasi to capture more market share and help it close the minor revenue gap with the CBD branch.

### **6. Encourage Impulse Buying**
- Place inexpensive, high-margin items (like vitamins, painkillers, or band-aids) right at the counter to encourage quick add-on purchases from the  walk-in customers.



