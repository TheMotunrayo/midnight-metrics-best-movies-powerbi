# Midnight Metrics: Analysing 1,000 Movies' Box Office Performance with Power BI
A five page Power BI dashboard exploring what drives box office success across 1,000 films: genres, age ratings, studios, directors, release timing and runtime.

**Connect with me:** [LinkedIn](https://www.linkedin.com/in/motunrayo-olorungbohunmi)

**Objective**:
The dashboard answers four business questions, one per page:

1. How is the movie business performing overall?
2. Which genres and age ratings earn the most, and which do critics prefer?
3. Which studios and directors perform best?
4. When is the best time to release a film, and does runtime matter?

A fifth page summarises the key insights and recommendations.

**Dataset**

The **Best Movie dataset** contains 1,000 films released between 1939 and 2015, with 10 columns: Genre, Production Company, Director, Movie Title, Rating (age rating), Theater Release, Rotten Tomatoes Rating, Movie Length (min), Global Box Office and Budget.

The dashboard defaults to **1997–2015 (983 films)**, which covers 98% of the data.

**Tools**
- Power Query for cleaning and transformation
- DAX for calculated measures
- Power BI Desktop for the data model, visuals and report

**Data Cleaning**
| Issue | What I found | Decision |
|---|---|---|
| Hidden missing values | 95 runtimes and 72 Rotten Tomatoes ratings recorded as 0 | Replaced with null so averages aren't distorted |
| Column name | "Rating" had a hidden leading space | Renamed to **Age Rating** to avoid confusion with the Rotten Tomatoes ratings |
| Duplicate titles | Fantastic Four and Godzilla each appeared twice | Kept all four rows, they are remakes with different directors, studios and years |
| Outlier | Gone with the Wind (1939) listed at $6.6bn, likely inflation-adjusted | Kept but the dashboard defaults to 1997–2015 |
| Suspicious record | Priest (2011) listed at $1.45bn box office and a $781M budget | Kept as supplied, flagged on the dashboard and in the report |
| Low value | Chic! listed at $587 box office | Kept, plausible for a very small release |

I also set correct data types, trimmed and cleaned the text columns, and added **Release Year, Release Month and Profit columns**. All steps were done in Power Query and are recorded in its Applied Steps.

Data Model and DAX Measures

The model is a single table. A separate Date table wasn't needed because the data ends in 2015 and no time-intelligence calculations were required. Auto date/time and relationship auto-detection were turned off.

All measures live in a dedicated Movies Measures table (see measures.dax):
```dax
Total Movies = COUNTROWS(Movies)
Total Box Office = SUM(Movies[Global Box Office])
Total Budget = SUM(Movies[Budget])
Total Profit = SUM(Movies[Profit])
ROI % = DIVIDE([Total Profit], [Total Budget])
Avg RT Rating = AVERAGE(Movies[Rotten Tomatoes Rating])
Avg Movie Length = AVERAGE(Movies[Movie Length (min)])
Avg Box Office per Movie = DIVIDE([Total Box Office], [Total Movies])
Profitable Movies % = DIVIDE(COUNTROWS(FILTER(Movies, Movies[Profit] > 0)), [Total Movies])
```
**Dashboard Pages**
| Page | What it shows |
|---|---|
| **Overview** | KPI cards, box office and number of releases by year, box office by genre, age-rating mix |
| **Genres & Age Ratings** | ROI by genre, critic rating by genre, budget vs box office scatter with trend line, average box office per film by age rating |
| **Studios & Directors** | Top 10 studios and directors, top 10 films table with data bars and colour-coded profit |
| **Release Timing & Runtime** | Monthly releases vs average earnings, genre-by-month heatmap, average box office by 30-minute runtime bands |
| **Insights & Recommendations** | One-page summary, plus data and methodology notes |
