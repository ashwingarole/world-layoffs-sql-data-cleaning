# World Layoffs Data Cleaning Project (MySQL)

## 📌 Project Overview
This project focuses on cleaning and standardizing a raw, unorganized dataset containing global tech layoff records using MySQL. The raw data contained duplicate entries, inconsistent naming conventions, structural nulls, and incorrectly formatted dates. 

The goal was to transform this raw CSV extract into a reliable, consistent, and analysis-ready database table.

---

## 🛠️ Data Cleaning Process & SQL Techniques

### 1. Creating Staging Tables
To protect the original raw records from unintended data loss, I created duplicate staging tables (`layoffs_staging` and `layoffs_staging2`) before executing any data modification queries.

### 2. Identifying and Removing Duplicates
* Used the `ROW_NUMBER()` window function partitioned across all key attributes (`company`, `location`, `industry`, `total_laid_off`, `percentage_laid_off`, `date`, `stage`, `country`, `funds_raised_millions`) to identify exact duplicate entries.
* Filtered where `row_num > 1` using a Common Table Expression (CTE) and deleted redundant rows.

### 3. Data Standardization
* **Whitespace Trimming:** Applied `TRIM()` on text fields like `company` to remove leading and trailing spaces.
* **Label Unification:** Identified variations in categorical fields (such as multiple variations of 'Crypto' or trailing punctuation in country names) and updated them to a unified naming standard.
* **Date Parsing:** Converted string-formatted dates into standard MySQL `DATE` formats (`YYYY-MM-DD`) using the `STR_TO_DATE()` function, followed by altering the column definition.

### 4. Handling Null and Blank Values
* Converted empty/blank strings (`''`) into formal database `NULL` values across categorical columns.
* Used **Self-Joins** to populate missing values: if a company had an empty `industry` in one row but a known industry in another row, the missing value was automatically populated.

### 5. Removing Unnecessary Rows and Columns
* Identified and removed records where both `total_laid_off` and `percentage_laid_off` were `NULL`, as these rows provided no usable metric data.
* Dropped the temporary helper column (`row_num`) used during the deduplication phase.

---

## 💻 Tech Stack
* **Database:** MySQL
* **Tool:** MySQL Workbench
* **Key SQL Concepts:** CTEs, Window Functions (`ROW_NUMBER()`), Self-Joins, String Manipulation (`TRIM`), Date Conversion (`STR_TO_DATE`), DDL (`ALTER TABLE`), DML (`UPDATE`, `DELETE`).

---

## 📁 Repository Structure
* `layoffs.csv`: Raw, uncleaned dataset.
* `data_cleaning_queries.sql`: Complete MySQL script containing every step of the cleaning process.
* `README.md`: Project documentation and workflow overview.