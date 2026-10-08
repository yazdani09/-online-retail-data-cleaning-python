# Online Retail II — Data Cleaning and Preprocessing

## Project Overview

This project was completed as part of my Week 1 Data Analyst assignment on Data Acquisition, Cleaning, and Preprocessing.

The objective of this project was to acquire a publicly available retail dataset, understand its structure, identify data-quality issues, clean and preprocess the data using Python, and prepare an analysis-ready dataset.

## Dataset

**Dataset:** Online Retail II  
**Source:** UCI Machine Learning Repository

The dataset contains online retail transaction information such as invoice number, stock code, product description, quantity, invoice date, price, customer ID, and country.

## Tools Used

- Python
- Pandas
- NumPy
- Matplotlib
- Seaborn
- Jupyter Notebook
- VS Code

## Initial Dataset

The local dataset used for this project contained:

- 525,461 rows
- 8 columns

## Data Quality Problems Identified

During the initial investigation, I found several data-quality issues:

- 6,865 exact duplicate rows
- 2,928 missing Description values
- 107,927 missing Customer ID values initially
- 12,326 negative Quantity records
- 3 negative Price records
- 3,687 zero-price records
- 57,870 Quantity outliers using the IQR method
- 35,273 Price outliers using the IQR method
- 2,592 non-product transactions such as postage, manual entries, bank charges, adjustments, and Amazon fees

## Cleaning Decisions

### Duplicate Rows

I found 6,865 exact duplicate records. Since all columns were identical in these records, they were removed to avoid counting the same transaction more than once.

### Missing Description

There were 2,928 records with missing product descriptions. Investigation showed that these records were associated with zero-price transaction records and did not provide reliable product-level information.

These rows were removed.

### Missing Customer ID

A large number of transactions did not have Customer IDs. I did not remove these records because they still contained useful information such as product, quantity, price, date, and country.

These records were retained for analyses that do not require individual customer identification.

### Negative Prices

Three records had negative prices. They were identified as accounting-related `Adjust bad debt` records rather than normal product sales.

These three records were removed from the cleaned dataset.

### Zero Prices

There were 3,687 zero-price records initially. After removing rows with missing descriptions, 753 remained.

I investigated these records instead of automatically deleting them. Many contained descriptions such as `damaged`, `missing`, `smashed`, `given away`, `temp`, and `MIA`.

They were retained in the cleaned dataset but excluded from the final positive-sales dataset.

### Quantity Outliers

The IQR method identified 57,870 quantity outliers.

I did not remove all of them because some large quantities represented genuine bulk transactions. Therefore, they were investigated rather than treated automatically as errors.

### Price Outliers

The IQR method identified 35,273 price outliers.

Further investigation showed that many extreme values belonged to business-related transactions such as:

- POSTAGE
- DOTCOM POSTAGE
- Manual
- Bank Charges
- AMAZON FEE
- Adjustments

For example, some Manual transactions had very high values, including £25,111.09.

Therefore, price outliers were investigated using business context rather than simply being deleted based on an IQR threshold.

## Final Sales Dataset

After cleaning and preprocessing, I created a separate `sales_df` containing product sales suitable for analysis.

Conditions used:

- Quantity > 0
- Price > 0
- Valid Description
- Non-product transaction codes excluded

The final dataset contains:

- **502,630 sales transactions**
- **4,245 unique products**
- **4,286 known customers**
- **40 countries**
- **95.66% of the original records retained**

## Preprocessing

The following additional columns were created:

- `TotalAmount`
- `Year`
- `Month`
- `Day`
- `Hour`

`TotalAmount` was calculated using:

```python
sales_df["TotalAmount"] = sales_df["Quantity"] * sales_df["Price"]
The Customer_ID column was converted to a nullable integer type because it contains missing values.

Final Validation

The final sales dataset was checked for:

Duplicate rows
Negative quantities
Zero quantities
Negative prices
Zero prices
Negative transaction amounts
Missing values

The final validation showed:

Duplicate rows: 0
Negative quantities: 0
Zero quantities: 0
Negative prices: 0
Zero prices: 0
Negative TotalAmount: 0
Repository Structure
Dataset/
cleaned_data/
notebook/
report/
screenshots/
README.md
.gitignore
Key Learning

The main lesson I learned from this project is that data cleaning is not simply about deleting unusual values.

For example, the IQR method marked many Quantity and Price values as outliers, but further investigation showed that some of them were legitimate bulk sales or business transactions such as postage and fees.

Therefore, I used both statistical methods and business context when making cleaning decisions.

This helped me create a more reliable dataset for further analysis.

Author

Abu Yazdani

BCA Graduate | Aspiring Data Analyst

Skills practiced: Python, Pandas, NumPy, Matplotlib, Seaborn, Data Cleaning, Data Preprocessing, and Exploratory Data Analysis.