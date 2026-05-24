<img width="852" height="235" alt="image" src="https://github.com/user-attachments/assets/6eee560d-9e9f-4f51-8324-b9bb3b6b31fa" />

# 📊 SSIS Project – AdventureWorksDW2022 (Merge & Analysis)

## 📌 Overview

This project demonstrates an **ETL process using SSIS** with the **AdventureWorksDW2022** data warehouse.

The goal is to:

* Extract data from dimension and fact tables
* Perform sorting and merging
* Load the transformed data into a destination table
* Prepare data for reporting and analysis

---

## 🗂️ Database Used

* **AdventureWorksDW2022**
* Sample Microsoft Data Warehouse

---

## 📁 Tables Used

### 🔹 DimCustomer (Dimension Table)

Contains customer details:

* CustomerKey
* FirstName
* LastName
* Gender
* GeographyKey

---

### 🔹 FactInternetSales (Fact Table)

Contains sales transactions:

* SalesOrderNumber
* CustomerKey
* ProductKey
* OrderDate
* SalesAmount

---

## 🔗 Relationship

```sql
DimCustomer.CustomerKey = FactInternetSales.CustomerKey
```

---

## 🔄 SSIS Data Flow Design

```id="flow1"
OLE DB Source (DimCustomer)
           ↓
Sort (CustomerKey)
           ↓
        Merge Join ← Sort (CustomerKey)
           ↑
OLE DB Source (FactInternetSales)
           ↓
OLE DB Destination (Final Table)
```

---

## ⚙️ Transformations Used

### 🔹 1. OLE DB Source

* Extracts data from:

  * DimCustomer
  * FactInternetSales

---

### 🔹 2. Sort Transformation

* Sort both inputs on:

  * `CustomerKey`
* Required for Merge Join

---

### 🔹 3. Merge Join

* Join Type: INNER JOIN
* Join Column: `CustomerKey`

---

### 🔹 4. OLE DB Destination

* Loads merged data into a new table

---

## 🧪 Sample SQL Query

```sql id="sql1"
SELECT 
    c.FirstName,
    c.LastName,
    f.SalesOrderNumber,
    f.OrderDate,
    f.SalesAmount
FROM DimCustomer c
JOIN FactInternetSales f
ON c.CustomerKey = f.CustomerKey;
```

---

## ❌ Issues Faced & Fixes

### 1. Metadata / Lineage ID Error

**Error:**

```id="err1"
has lineage ID that was not previously used
```

**Cause:**

* Column structure changed
* SSIS cached old metadata

**Fix:**

* Delete and recreate component (Sort / Destination)
* Refresh mappings

---

### 2. Sort Transformation Error

**Error:**

```id="err2"
Sort failed validation (VS_NEEDSNEWMETADATA)
```

**Cause:**

* Upstream column change
* Broken metadata

**Fix:**

* Recreate Sort transformation

---

### 3. Merge Join Issues

**Cause:**

* Inputs not sorted
* Different sort keys

**Fix:**

* Ensure both inputs:

  * Sorted on same column
  * Same data type

---

## 🧠 Key Learnings

* Importance of **Fact vs Dimension tables**
* SSIS is sensitive to **metadata changes**
* Sorting is mandatory for **Merge Join**
* Rebuilding components can fix hidden issues

---

## 🚀 Outcome

* Successfully merged customer and sales data
* Built a working ETL pipeline
* Prepared dataset for reporting

---

## 📌 Future Enhancements

* Add **DimProduct join**
* Create **SSRS reports**
* Build **SSAS cube**

---

## 💡 Author Notes

This project demonstrates:

* Real-world data warehouse usage
* Hands-on SSIS transformations
* Debugging common ETL errors

---
