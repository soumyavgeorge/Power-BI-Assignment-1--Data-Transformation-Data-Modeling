# Power BI Assignment 1 – Data Transformation & Data Modeling 
📊 E-Commerce Sales Data Preparation & Data Modeling in Power BI

> This project covers the complete data preparation and data modeling workflow for an e-commerce sales dataset in Microsoft Power BI Desktop. Using Power Query Editor, the raw order, order-detail and sales-target tables were imported, cleaned, transformed, merged, filtered, sorted and aggregated into clean summary views, and then linked together in Model View with proper relationships. The result is a well-structured, analysis-ready data model that can be used directly for reporting and dashboard building.

---

## 📂 Dataset Description
The assignment uses three CSV files:

| File | Description |
| :--- | :--- |
| `List of Orders.csv` | Order-level details: `Order ID`, `Order Date`, `CustomerName`, `State`, `City` |
| `Order Details.csv` | Line-item details: `Order ID`, `Amount`, `Profit`, `Quantity`, `Category`, `Sub-Category` |
| `Sales target.csv` | Monthly sales targets by `Month of Order Date` and `Category` |

Tables created during the project:

| Table | Description |
| :--- | :--- |
| `Order Data` | `List of Orders` merged with `Order Details`, sorted and filtered for Tamil Nadu |
| `Order Details -2` | Summary of `Order Details` by `Category` and `Sub-Category` |
| `Sales target - 2` | Summary of `Sales target` by month |

---

## 🛠️ Step-by-Step Workflow

### 1. Import Data
* Imported **`List of Orders.csv`** into Power BI.
* Opened `List of Orders` in **Power Query Editor** by clicking **Transform**.
* Imported **`Order Details.csv`** and **`Sales target.csv`** into Power Query Editor.
* Used **Promoted Headers** on each table so that the first row became the column names.

### 2. Data Transformation

**`List of Orders`**
* **Restricted to 500 rows:** `Home` → `Keep Rows` → `Keep Top Rows` → `500` → `OK`.
* **Order Date:** Set the data type to **Date**.
* **CustomerName:** Converted to proper case using `Transform` → `Format` → `Capitalize Each Word`.
* **Location:** Merged `City` and `State` with a comma separator to create the new column `Location` in the format *City, State* (e.g. `Chennai, Tamil Nadu`).

**`Order Details`**
* **Amount:** Changed the data type to **Fixed Decimal Number**.
* **Profit Margin:** Added a custom column that divides `Profit` by `Amount`, with the data type set to **Percentage** (e.g. `-90.04%`, `43.98%`).
* **Profit Status:** Added a conditional column based on the `Profit` column:
  * Profit < 0 → **Loss**
  * Profit = 0 → **Break-Even**
  * Profit > 0 → **Profit**

**`Sales target`**
* **Target:** Changed the data type to **Fixed Decimal Number**.
* **Month of Order Date:** Set to the **Date** data type.

### 3. Merging Data (Joins)
* Merged `List of Orders` and `Order Details` on the **`Order ID`** column (*Merged Queries*) and expanded the `Order Details` columns (*Expanded Order Details*) to create the single table **`Order Data`**.

### 4. Handling Missing & Duplicate Data
* **Missing values:** Checked all columns with Power Query's column profiling. Every column shows **100% Valid, 0% Error, 0% Empty**, so there were no missing values to fill. As a safeguard, **Remove Blank Rows** was still applied.
* **Duplicates:** Removed duplicate rows with `Home` → `Remove Rows` → `Remove Duplicates`.
* Both cleaning steps (*Removed Duplicates* and *Removed Blank Rows*) were applied to all tables: `List of Orders`, `Order Details`, `Sales target`, `Order Data`, `Order Details -2` and `Sales target - 2`.

### 5. Sorting & Filtering Data (`Order Data`)
* **Sorting:** Sorted the `Order Date` column in **descending order** (*Sorted Rows*) to analyse the most recent trends first.
* **Filtering:** Filtered the `State` column to **Tamil Nadu** (*Filtered Rows*) for regional analysis. The result has 25 rows, all with `Location` = `Chennai, Tamil Nadu`.

### 6. Grouping & Aggregating Data
To keep the original tables unchanged, the queries were duplicated and then grouped with **Group By**.

* **`Order Details -2`**
  * Duplicated the `Order Details` table.
  * Grouped by **`Category`** and **`Sub-Category`**, calculating:
    * **`Order Count`**: count of rows
    * **`Average Profit`**: average of `Profit`
    * **`Total Amount`**: sum of `Amount`
  * Reordered the columns: `Category` → `Sub-Category` → `Order Count` → `Average Profit` → `Total Amount`.
  * Result: 17 rows, one per sub-category.

* **`Sales target - 2`**
  * Duplicated the `Sales target` table.
  * Grouped by **`Month of Order Date`** and calculated **`Total Target`** as the sum of `Target`.
  * Result: 12 rows, April 2018 to March 2019.

### 7. Data Modeling & Relationships (`Model View`)
Loaded the tables using `Close & Apply` and created the relationships in **Model View**:

| From Table (Column) | To Table (Column) | Cardinality | Cross-Filter Direction | Status |
| :--- | :--- | :--- | :--- | :--- |
| `List of Orders` (`Order ID`) | `Order Details` (`Order ID`) | **One-to-Many (`1:*`)** | Single | Active |
| `Order Details` (`Category`) | `Sales target` (`Category`) | **Many-to-Many (`*:*`)** | Both | Active |

* Checked in **Manage Relationships** that both relationships are **Active**, so filters flow correctly between the tables.

---

## 💡 Key Concepts Applied
* **ETL & Data Transformation:** Importing CSV files, promoting headers, changing data types, text formatting, merging columns, custom and conditional columns, reordering columns.
* **Data Cleaning:** Restricting rows, profiling for missing values, removing blank rows and duplicates.
* **Merging Queries:** Joining `List of Orders` and `Order Details` into `Order Data` using `Order ID`.
* **Sorting & Filtering:** Descending date sort and state-level filter.
* **Grouping & Aggregation:** Group By with count, average and sum.
* **Data Modeling:** Dimension vs. fact tables, `1:*` and `*:*` cardinality, cross-filter direction.

---

## ✅ Conclusion
This project shows how raw transactional data can be turned into a reliable, analysis-ready model in Power BI. Importing and cleaning the three source tables, adding calculated columns such as `Location`, `Profit Margin` and `Profit Status`, and removing blank rows and duplicates gave consistent, trustworthy data. Merging orders with their line items, then sorting and filtering, produced a focused, most-recent-first view of Tamil Nadu orders, while the Group By summaries captured performance by sub-category and monthly targets without touching the source tables. With relationships set between `List of Orders`, `Order Details` and `Sales target`, the model is ready for visuals and dashboards, such as comparing actual sales and profit against monthly targets.


```
