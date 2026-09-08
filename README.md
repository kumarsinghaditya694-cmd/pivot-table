# 📊 Pivot Table - Data Analysis Project

## 🎯 Project Overview

This project is a **complete guide to data analysis using the Pandas library**. You'll learn:
- **Data Cleaning** (Handling missing values)
- **Data Merging** (Combining multiple DataFrames)
- **Data Grouping** (Grouping similar data)
- **Pivot Tables** (Summarizing and analyzing data)

Perfect for beginners who want to master data manipulation!

---

## 📁 Project Structure

```
pivot-table/
├── pivot table.ipynb    # Main Jupyter notebook with all code
└── README.md            # This file (Documentation)
```

---

## 🚀 Quick Start

### Prerequisites

You need Python 3.7+ and these libraries:

```bash
pip install pandas numpy
```

### How to Run

1. **Open Jupyter Notebook:**
   ```bash
   jupyter notebook
   ```

2. **Open the `pivot table.ipynb` file**

3. **Run each cell** (Press `Ctrl + Enter` for each cell)

---

## 📚 Complete Code Breakdown

### **Step 1: Import Required Libraries**

```python
import numpy as np
import pandas as pd
```

- **NumPy**: For mathematical calculations and working with arrays
- **Pandas**: For data manipulation and analysis

---

### **Step 2: Create Sample Data - Patient Information**

#### Patients DataFrame

```python
patients = pd.DataFrame({
    "Patient_ID": [1, 2, 3, 4, 5, 6],
    "Name": ["Rahul", "Anita", "Suresh", "Pooja", "Vikas", "Meena"],
    "Doctor_ID": [101, 102, 101, 103, 104, 102],
    "Age": [25, 34, np.nan, 45, 29, np.nan]  # np.nan = Missing value
})
```

**What it does:** Stores patient information (ID, Name, Doctor assignment, Age)
- Some age values are missing (np.nan)
- Each patient is linked to a doctor via Doctor_ID

#### Doctors DataFrame

```python
doctors = pd.DataFrame({
    "Doctor_ID": [101, 102, 103, 104],
    "Doctor": ["Dr. Sharma", "Dr. Mehta", "Dr. Khan", "Dr. Singh"],
    "Department": ["Cardiology", "Neurology", "Orthopedic", "Dermatology"]
})
```

**What it does:** Stores doctor information and their specialization departments

---

### **Step 3: Handle Missing Values**

```python
# Check for missing values
pd.isnull(patients).sum()
# Output: 
# Patient_ID    0
# Name          0
# Doctor_ID     0
# Age           2

# Calculate average age
m = patients["Age"].mean()  # Result: 33.25

# Fill missing age values with the average
patients["Age"] = patients["Age"].fillna(m)
```

**What it does:**
1. Counts missing values (NaN) in each column
2. Calculates the mean age (ignoring NaN values)
3. Replaces missing age values with the average

---

### **Step 4: Combine Multiple DataFrames**

```python
# New patient data from follow-up
followup = pd.DataFrame({
    "Patient_ID": [7, 8],
    "Name": ["Arjun", "Kavita"],
    "Doctor_ID": [103, 104],
    "Age": [38, 31]
})

# Combine both DataFrames vertically
all_p = pd.concat([patients, followup])

# Reset index numbering (0, 1, 2, ... instead of duplicates)
all_patients = all_p.reset_index(drop=True)
```

**What it does:**
- `pd.concat()`: Stacks DataFrames one below the other (adds rows)
- `reset_index(drop=True)`: Renumbers rows from 0 onwards

---

### **Step 5: Merge DataFrames (SQL JOIN-like operation)**

```python
finaldf = pd.merge(all_p, doctors)
```

**Result:**
```
Patient_ID    Name    Doctor_ID    Age    Doctor        Department
1             Rahul   101          25     Dr. Sharma    Cardiology
2             Anita   102          34     Dr. Mehta     Neurology
3             Suresh  101          33.25  Dr. Sharma    Cardiology
4             Pooja   103          45     Dr. Khan      Orthopedic
5             Vikas   104          29     Dr. Singh     Dermatology
6             Meena   102          33.25  Dr. Mehta     Neurology
7             Arjun   103          38     Dr. Khan      Orthopedic
8             Kavita  104          31     Dr. Singh     Dermatology
```

**What it does:**
- Combines `patients` and `doctors` DataFrames
- Matches rows where `Doctor_ID` is the same
- Similar to SQL's INNER JOIN

---

### **Step 6: Group and Analyze Data (Pivot-like Operations)**

#### Count patients per doctor:

```python
finaldf.groupby("Doctor")["Patient_ID"].count()
# Output:
# Doctor
# Dr. Khan      2
# Dr. Mehta     2
# Dr. Sharma    2
# Dr. Singh     2
```

#### Count patients per department:

```python
dep = finaldf.groupby("Department")["Patient_ID"].count()
# Output:
# Department
# Cardiology     2
# Dermatology    2
# Neurology      2
# Orthopedic     2
```

#### Find the department with most patients:

```python
dep.idxmax()
# Output: 'Cardiology'
```

**What it does:**
- `groupby()`: Groups data by a column
- `count()`: Counts non-null values in each group
- `idxmax()`: Returns the group with the maximum value

---

### **Step 7: Create Actual Pivot Table (Sales Data)**

```python
data = {
    'Date': pd.date_range('2026-09-03', periods=20),
    'Product': ['A', 'B', 'C', 'D', 'A', 'B', 'C', 'D', ...],
    'Region': ['East', 'West', 'North', 'South', 'East', 'West', ...],
    'Sales': [196, 417, 216, 650, 327, 497, 593, 723, ...],
    'Units': [19, 98, 57, 60, 13, 28, 21, 33, ...],
    'Rep': ['John', 'Mary', 'Bob', 'Alice', 'John', 'Mary', ...]
}

df = pd.DataFrame(data)
```

**What it does:** Creates a sales dataset with dates, products, regions, sales amounts, and sales representative names

---

## 🔑 Key Concepts Explained

### **1. NaN (Missing Values)**
```python
np.nan  # Represents missing or undefined data
```
- Used when a value is unknown or not available
- Mathematical operations ignore NaN by default

### **2. DataFrame Merge (JOIN Operations)**
```python
pd.merge(df1, df2)    # Combines two DataFrames based on common columns
pd.concat([df1, df2]) # Stacks DataFrames vertically (adds rows)
```

### **3. GroupBy (The Foundation of Pivot Tables)**
```python
df.groupby("Column").agg_function()

# Common aggregation functions:
.sum()    # Total sum
.mean()   # Average value
.count()  # Count of items
.max()    # Maximum value
.min()    # Minimum value
```

### **4. Reset Index**
```python
df.reset_index(drop=True)
# Resets row numbering to 0, 1, 2, 3...
# drop=True removes the old index
```

---

## 💡 Practical Examples

### Example 1: Total Sales by Region

```python
sales_by_region = df.groupby("Region")["Sales"].sum()
# Result:
# Region
# East      3318
# North     1849
# South     3457
# West      2872
```

### Example 2: Average Units Sold per Product

```python
units_by_product = df.groupby("Product")["Units"].mean()
# Result:
# Product
# A    49.5
# B    64.5
# C    45.3
# D    51.2
```

### Example 3: Sales by Region and Product (2D Pivot)

```python
pivot = df.pivot_table(
    values='Sales', 
    index='Region', 
    columns='Product', 
    aggfunc='sum'
)
# Creates a table with regions as rows and products as columns
```

---

## 🎓 Learning Path

Follow this order to master the concepts:

1. ✅ **Basics** - Import libraries and create DataFrames
2. ✅ **Cleaning** - Handle missing values
3. ✅ **Merging** - Combine data from multiple sources
4. ✅ **Grouping** - Group and summarize data
5. ✅ **Pivot Tables** - Create advanced pivot tables
6. ⏭️ **Visualization** - Create charts with matplotlib/seaborn

---

## 🛠️ Useful Commands & Tips

| Command | What it does |
|---------|-------------|
| `df.head()` | Show first 5 rows |
| `df.tail()` | Show last 5 rows |
| `df.info()` | Show data types and missing values |
| `df.describe()` | Show statistical summary (mean, min, max, etc.) |
| `df.shape` | Show number of rows and columns |
| `df.columns` | Show column names |
| `df.dtypes` | Show data type of each column |
| `df.isnull().sum()` | Count missing values per column |

---

## 🚨 Common Errors & How to Fix Them

| Error | Cause | Solution |
|-------|-------|----------|
| `KeyError: 'column_name'` | Column name doesn't exist | Use `df.columns` to check available columns |
| `ValueError: cannot merge` | No common column to merge on | Ensure both DataFrames have a common column |
| `TypeError: unsupported operand type` | Operating on incompatible data types | Convert columns using `.astype()` |
| `NaN in result` | Operations include missing values | Use `fillna()` or `dropna()` |

---

## 📊 What You'll Learn

After completing this project, you'll be able to:

✅ Import and create DataFrames
✅ Handle missing data
✅ Merge multiple data sources
✅ Group and analyze data
✅ Create pivot tables
✅ Extract meaningful insights from data

---

## 🎯 Next Steps

1. **Visualization**: Learn matplotlib or seaborn to create charts
2. **Advanced Pivot**: Explore multi-level pivot tables
3. **Real Data**: Work with CSV files and real-world datasets
4. **Automation**: Write functions to automate analysis
5. **Performance**: Learn about large datasets and optimization

---

## 📖 Resources

- **Official Pandas Documentation**: https://pandas.pydata.org/
- **NumPy Guide**: https://numpy.org/
- **Python Data Analysis**: https://www.w3schools.com/python/pandas/

---

## 💻 System Requirements

- Python 3.7 or higher
- pip (Python package manager)
- Jupyter Notebook (optional but recommended)

---

## 🤝 Contributing

Feel free to modify this project and add your own examples!

---

## 📄 License

This project is open source and available for educational purposes.

---

## ✉️ Questions or Issues?

If you're stuck:

1. **Read the code comments** in the notebook
2. **Run each cell separately** and check outputs
3. **Use `df.head()`** to see data at each step
4. **Print variable types** with `print(type(variable))`

**Good luck with your data analysis journey! 🚀**
