# pharma-sales

## Context

better Health Pharmacy is a chain of pharmacies operating within Nairobi County. The pharmacies provide a variety of pharmaceutical products and healthcare supplies to customers across the region, contributing to accessible healthcare services for the community.

---

## Problem Statement

At the end of the fourth quarter (Q4) of 2025, the owners of Good Health Pharmacy require a clear and structured sales report to evaluate the pharmacy’s sales performance during the quarter. However, the available sales data has not yet been organized or analyzed in a way that highlights key insights such as total sales, sales trends over time, and performance across different products or customers. As a result, stakeholders lack a comprehensive view of the pharmacy’s Q4 sales performance, making it difficult to accurately assess business outcomes and identify areas for improvement.

---

## Objective

To analyze the pharmacy’s Q4 2025 sales data and generate insights that summarize sales performance and trends during the quarter.

---

## Significance

This analysis will provide stakeholders with a clear understanding of the pharmacy’s sales performance in Q4, including sales trends, peak sales periods, and the distribution of sales across products or customer segments. The insights generated will support better decision-making in areas such as sales strategy, inventory management, and revenue planning for the next quarter.

---

# Data Cleaning Process (Using Pandas)

## 1. Initial Dataset Inspection

The dataset was first inspected to understand its structure and quality.

- **Total rows:** 6,698
- **Total columns:** 12

This initial step helped identify potential issues such as duplicate records, missing values, incorrect data types, and inconsistent categorical entries.

---

# 2. Duplicate Records

Duplicate rows were checked using the Pandas duplicate detection function.

```python
df.duplicated().sum()
```

- **Number of duplicate rows found:** 152

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
