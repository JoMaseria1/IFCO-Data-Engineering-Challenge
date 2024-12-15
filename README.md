# IFCO Data Engineering Challenge

This challenge involves processing and analyzing datasets to answer various business-related tests. For this, I used:

- Databricks Community Edition (PySpark) for Tasks 1-5.
- Power BI for Task 6 (Dashboarding).

### Resources:

[Databricks Notebook](https://community.cloud.databricks.com/editor/notebooks/4458272523191699?o=388291198913926)

[Test 6 - Power BI](https://github.com/JoMaseria1/IFCO-Data-Engineering-Challenge/blob/2a6f78bbd554e7d9d3305f313c5e74e10d76a5b2/Test%206%20-%20Power%20BI.pbix)

## Tasks 1-5

The PySpark notebook is divided into three main sections:

- Loading and transforming orders.csv: Import, clean, and prepare data for analysis, then export it as CSV.
- Loading and transforming invoicing_data.json: Similar to the previous step but for invoicing data.
- Test completion: Solving the provided tests using the transformed data, with detailed code comments and unit tests.

Considerations:

- Due to the limitations of the Community Edition, a new cluster needs to be created in the environment to run the notebook.

Since the tasks are resolved and commented line by line in the notebook, please refer to the provided resource to evaluate them.

## Task 6

[orders_df.csv](https://github.com/JoMaseria1/IFCO-Data-Engineering-Challenge/blob/eb3749b735948f360df7ae7f028c059e7a523083/orders_df.csv)

[invoicing_df.csv](https://github.com/JoMaseria1/IFCO-Data-Engineering-Challenge/blob/eb3749b735948f360df7ae7f028c059e7a523083/invoicing_df.csv)

The transformed datasets in Databricks have been loaded into Power BI as data sources and dimension tables have been created in the data model to performn the necessary calculations and relationships. The resulting model can be found below:

![image](https://github.com/user-attachments/assets/862d3de8-28e4-4d53-a65e-73366ec84252)

Considerations:

- While Dim_Order_ID, Dim_Crate, Dim_Owner_Type and Dim_Sales_Owner are a result of unique values in the fact tables, Dim_Calendar has been generated with the function CALENDARAUTO.

### 1. Distribution of orders by crate type: 

The pie chart shows the following distribution over the past 2 years:

Plastic: 10 (36%)
Wood: 9 (32%)
Metal: 9 (32%)

![image](https://github.com/user-attachments/assets/24b33173-43d0-4a9d-b406-7df9c98f94b8)

### 2. Sales owners that need most training to improve selling on plastic crates, based on the last 12 months orders:

Focusing on Main Owners only and due to invoicing data is missing over the past 12 months, the distribution of number of orders has been stated as the key metric to determine that worst performers were David Goliat, Chris Pratt and Leon Leonov.

![image](https://github.com/user-attachments/assets/2c8ce6df-fa7a-44f3-b7f6-be97b93f23e2)

### 3. Top 5 performers by month selling plastic crates for a rolling 3 months evaluation window:

The rolling sum was calculated as the following DAX metric:

```
# Orders (3M-RS) = 
VAR MaxDate = MAX(Dim_Calendar[Date]) 
VAR ThreeMonthsAgo = EDATE(MaxDate, -3) -- 3 months before max date
VAR RollingSum = 
    CALCULATE(
        [# Orders],
        Dim_Calendar[Date] > ThreeMonthsAgo,
        Dim_Calendar[Date] <= MaxDate
    )
RETURN 
RollingSum
```

The top performers are found in the summary table (yellow), while the MoM evolution of the rolling sum is represented in the bottom line chart (orange)

![image](https://github.com/user-attachments/assets/faad31dd-3f91-4957-9977-6c110c7bf30e)








