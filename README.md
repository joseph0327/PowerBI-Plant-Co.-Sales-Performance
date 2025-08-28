# PowerBI-Plant-Co.-Sales-Performance

🌱 Plant Co. Power BI Dashboard
Gross Profit, Sales & Quantity Performance Analysis (2024)

This project analyzes Gross Profit, Sales, and Quantity performance using Power BI.
It includes YTD vs PYTD comparisons, profitability segmentation, and dynamic visuals.

Screenshots: 
![image alt](https://github.com/joseph0327/PowerBI-Plant-Co.-Sales-Performance/blob/be5ea1cabff32bbfece1fcfe9b2d1f5feeb643eb/visual1.png)
![image alt](https://github.com/joseph0327/PowerBI-Plant-Co.-Sales-Performance/blob/be5ea1cabff32bbfece1fcfe9b2d1f5feeb643eb/visual2.png)
![image alt](https://github.com/joseph0327/PowerBI-Plant-Co.-Sales-Performance/blob/be5ea1cabff32bbfece1fcfe9b2d1f5feeb643eb/visual3.png)

----------------------------------------------------

📊 Data Model
- Fact_Sales → Sales, COGS, Price, Quantity
- Dim_Product → Product hierarchy
- Dim_Account → Customer details
- Dim_Date → Calendar for time intelligence
- _Measure → Stores calculated measures
- Slc_Values → Switch for dynamic measures

  Sceenshot:
  ![image alt](https://github.com/joseph0327/PowerBI-Plant-Co.-Sales-Performance/blob/be5ea1cabff32bbfece1fcfe9b2d1f5feeb643eb/Data%20Model.png)
  

----------------------------------------------------
🚀 Steps to Start the Project

1. Load & Transform Data
   - Load Excel
   - Rename fact & dim tables
   - Remove duplicate IDs
   - Fix data types
   - Close & Apply

2. Create Date Table
   Dim_Date = CALENDAR(DATE(2022,1,1), DATE(2024,12,31))

3. Add InPast Column
   Inpast =
   VAR lastsalesdate = MAX(Fact_Sales[Date_Time])
   VAR lastsalesPY = EDATE(lastsalesdate,-12)
   RETURN Dim_Date[Date] <= lastsalesPY

4. Create Slicer Table
   - Enter Data → Values: Gross Profit, Quantity, Sales

5. Create Measures Table (_Measure)
   - Store all measures

----------------------------------------------------
📐 Key Measures

Sales = SUM(Fact_Sales[Sales_USD])  
Quantity = SUM(Fact_Sales[quantity])  
Cost of Goods = SUM(Fact_Sales[COGS_USD])  
Gross Profit = [Sales] - [Cost of Goods]  

YTD_Sales = TOTALYTD([Sales], Fact_Sales[Date_Time])  
YTD_Quantity = TOTALYTD([Quantity], Fact_Sales[Date_Time])  
YTD_GrossProfit = TOTALYTD([Gross Profit], Fact_Sales[Date_Time])  

PYTD_Sales = CALCULATE([Sales], SAMEPERIODLASTYEAR(Dim_Date[Date]), Dim_Date[Inpast] = TRUE)  
PYTD_Quantity = CALCULATE([Quantity], SAMEPERIODLASTYEAR(Dim_Date[Date]), Dim_Date[Inpast] = TRUE)  
PYTD_GrossProfit = CALCULATE([Gross Profit], SAMEPERIODLASTYEAR(Dim_Date[Date]), Dim_Date[Inpast] = TRUE)  

-- Switch Measures --
S_PYTD =
VAR SelectedValue = SELECTEDVALUE(Slc_Values[Values]) 
RETURN SWITCH(
    SelectedValue, 
    "Sales", [PYTD_Sales],
    "Quantity", [PYTD_Quantity],
    "Gross Profit", [PYTD_GrossProfit],
    BLANK()
)

S_YTD =
VAR SelectedValue = SELECTEDVALUE(Slc_Values[Values]) 
RETURN SWITCH(
    SelectedValue, 
    "Sales", [YTD_Sales],
    "Quantity", [YTD_Quantity],
    "Gross Profit", [YTD_GrossProfit],
    BLANK()
)

YTD vs PYTD = [S_YTD] - [S_PYTD]

-- Date Columns --
Year = YEAR(Dim_Date[Date])  
Quarter = "Q" & FORMAT(Dim_Date[Date], "Q")  
Month = FORMAT(Dim_Date[Date], "MMMM")  
Month Number = MONTH(Dim_Date[Date])  
Day = DAY(Dim_Date[Date])  

----------------------------------------------------
🎨 Dashboard Design
- Cards (KPIs: YTD, PYTD, YTD vs PYTD, GP%)  
- Slicer (Gross Profit, Quantity, Sales)  
- Waterfall (YTD vs PYTD)  
- TreeMap (Bottom 10 countries)  
- Line + Column chart (YTD & PYTD by month & product type)  
- Scatter (GP% vs Value segmentation)  

Dynamic Titles:
Waterfall Title = SELECTEDVALUE(Slc_Values[Values]) & " YTD vs PYTD | Month-Country-Product"
Report Title = "Plant Co. " & SELECTEDVALUE(Slc_Values[Values]) & " Performance " & SELECTEDVALUE(Dim_Date[Date].[Year])

----------------------------------------------------
📷 Dashboard Previews  
- Gross Profit Performance (visual1.png)  
- Quantity Performance (visual2.png)  
- Sales Performance (visual3.png)  

----------------------------------------------------
🛠 Tools Used
- Microsoft Power BI Desktop  
- DAX (Data Analysis Expressions)  
- Excel  

----------------------------------------------------
📌 Key Insights
- Dynamic slicer switches between Gross Profit, Sales, and Quantity  
- YTD vs PYTD comparison highlights performance gaps  
- Scatter segmentation identifies customer profitability clusters  
- Titles adjust dynamically for clarity  

----------------------------------------------------
📎 Author
Built with Power BI by Joseph Villarin
