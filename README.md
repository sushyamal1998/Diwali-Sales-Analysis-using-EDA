# Diwali Sales Analysis using EDA(Exploratory Data Analysis)

## Project Overview

**Project Title** : Diwali Sales Analysis


This Dataset contain retail data collecting in Diwali festival session. dataset contain columns like User_id, Customer_id, Product_name, Age_group, Product_category, Ammount etc.<br>
Here i habe to visualize some business related queries which helps company to analyze this data which helps to growth business.This Exploratory Data Analysis (EDA) project includes:

- **Data loading, Cleaning and Preprocessing**
- **UnivariateAnalysis**
- **Bivariate Analysis**
- **Multivariate Analysis**

## Tools and Library Used
- **Python**
- **pandas->** For data manupulation
- **Matplotlib and Seaborn->** For Datavisualization
- **Google Colab->** For interactive Analysis

## Dataset
- **Source->** Kaggle - Diwali Sales Analysis
- **Rows->** 11251
- **Columns->** 15 features

### 1: Data Loading, cleaning and preprocessing
- import necessary libraries
```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
```
- loading dataset
```python
df = pd.read_csv("/content/Diwali Sales Data.csv", encoding='latin-1')
```

- Dataset cleaning
```python
df.info()
df.drop(['Status','unnamed1'], axis = 1, inplace = True)
```
- check any column contain empty string or empty space
```python
for col in df.columns:
  if df[col].dtype == "object":
    has_empty = df[col].str.strip().eq("").any()
    if has_empty:
      print(f"column {col} has empty string")
```
- Change Marital Status column name
```python
def change_stat(value):
  if value == 1:
    return "Married"
  else:
    return "Not Married"

df["Marital_Status"] = df["Marital_Status"].apply(change_stat)
```
- drop null values
```python
x = df['Amount'].mean()
df['Amount'] = df['Amount'].fillna(x)
df['Amount']= df['Amount'].astype(int)
```

### 2: Exploratory Data Analysis(EDA)
- univariate analysis, to see total number of male/female candidate.
```python
ax = sns.countplot(x = df['Gender'])
ax.bar_label(ax.containers[0])
plt.show()
```

- Univariate analysis: count state wise customer.
```python
ax =sns.countplot(x = df['State'])
ax.bar_label(ax.containers[0])
sns.set_style(style = 'darkgrid')
plt.xticks(rotation = 90)
plt.show()
```

- Univariate analysis of prodct category.
```python
ax = sns.countplot(x =df['Product_Category'])
ax.bar_label(ax.containers[0])
plt.xticks(rotation = 90)
plt.show()
```

- Univariate analysis of age.
```python
ax = sns.histplot(x = df['Age'], bins = 10)
ax.bar_label(ax.containers[0])
plt.show()
```

- Bivariate analysis: to see total amount by gender.
```python
sales_gen = df.groupby(['Gender'], as_index = False)['Amount'].sum().sort_values(by = ['Amount'], inplace = False)
ax = sns.barplot(x= 'Gender', y = 'Amount', data = sales_gen)
ax.bar_label(ax.containers[0])
plt.title("purcase amount by gender")
plt.tight_layout()
plt.show()
```

- Bivariate analysis: Count number of male/female candidate of different age group.
```python
ax =sns.countplot(x = df['Age Group'], hue = df['Gender'])
for container in ax.containers:
  ax.bar_label(container)
plt.show()
```

- Bivariate Analysis: Total purchase amount by age group.
```python
sample_age= df.groupby(['Age Group'], as_index = False)['Amount'].sum().sort_values(by = 'Amount', ascending = False)
ax = sns.barplot(x = 'Age Group', y = 'Amount', data = sample_age)
ax.bar_label(ax.containers[0],fmt='₹%.0f')
plt.show()
```

- Bivariate Analysis: State wise number of order.
```python
sample_state = df.groupby(['State'], as_index = False)['Orders'].sum().sort_values(by = ['Orders'], ascending = False)
ax = sns.barplot(x = 'State', y = 'Orders', data = sample_state)
ax.bar_label(ax.containers[0], fmt='₹%.0f')
plt.xticks(rotation = 90)
plt.show()
```

- Bivariate Analysis: number of order by mariatl status.
```python
sample_state = df.groupby(['Marital_Status'], as_index = False)['Orders'].sum().sort_values(by = ['Orders'], ascending = False)
ax = sns.barplot(x = 'Marital_Status', y = 'Orders', data = sample_state)
ax.bar_label(ax.containers[0])
plt.show()
```

- Bivariate Analysis: number of orders by occupation.
```python
sample = df.groupby(['Occupation'], as_index = False)['Orders'].sum().sort_values(by = 'Orders', ascending = False)
ax = sns.barplot(x = 'Occupation', y = 'Orders', data = sample)
ax.bar_label(ax.containers[0])
plt.xticks(rotation = 90)
plt.show()
```

- Multivariate Analysis: total amount by gender and age group.
```python
sample =df.groupby(["Age Group", "Gender"], as_index = False)["Amount"].sum().sort_values(by = "Amount", ascending = False)
plt.figure(figsize = (12,6))
ax = sns.barplot(x = "Age Group", y = "Amount", hue = "Gender", data = sample)
for i in ax.containers:
  ax.bar_label(i,fmt='₹%.0f')

plt.title("Total Purchase Amount by Age Group and Gender", fontsize=14)
plt.show()
```

- Multivariate Analysis: State wise spending by gender.
```python
df_city_gender = df.groupby(['State', 'Gender'], as_index=False)['Amount'].sum()
plt.figure(figsize=(8, 5))
sns.barplot(x='State', y='Amount', hue='Gender', data=df_city_gender, palette='coolwarm')
plt.title("State-wise Purchase by Gender", fontsize=14)
plt.tight_layout()
plt.xticks(rotation = 90)
plt.grid(True)
plt.show()
```

- Heatmap: Correlation between Amount, Age, Orders.
```python
corr = df[['Amount', 'Age', 'Orders']].corr()
plt.figure(figsize = (4,4))
sns.heatmap(corr, annot = True)
plt.show()
```

- Pie Chart: Purchase Distribution by Marital Status and Gender.
```python
df_marital = df.groupby(['Marital_Status', 'Gender'])['Amount'].sum().unstack()
df_marital.plot(kind='pie', subplots=True, autopct='%1.1f%%', figsize=(10, 5), legend=False)
plt.title("Purchase Distribution by Marital Status")
plt.show()
```


## Conclusion

- From the above data we can conclude that most of customer are from uttar pradesh and most of them are female and most of there age between 30-40.
- Age group 26-35 buy highest amount of things.
- Along the customer most of them are unmarried and most customer are working IT sector.
- From heatmap it shows amount, Age, Orders are not too much related to each other.
  
