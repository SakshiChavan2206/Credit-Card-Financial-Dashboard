
# Credit Card Financial Dashboard
## Problem Statement

To develop a comprehensive credit card weekly dashboard that provides real-time insights into key performance metrics and trends, enabling stakeholders to monitor
and analyze credit card operations effectively. 

### Steps followed 

- Step 1 : Created two tables in MySQL and imported the data from a CSV file.

for creating Credit Card Transaction Table, the following DAX expression was written;
       
        CREATE TABLE credit_card (
    Client_Num INT,
    Card_Category VARCHAR(20),
    Annual_Fees INT,
    Activation_30_Days INT,
    Customer_Acq_Cost INT,
    Week_Start_Date DATE,
    Week_Num VARCHAR(20),
    Qtr VARCHAR(10),
    current_year INT,
    Credit_Limit DECIMAL(10,2),
    Total_Revolving_Bal INT,
    Total_Trans_Amt INT,
    Total_Trans_Ct INT,
    Avg_Utilization_Ratio DECIMAL(10,3),
    Use_Chip VARCHAR(10),
    Exp_Type VARCHAR(50),
    Interest_Earned DECIMAL(10,3),
    Delinquent_Acc VARCHAR(5)
);

for creating Credit Card Customer Table, the following DAX expression was written;

        CREATE TABLE customer(
    Client_Num INT,
    Customer_Age INT,
    Gender VARCHAR(5),
    Dependent_Count INT,
    Education_Level VARCHAR(50),
    Marital_Status VARCHAR(20),
    State_cd VARCHAR(50),
    Zipcode VARCHAR(20),
    Car_Owner VARCHAR(5),
    House_Owner VARCHAR(5),
    Personal_Loan VARCHAR(5),
    Contact VARCHAR(50),
    Customer_Job VARCHAR(50),
    Income INT,
    Cust_Satisfaction_Score INT
);

- Step 2 : Load data into Power BI Desktop, connecting through MySQL Database.
- Step 3 : Open the power query editor & in the view tab under the Data preview section, check "column distribution", "column quality" & "column profile" options.
- Step 4 : Also since by default, profile will be opened only for 1000 rows so you need to select "column profiling based on entire dataset".   
- Step 5 : It was observed that in none of the columns errors & empty values were present.
- Step 6 : Two separate dashboards were created for detailed analysis. One is based on credit card transactions and another on customer details.
- Step 7 : For analysing the revenue as per age and income, I have created two groups i.e. Income Group and Age Group(mentioned Later).
- Step 8 : Income Group contains three categories i.e. "<35000", ">35000 && <70000", ">70000" named "Low", "Medium", "High" respectively.
- Step 9 : Age Group contains six categories i.e. "< 30", ">=30 && <40", ">=40 && <50", ">=50 && <60", ">=60", "Others, if any" named "20-30", "30-40", "40-50", "50-60", "60+", "Unknown" respectively.
- Step 10 : In the report view, under the canvas background option, I have selected a wallpaper as per the desired theme and adjusted the transparency as per the requirement.
- Step 11 : To calculate the revenue, I added all the three streams of revenue i.e. Annual Fee, Total Transaction Amount, and Interest Earned.
- Step 12 :  Since the data contains various dimensions to analyse the revenue against, thus in order to analyze, different visuals are added in the visualizations pane in the report view. 

      Revenue = credit_card[Annual_Fees] + credit_card[Total_Trans_Amt] + credit_card[Interest_Earned]

- Step 13 : Visual filters (Slicers) were added for the field named "Week_Start_Date". Also four tree maps are added to perform as a slicer i.e. "Quarters", "Card Category", "Income Group", and "Gender". Treemap was choosen over slicer to show different categories with different colours which was unlikeky to show in slicers.
- Step 14 : Eight card visuals were added in total for both the canvas, those are representing "Revenue", "Total Interest", "Total Transaction Amount", "Total Transaction Count" in credit card transactions and "Revenue", "Total Interest", "Income", "Customer Satisfaction Score" in Credit Card Customer.           
- Step 15 : In the Credit Card Customer report, the whole report is seggregated as per the "Gender" to get a clear picture of credit card used genderwise.

- Step 16 : Visuals used to represent revenue as per different categories are mentioned below,
 
 For Credit Card Transaction Report:

  (a) Revenue, Interest Earned, Annual Fee as per Card Categories (Table)

  (b) Quarter-wise Revenue and Total Transaction Count (Line and Stacked Column Chart)
  
  (c) Revenue by Expenditure Type (Stacked Bar Chart)
  
  (d) Revenue by Education Level (Stacked Bar Chart)
  
  (e) Revenue by Customer Job (Stacked Bar Chart)
  
  (f) Revenue by Chip Type (Donut Chart)

  (g) Revenue by Card Category (Pie Chart)

  
For Credit Card Customer Report (For Both male and female separately):

  (a) Revenue by Week (Line Chart)
  
  (b) Revenue, Interest Earned, Income as per customer Job (Table)
  
  (c) Revenue by Age Group (Stacked Bar Chart)
  
  (d) Revenue by Top 5 States (Stacked Bar Chart)
  
  (e) Revenue by Marital Status (Stacked Bar Chart)
  
  (f) Revenue by Income Group (Stacked Bar Chart)

  (g) Revenue by Dependent (Stacked Bar Chart)

  (h) Revenue by Education (Stacked Bar Chart)
  
For Additional Information:

 (a) Current Week Revenue, preview Week Revenue, Week on Week Growth as per Week Numbers (Table)
 
 (b) Delinquent Account Count, %GT of Delinquent Account(Table)
 
 (c) Count of Card Activation within 30 days, %GT of Activation (Table)


- Step 17 : In the report view, under the insert tab, two text boxes were added to the canvas, in which respective report names were mentioned.
- Step 18 : Additional Column Calculated in which, customers were grouped into various age groups.

for creating a new column following DAX expression was written;
       
        Age_Group = SWITCH(
            TRUE(),
                customer[Customer_Age] < 30, "20-30",
                customer[Customer_Age] >=30 && customer[Customer_Age] < 40, "30-40",
                customer[Customer_Age] >=40 && customer[Customer_Age] < 50, "40-50",
                customer[Customer_Age] >=50 && customer[Customer_Age] < 60, "50-60",
                customer[Customer_Age] >=60, "60+",
                "Unknown"
)
        
Snap of the new calculated column ,
![image](https://github.com/user-attachments/assets/45f09870-92d8-410a-99c0-1d22901c5170)




- Step 19 : Additional Column Calculated in which, customers were grouped into various income groups.

for creating new column following DAX expression was written;
       
        Income_Group = SWITCH(TRUE(),
                    customer[Income] < 35000, "Low",
                    customer[Income] >=35000 && customer[Income] < 70000, "Medium",
                    customer[Income] >= 70000, "High",
                    "Unknown"
)
  
- Step 20 : A new measure was created to find Week Numbers for respective weeks.

Following DAX expression was written for the same,
        
        Week_Num02 = WEEKNUM(credit_card[Week_Start_Date])
               
 - Step 21 : A new measure was created to calculate Previous_Week_Revenue,
 
 Following DAX expression was written to find Previous_Week_Revenue,
 
         Previous_Week_Revenue = CALCULATE(SUM(credit_card[Revenue]),
                                 FILTER(ALL(credit_card),
                                 credit_card[Week_Num02] = MAX(credit_card[Week_Num02])-1))
 
- Step 22 : A new measure was created to calculate Current_Week_Revenue,
 
 Following DAX expression was written to find Current_Week_Revenue,
 
         Current_Week_Revenue = CALCULATE(SUM(credit_card[Revenue]),
                                 FILTER(ALL(credit_card),
                                 credit_card[Week_Num02] = MAX(credit_card[Week_Num02])))
 
 - Step 23 : A new measure was created to calculate Week on Week Revenue Growth.
 
 Following DAX expression was written to find Week on Week Revenue Growth,
 
         WOW_Revenue = DIVIDE(([Current_Week_Revenue]-[Previous_Week_Revenue]),[Previous_Week_Revenue])
    
 A Table visual represented Current_Week_Revenue, Previous_Week_Revenue, and Week on Week Revenue Growth.
 
 ![image](https://github.com/user-attachments/assets/d5e8f6f2-7481-4675-8eee-3aedc434a112)



  - Step 24 : New transaction and customer data are added to the respective existing files to keep the report up-to-date using the append query. Also, After appending the data, the "Enable Load" option was turned off to hide the file from the report view to avoid confusion.
 
 - Step 25 : The report was then published to Power BI Service.
 ![image](https://github.com/user-attachments/assets/d6c76533-8bbe-49d1-aa38-843b9bbf126b)

 

# Snapshot of Dashboard (Power BI Desktop)

 ![image](https://github.com/user-attachments/assets/6eeb78d5-1eb6-4ca9-b715-54519e726a93)

 ![image](https://github.com/user-attachments/assets/c7e523ba-5950-4d95-8064-6bb7e5681804)
 
# Insights

A single-page report was created on Power BI Desktop & it was then published to Power BI Service.

WoW change:

• Revenue increased by 28.8%,

• Total Transaction Amt & Count increased by xx% & xx%

• Customer count increased by xx%

Overview YTD:

• Overall revenue is 57M

• Total interest is 8M

• Total transaction amount is 46M

• Male customers are contributing more in revenue 31M, females 26M

• Blue & Silver credit cards are contributing to 93% of overall
transactions

• TX, NY & CA is contributing to 68%

• Overall Activation rate is 57.5%

• Overall Delinquent rate is 6.06%
