# Peer Analysis – Small Cap Steel Companies (Power BI)

## Overview
Built a Power BI dashboard to compare financial performance of small-cap steel companies across revenue, profitability, and debt using YoY analysis.

## Tools Used
- Power BI  
- Power Query  
- DAX  

## Data Preparation
- Imported data from Excel  
- Unpivoted tables (wide → long format)  
- Created Year column:
  Year = 2000 + VALUE(RIGHT([Year],2))  
- Created Date column:
  Date = DATE([Year],3,31)  

## Data Modeling
Created a separate Company table to enable slicer filtering:

Company Table =  
DISTINCT(  
    UNION(  
        SELECTCOLUMNS(Debt,"Company",Debt[Company]),  
        SELECTCOLUMNS(EBITDA,"Company",EBITDA[Company]),  
        SELECTCOLUMNS(PAT,"Company",PAT[Company]),  
        SELECTCOLUMNS(Revenue,"Company",Revenue[Company])  
    )  
)

Connected Company Table to all fact tables (Revenue, EBITDA, PAT, Debt)  
Relationship: Many-to-One, Single direction  

## DAX Measures

Revenue = SUM(Revenue[Value])  
EBITDA = SUM(EBITDA[Value])  
PAT = SUM(PAT[Value])  
Debt = SUM(Debt[Value])  

Debt YoY % =  
VAR Prev = CALCULATE([Debt], SAMEPERIODLASTYEAR(Debt[date]))  
RETURN IF(ISBLANK(Prev), BLANK(), DIVIDE([Debt]-Prev, Prev))  

## Dashboard Highlights
- Peer comparison of Revenue, EBITDA, PAT  
- YoY growth analysis  
- Margin comparison (EBITDA %, PAT %)  
- Debt trend analysis  
- Company slicer controlling all visuals

## Dashboard preview 
{scp.png}

## Key Insights
- Gallantt Ispat shows strong revenue growth  
- MSP Steel shows volatility in profitability  
- Kamdhenu significantly reduced debt  
- Beekay Steel remains relatively stable

