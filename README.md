# Churn_analysis

https://app.powerbi.com/view?r=eyJrIjoiZTc4YjYzNzItNzBlOC00ZjZlLWFlNDUtNzRlNzk2YTFlMTk0IiwidCI6ImM2ZTU0OWIzLTVmNDUtNDAzMi1hYWU5LWQ0MjQ0ZGM1YjJjNCJ9


# Churn Analysis Project

## Introduction
In today’s competitive business environment, retaining customers is crucial for long-term success. Churn analysis is a key technique used to understand and reduce customer attrition. This project leverages data analytics and machine learning to predict churn and identify factors contributing to customer departures.

## Data & Resources
- **Database:** Microsoft SQL Server
- **Visualization:** Power BI
- **Machine Learning:** Python (Random Forest)
- **Colors Used:** `#FFF5C2 #9F9FD0 #7676BC `

## Target Audience
While this project focuses on a telecom firm, the techniques are applicable across various industries, including retail, finance, and healthcare.

## Project Goals
- Implement an ETL process in SQL Server
- Develop a Power BI dashboard for customer analysis
- Predict future churners using machine learning

## Key Metrics
- Total Customers
- Total Churn & Churn Rate
- New Joiners

## Step 1: ETL Process in SQL Server
1. **Create Database**:
   ```sql
   CREATE DATABASE db_Churn;
   ```
2. **Import CSV into SQL Server**
3. **Data Exploration:**
   ```sql
   SELECT Gender, COUNT(Gender) AS TotalCount FROM stg_Churn GROUP BY Gender;
   ```
4. **Check Null Values:**
   ```sql
   SELECT SUM(CASE WHEN Customer_ID IS NULL THEN 1 ELSE 0 END) AS Customer_ID_Null_Count FROM stg_Churn;
   ```
5. **Remove Nulls & Insert into Production Table**
6. **Create Views for Power BI**
   ```sql
   CREATE VIEW vw_ChurnData AS SELECT * FROM prod_Churn WHERE Customer_Status IN ('Churned', 'Stayed');
   ```

## Step 2: Power BI Transformations
- Create new calculated columns for `Churn Status`, `Monthly Charge Range`, `Age Group`, and `Tenure Group`.
- Unpivot service columns for better visualization.

## Step 3: Power BI Measures
```DAX
Total Customers = COUNT(prod_Churn[Customer_ID])
Churn Rate = [Total Churn] / [Total Customers]
```

## Step 4: Power BI Visualizations
- **Summary Page:** Total Customers, Churn Rate, Demographics, Payment Methods
- **Churn Reason Analysis:** Breakdown of churn categories and reasons
- **Service Usage:** Impact of internet type, contract, and services on churn

## Step 5: Predict Customer Churn with Machine Learning
### Algorithm: Random Forest
#### Data Preparation
```python
import pandas as pd
from sklearn.model_selection import train_test_split
from sklearn.ensemble import RandomForestClassifier
from sklearn.metrics import classification_report

# Load Data
file_path = 'Prediction_Data.xlsx'
data = pd.read_excel(file_path, sheet_name='vw_ChurnData')

# Encode categorical variables
label_encoders = {}
for column in ['Gender', 'Contract', 'Payment_Method']:
    label_encoders[column] = LabelEncoder()
    data[column] = label_encoders[column].fit_transform(data[column])

# Train Model
X = data.drop(['Customer_Status'], axis=1)
y = data['Customer_Status']
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

model = RandomForestClassifier(n_estimators=100, random_state=42)
model.fit(X_train, y_train)
y_pred = model.predict(X_test)

print(classification_report(y_test, y_pred))
```

## Conclusion
This project provides a comprehensive workflow for churn analysis, from ETL to visualization and predictive modeling. By leveraging SQL, Power BI, and machine learning, businesses can proactively address churn and improve customer retention.
