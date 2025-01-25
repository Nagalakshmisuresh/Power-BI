
# Box Office - Dashboard

### Dashboard Link : https://app.powerbi.com/view?r=eyJrIjoiYzFhYjMxMGItNmVmYS00ODFkLTg3ZjMtZmRkMzMzZTc0NGI2IiwidCI6Ijk0ZWFkZTY1LTQ4NDEtNDIxNC05NjkxLTFiM2NkYWU1YTM3NyJ9

## Problem Statement

The Indian Box Office dataset comprises detailed information on films for the last 10 years, including aspects such as film titles, release dates, lead 
actors/actresses, industries, budgets, collections, verdicts, IMDb ratings, runtimes, OTT 
platforms, directors, languages, and genres. Participants are tasked with leveraging Power BI 
to analyse this dataset and derive meaningful insights that can inform decision-making within 
the film industry.
Dataset provided: Participants will be provided with a rich collection of datasets, each 
meticulously curated to facilitate a comprehensive analysis of the Indian Box Office landscape 
over the past decade. The datasets include:
• Boxoffice_fact.csv: This file encompasses granular details of box office collections, 
providing essential quantitative metrics for revenue analysis.
• Director_dim.csv: A dimensional dataset offering professional information about film 
directors, enabling participants to correlate directorial influence with box office 
success.
• Genere_dim.csv: This dataset categorizes movies into various genres, aiding in the 
examination of genre-specific trends and audience preferences.
• Language_dim.csv: Language-specific data allowing for nuanced insights into the 
performance of films across different linguistic demographics.

### Steps followed 

- Step 1 : Load data into Power BI Desktop, dataset is a csv file.
- Step 2 : Click on Home > Get Data > Excel (or any relevant source) to import your data.
- Step 3 : Once imported, click Transform Data to open the Power Query Editor.
- Step 4 : On the right-hand side, in the Query Settings pane, rename your query (e.g., Movie Data).
- Step 5 : Open power query editor & in view tab under Data preview section, check "column distribution", "column quality" & "column profile" options.
- Step 6 : Also since by default, profile will be opened only for 1000 rows so you need to select "column profiling based on entire dataset".
- Step 7 : Ensure all column headers are meaningful and consistent (e.g., replace spaces with underscores or remove special characters if necessary).
   To rename a column:
  Right-click on the column header and select Rename, or double-click on the column header and type the new name.
- Step 8 : Verify and change data types for each column:
  Select each column and set the appropriate data type from the ribbon:
  Date: Release Date
  Whole Number/Decimal: FilmID, Budget in Crores,    Worldwide Collection in Crores, etc.
  Text: Title, Lead Actor/Actress, Industry, etc.
- Step 9 : Remove any unnecessary rows, such as rows with null or blank values.
- Step 10 : If any column has inconsistent data, replace errors or unwanted values.
- Step 11 : Sort rows based on specific columns.

### Based on Requirement  Created  Insights : 

### KPIs
### Visual Type: Card Visuals
- Total number of films (Count of FilmID).
- Total budget (Sum of Budget in Crores).
- Total worldwide collections (Sum of Worldwide Collection in Crores).
- Average IMDb rating (Average of IMDb Rating).

### Top 10 Films by Worldwide Collection :
### Visual Type: Bar Chart (Horizontal)
- Highlights the highest-grossing films.

### Annual Release Patterns :
### Visual Type: Line Chart
-  Analyzes trends in film releases per year and identifies the year with the highest number of releases.

### First-Day vs Worldwide Collections :
### Visual Type: Scatter Plot
- Identifies films with unusual success patterns on their opening day.

###  Genre Performance Based on IMDb Ratings :
### Visual Type: Clustered Bar Chart
- Compares the performance of different genres based on their average IMDb ratings.

###  Relationship Between Budgets and Worldwide Collections :
### Visual Type: Scatter Plot
- Explores whether higher budgets lead to better worldwide collections.

### Directors with the Most Box Office Hits (Based on Verdict) :
### Visual Type: Stacked Column Chart

- Identifies directors with the most successful films.

### Language Trends and Successful Languages :
### Visual Type: Donut Chart
- Identifies the most financially successful languages for films in the last 10 years.

###  Success of Films on OTT Platforms  :
### Visual Type: Stacked Bar Chart
- Analyzes the popularity and financial success of films released on OTT platforms.

###  Impact of Lead Actors/Actresses on Film Success :
### Visual Type: Clustered Bar Chart
- Assesses how specific actors/actresses influence film success.

Created Few new Calculated Columns following DAX expression was written;
       
       First-Day Percentage = 
       [First Day Collection Worldwide  in Crores] /   [Worldwide Collection  in Crores] * 100

       Directors Boxoffice hits = SWITCH(TRUE(), Boxoffice_Fact[Verdict] = "All Time Blockbuster", 1,
                           Boxoffice_Fact[Verdict] = "Blockbuster", 2,
                           Boxoffice_Fact[Verdict] = "Super Hit", 3,
                           Boxoffice_Fact[Verdict] = "Hit", 4,
                           Boxoffice_Fact[Verdict] = "Average", 5,
                           Boxoffice_Fact[Verdict] = "Above Average", 6,
                           Boxoffice_Fact[Verdict] = "Below Average", 7,
                           Boxoffice_Fact[Verdict] = "Flop", 8,
                           Boxoffice_Fact[Verdict] = "Disaster", 9)



Snap of new calculated column ,

![Snap_1]![Image](https://github.com/user-attachments/assets/0aa2b9cc-0686-43b3-aa84-4cbf527b2a01)

        
- Step 12 : New measure was created while developing the Report.

  Some of the Dax Measures are,
        
        Average IMDB Rating = AVERAGE(Boxoffice_Fact[IMDb Rating])
        Dates MTD = CALCULATE(SUM(Boxoffice_Fact[Worldwide Collection  in Crores]),DATESMTD('Date Master'[Date]))
        Dates QTD = CALCULATE(SUM(Boxoffice_Fact[Worldwide Collection  in Crores]),DATESQTD('Date Master'[Date]))
        Dates YTD = CALCULATE(SUM(Boxoffice_Fact[Worldwide Collection  in Crores]),DATESYTD('Date Master'[Date]))
        Distinct Genre ID = DISTINCTCOUNT(Genere_dim[GenreID])
        Same Period Last Year = CALCULATE(SUM(Boxoffice_Fact[Worldwide Collection  in Crores]),SAMEPERIODLASTYEAR('Date Master'[Date]))
        Total Budget = SUM(Boxoffice_Fact[Budget in Crores])
        Total Films = DISTINCTCOUNT(Boxoffice_Fact[FilmID])
        Total First Day Collections = SUM(Boxoffice_Fact[First Day Collection Worldwide  in Crores])
        Total Genre films = DISTINCTCOUNT(Genere_dim[GenreID])
        Total IMDB Ratings = SUM(Boxoffice_Fact[IMDb Rating])
        Total Indian Gross Collections = SUM(Boxoffice_Fact[India Gross Collection in Crores])
        Total Overseas Collection = SUM(Boxoffice_Fact[Overseas Collection in Crores])
        Total World Wide Collections = SUM(Boxoffice_Fact[Worldwide Collection  in Crores])
        WorldwidePercentage = DIVIDE(
        SUM('Boxoffice_Fact'[Worldwide Collection in   Crores]),
        [Total World Wide Collections],0) * 100
        YOY % = ([Total World Wide Collections] - [Previous Year]) / ([Previous Year]) * (100)

        
  A card visual was used to represent World Wide Collections.

  ![Snap_Count]![Image](https://github.com/user-attachments/assets/30cfc33e-1f99-4e5f-9e9d-9593421f05a6)

        
 - Step 13 : New measure was created to find Toal films
 
   Following DAX expression was written to find Total Films,
 
         Total Films = DISTINCTCOUNT(Boxoffice_Fact[FilmID])
 
   A card visual was used to represent this Total Films.
 
   Snap of Total Films
 
   ![Snap_Percentage]![Image](https://github.com/user-attachments/assets/acf7b394-09e4-4cee-8bc6-c1c07daaccdc)

 
 - Step 14 : New measure was created to calculate total India Collections.
 
   Following DAX expression was written to find total distance,

   Total Indian Gross Collections = SUM(Boxoffice_Fact[India Gross Collection in Crores])
  
 
   A card visual was used to represent the Total India Collections.
 
 
   ![Snap_3]![Image](https://github.com/user-attachments/assets/cf39f903-c819-43e4-b015-a7052c126473)
 
 - Step 15 : New measure was created to calculate total Overseas Collections.
 
   Following DAX expression was written to find total Overseas Collections,

   Total Overseas Collection = SUM(Boxoffice_Fact[Overseas Collection in Crores])
  
 
   A card visual was used to represent the Total Overseas Collections.
 
 
   ![Snap_3]![Image]![Image](https://github.com/user-attachments/assets/6614cc85-8887-42ff-ad99-3e835756d2ec)
 
 - Step 16 : The report was then published to Power BI Service.
 
 
![Publish_Message](https://user-images.githubusercontent.com/102996550/174094520-3a845196-97e6-4d44-8760-34a64abc3e77.jpg)

# Snapshot of Dashboard (Power BI Service)

![dashboard_snapo]![Image](https://github.com/user-attachments/assets/f3518234-d143-466e-9321-316cd45281aa)

 
 # Report Snapshot (Power BI DESKTOP)

 
![Dashboard_upload]![Image](https://github.com/user-attachments/assets/4636e5dc-abf3-4742-b879-ca4d4760d13c)


![Dashboard_upload]![Image](https://github.com/user-attachments/assets/2afdd066-74be-46ba-b1e8-f9a7ffafe3cc)


![Dashboard_upload]![Image](https://github.com/user-attachments/assets/7e1f4c86-fe75-4853-92b1-9f94d36d3ae2)


![Dashboard_upload]![Image](https://github.com/user-attachments/assets/242fb0e5-9f52-4f9d-ae11-e3cc751ca42f)
# Box Office-Dashboard.md.txt
Displaying # Box Office-Dashboard.md.txt.

