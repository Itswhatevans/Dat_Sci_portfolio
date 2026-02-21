---
title: "Blog Tutorial"
author: "Jordan Evans"
date: "2026-02-20"
categories: ["Data Science", "Quarto"]
---

## __Introduction:__
Have you ever been given a dataset and quickly found it to so messy you couldn't even analyzy it? We'll be examining how to use the Pandas package in Python to tackle and wrangle a dataset.

### Imagine you have this data here:
| StudentName | Major       |GradYear  |GPA   |Internship?  |HoursWorked |
|-------------|-------------|----------|------|-------------|------------|
| Alice Smith | statistics  | 2026     | 3.8  | Yes         | 12         |
| bob jones   | Data Sci    | 2025     | 3.45 | yes         | 15         |
| CAROL LEE   | STATISTICS  | 2026     | NaN  | No          | 8          |
| David Kim   | data science| 2024     | 3.92 | No          | NaN        |
| Eva Brown   | Stats       | 2025     | 3.6  | YES         | 20         |
- Notice the inconsistencies in the varying cell values. What can you see right away that could pose problems? 
### Here are the issues that need fixing:
- Inconsistent capitalization
- Column names that don't follow "snake_case"
- Mixed Boolean values (Yes, YES, yes)
- Missing values
- Abbreviated Categories
- A column we might remove later

## Pattern Overview:
1. Standardize column names
2. Fix capitalization inconsistencies
3. Convert data types
4. Remove unnecessary columns
5. Handle missing values
6. Add a clean index column

## __Hit 'em with this!__

### First we need some data:
```python
import pandas as pd

# Use pd.DataFrame()
df = pd.DataFrame({
    "StudentName": ["Alice Smith", "bob jones", "CAROL LEE", 
    "David Kim", "Eva Brown"],
    "Major": ["statistics", "Data Sci", "STATISTICS", 
    "data science", "Stats"],
    "GradYear": [2026, 2025, 2026, 2024, 2025],
    "GPA": [3.8, 3.45, None, 3.92, 3.6],
    "Internship?": ["Yes", "yes", "No", "No", "YES"],
    "HoursWorked": [12, 15, 8, None, 20]
})
```
### Step 1: Clean up the Column Names
```python
# Use the Pandas '.rename()' method
df = df.rename(columns={
    "StudentName": "student_name",
    "Major": "major",
    "GradYear": "grad_year",
    "GPA": "gpa",
    "Internship?": "internship",
    "HoursWorked": "hours_worked"
})
```

### Step 2: Standardize Text Values
```python
# Use 'str.title()', 'str.lower()' to standardize:
df["student_name"] = df["student_name"].str.title()
df["major"] = df["major"].str.lower()
df["internship"] = df["internship"].str.lower()
```
### Step 2a: Standardize Majors
```python
# Pandas '.replace()' method is great here, just put the column name in the [] brackets and add the current name before the colon ":" with the desired new value after the colon ":".
df["major"] = df["major"].replace({
    "data sci": "data science",
    "stats": "statistics"
})
```

### Step 3: Convert Data Types
```python
# We're going to remap how we display whether students completed an internship to the established standard:
df["internship"] = df["internship"].map({
    "yes": True,
    "no": False
})
# Fun Fact: Boolean values are very versatile and can be utilized in other functions without having to encode 'yes' and 'no' into queries.
```
```python
# Next, we specify graduation year as integers:
df["grad_year"] = df["grad_year"].astype(int)
```

### Step 4: Handle Missing Values
```python
# Drop rows where GPA is missing:
#   **We do this for datasets that are large enough to afford a few missing observations for other columns.**
df = df.dropna(subset=["gpa"])

# In other cases, we can translate missing values to a something more useful, like hours worked:
df["hours_worked"] = df["hours_worked"].fillna(0)

```
### Step 5: Remove Unnecessary Columns
```python
# OR: If we decide the whole column isn't useful, we can drop it:
df = df.drop(columns=["hours_worked"])
```

### Step 6: Add a Clean Index Column;
```python
# If some of our analyses require tracking each row, it can be useful if their indices are displayed as their own column:
df = df.reset_index(drop=True)
df.insert(0, "student_id", range(1, len(df) + 1))
```


## Your Final Table Should Look Something Like This:
| student_id |student_name | major       | grad_year| gpa  |internship |
|------------|-------------|-------------|----------|------|-----------|
| 1          | Alice Smith | statistics  | 2026     | 3.8  | True      |
| 2          | Bob Jones   | data science| 2025     | 3.45 | True      |
| 3          | David Kim   | data science| 2024     | 3.92 | False     |
| 4          | Eva Brown   | statistics  | 2025     | 3.6  | True      |



## __Conclusion and Call to Action__
Try applying this 6-step workflow to your own messy CSV file.
What patterns repeat? What new cleaning rules do you need?
Data cleaning is less about memorizing commands and more about building repeatable structure.

Next time you open a messy dataset, try asking:
1. Are the column names consistent?
2. Are categories standardized?
3. Are data types correct?
4. What missing values matter?


### Thanks for joining me on this tutorial! Check back regularly for more coding tips!