# Olympic Games Analysis (Power BI + SQL)

## Project Overview
This project analyzes historical **Summer Olympic Games data** to uncover insights about country performance, athlete participation, and medal distribution.

The goal is to build an **interactive Power BI dashboard** that allows users to explore Olympic trends across years, countries, sports, and competitors.

---

## Business Problem
As a data analyst working for a news company, the objective is to:

- Help readers understand how countries have performed historically in the Summer Olympics  
- Provide insights into competitors and their participation  
- Enable users to explore data dynamically (by country, sport, year, etc.)

---

## Tech Stack
- **SQL Server** – Data extraction & transformation  
- **Power BI** – Data modeling & visualization  
- **DAX (Data Analysis Expressions)** – Calculations & measures  

---

## Project Files
- `OlympicGamesAnalysis.pbix` → Power BI dashboard  
- `DataAnalystProject_SQL_PBI_OlympicGamesAnalysis.sql` → SQL transformations  
- `Olympic Games Portfolio Project.docx` → Project documentation  

---

## Data Preparation (SQL)
Data was extracted from the `athletes_event_results` table and transformed into a clean analytical format.

### Key Transformations:
- Renamed columns for clarity (e.g., `Name → Competitor Name`)
- Converted gender codes (`M/F → Male/Female`)
- Created **Age Groups**
- Extracted **Year** from `Games` column
- Filtered only **Summer Olympics**
- Replaced `NA` medals with `"Not Registered"`

### Sample SQL:
```sql
SELECT
    [ID],
    [Name] AS 'Competitor Name',
    CASE WHEN SEX = 'M' THEN 'Male' ELSE 'Female' END AS Sex,
    [Age],
    CASE 
        WHEN [Age] < 18 THEN 'Under 18'
        WHEN [Age] BETWEEN 18 AND 25 THEN '18-25'
        WHEN [Age] BETWEEN 25 AND 30 THEN '25-30'
        WHEN [Age] > 30 THEN 'Over 30'
    END AS [Age Grouping],
    [Height],
    [Weight],
    [NOC] AS 'Nation Code',
    LEFT(Games, CHARINDEX(' ', Games) - 1) AS 'Year',
    [Sport],
    [Event],
    CASE WHEN Medal = 'NA' THEN 'Not Registered' ELSE Medal END AS Medal
FROM [olympic_games].[dbo].[athletes_event_results]
WHERE RIGHT(Games, CHARINDEX(' ', REVERSE(Games)) - 1) = 'Summer';
