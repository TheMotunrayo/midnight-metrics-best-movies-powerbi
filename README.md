# Midnight Metrics: Analysing 1,000 Movies' Box Office Performance with Power BI
A five page Power BI dashboard exploring what drives box office success across 1,000 films: genres, age ratings, studios, directors, release timing and runtime.

**Connect with me:** [LinkedIn](https://www.linkedin.com/in/motunrayo-olorungbohunmi)

![Overview](<Screenshot of power bi/Screenshot 2026-10-27 155431.png>)

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

All measures live in a dedicated Movies Measures table (See all DAX measures in [measures.dax](measures.dax).): 
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

The Release Year, Genre and Age Rating slicers are synced across all pages.

![Genres & Age Ratings](<Screenshot of power bi/Screenshot 2026-10-27 155501.png>)

![Studios & Directors](<Screenshot of power bi/Screenshot 2026-10-27 155530.png>)

![Release Timing & Runtime](<Screenshot of power bi/Screenshot 2026-10-27 155609.png>)

![Insights & Recommendations](<Screenshot of power bi/Screenshot 2026-10-27 155633.png>)

## Key Findings (1997–2015)

**Headline numbers:** 983 films, $176bn global box office, $57bn total budget, $119bn profit, 207% ROI, average Rotten Tomatoes rating of 3.21.

- **Volume doesn't equal earnings.** Adventure, Action, Comedy and Drama generate about 83% of box office. Drama has the most films (254) but earns $22bn, less than half of Adventure's $51bn from 115 films.
- **Critics and profits don't always agree.** Drama is the highest-rated major genre (3.43) but has the lowest ROI (139%). The top 10 earners still average a 3.6 rating, so the biggest hits are rarely critical failures.
- **One bad record can rewrite a finding.** Horror's ROI is 204.5% with Priest and 279.2% without it.
- **Family-friendly films earn more per release.** R-rated films average $98M per film, against $229M for PG-13, $271M for PG and $347M for G.
- **Efficiency beats volume.** Universal leads with $13.9bn from 48 films, but Walt Disney Pictures earned $10.8bn from 25 (about $430M vs $290M per film). David Yates earned $4.6bn from 4 films; Clint Eastwood earned $2.2bn from 14.
- **Release timing matters.** May, June and July hold about 25% of releases but about 40% of box office. January ($76M per film) and September ($78M) are the weakest months.
- **Longer films earn more, but not because they're long.** Films of 150–179 minutes average $588M vs $148M for 90–119 minutes, because long films tend to be franchise epics.

## Recommendations
1. Build the slate around Adventure and Action franchises.
2. Balance it with low-budget, high-ROI genres such as Romantic Comedy and Horror.
3. Aim big-budget films at PG-13 and PG audiences.
4. Release tentpoles in May–July or November–December; keep January, September and October for smaller films.
5. Invest in proven franchises and directors.
6. Don't rely on reviews for revenue, but keep quality high on tentpoles.
7. Improve the data: validate figures, standardise studio names, fill missing values, and add marketing costs and inflation adjustment.

## Limitations
- ROI covers production budgets only; marketing, distribution and the cinemas' share of revenue aren't included.
- Some figures appear erroneous or inconsistent (Priest; Avatar's $469M budget).
- 2015 is a partial year (data ends 7 August 2015).
- Small groups (Western, Concert/Performance, Documentary, NC-17, G) can produce extreme values.
- Studio names are inconsistent, which splits totals for companies such as Disney.
- The dataset is a sample of 1,000 films, not the whole industry.

```
├── README.md
├── Motunrayo Best Movie Dashboard.pbix   Power BI file
├── Best_Movie_Dashboard.pdf              PDF export of all five pages
├── measures.dax                          All DAX measures
└── screenshots/                          One image per dashboard page
```

## How to Open

1. Download the `.pbix` file.
2. Open it in [Power BI Desktop](https://www.microsoft.com/power-platform/products/power-bi/desktop) (free, Windows).
3. To refresh the data, update the data source path under **Transform data → Data source settings** to point to your copy of the dataset.

No Power BI? Open the PDF instead.

---

## About Me

Hi, I'm **Motunrayo Olorungbohunmi**, a data analyst turning raw data into clear stories with Power BI, Excel Python,and SQL. I'm documenting my journey, my projects and everything I learn along the way.

This project is part of that journey. If you have feedback, questions or ideas, I'd love to hear from you.

- 💼 **LinkedIn:** [LinkedIn](https://www.linkedin.com/in/motunrayo-olorungbohunmi)
- ✍️ **Medium:** [add your Medium profile link here]
- 📧 **Email:** [motundoreen@gmail.com]

*Built by Motunrayo Olorungbohunmi · October 2026*
