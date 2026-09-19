# Film Funding Factors

**What relates to a film's reported return: genre, budget, or cast experience?**

IMDb + TMDB data analysis | 2014 to 2023 | Data Analytics Capstone, General Assembly (DAB 26)

## Overview

Film funding decisions are made before financial outcomes are known. This project examines whether pre-release factors available to a studio greenlight team (budget, genre, previous lead experience, and creative team track record) are associated with a film's reported return.

**Reported Return** = (Reported Revenue − Production Budget) ÷ Production Budget
*(Excludes marketing, distribution, financing costs, and investor payments; not actual investor profit.)*

**Audience:** Studio greenlight teams (primary); investors, financiers, and content producers (secondary).

## Key Findings

- Across 2,465 films, **34.8% reported revenue below production budget**, with an average shortfall of $11.05M among those films.
- **Budget and previous lead experience showed weak individual correlations** with reported return (-0.044 and -0.033).
- **Larger budgets associate with a lower below budget rate**, from 44.3% in the $5M to $10M tier down to 14.5% in the $100M+ tier, though this is an association in this sample, not a funding rule.
- **Genre matters for risk, not just style.** Sci-Fi, Adventure, and Comedy had the lowest below budget rates (26% to 29%); War, History, and Musical had the highest (48% to 53%).
- **Creative team history had the clearest signal.** Films with directors or writers who had a stronger return track record showed notably higher median reported returns (e.g., writers: 1.91 vs. 0.58).
- **In a combined model controlling for production company, only runtime showed a clear relationship** with reported return. Budget, lead experience, and prior genre, director, and writer history each showed limited standalone signal. The model explained 9.1% of variation (7.5% adjusted), a reminder that pre-release factors alone leave most of the outcome unexplained.

## Recommendations

1. Combine multiple signals before greenlighting rather than relying on any one factor.
2. Weigh below budget rates when comparing genre options, not just historical popularity.
3. Treat budget size as context, not a guarantee. Higher budgets correlate with, but don't ensure, staying within budget.
4. Don't use previous lead experience alone as a greenlight signal.
5. Validate creative team history further with broader career data before applying it to funding decisions.

## Data

| Source | Provides |
|---|---|
| IMDb | Film details, genres, cast & crew |
| TMDB (Kaggle) | Budget, revenue, production companies |

**Pipeline:** 227,265 IMDb records and 486,063 TMDB records were matched by title ID down to 102,868 records, filtered to 2,826 usable, and finalized at **2,465 films** (2014 to 2023, 41 columns) after filtering for usable reported budget and revenue.

Raw data files are not included in this repo (the IMDb `title.principals` file alone is about 4.4 GB, and both sources carry their own licensing terms). To reproduce:
- IMDb datasets: https://datasets.imdbws.com/
- TMDB Movies Dataset 2023: https://www.kaggle.com/datasets/asaniczka/tmdb-movies-dataset-2023-930k-movies

**What's included:** `datasets/cleaned/movies_analysis_final.csv` is the final, processed analysis table (2,465 films, 41 columns) used throughout the notebook and presentation. It is a derived dataset built from IMDb and TMDB, filtered and enriched with engineered fields (genre indicators, historical return by genre/director/writer). It is shared here for educational and portfolio purposes only, not as a redistribution of the original raw IMDb or TMDB datasets. See [DATA_DICTIONARY.md](DATA_DICTIONARY.md) for a full column-by-column description.

## Method

1. **Load and store:** Five raw files (four IMDb + one TMDB) loaded in chunks into a local SQLite database to avoid memory errors on the large IMDb files.
2. **Filter and clean:** Filtering and exploration done in DB Browser for SQLite; missing values (IMDb's literal `\N`) standardized; columns missing in over 90% of rows dropped.
3. **Merge:** IMDb and TMDB tables merged by title ID; filtered to films with usable reported budget and revenue.
4. **Analyze:** Correlation analysis, group comparisons (genre, budget tier, lead experience, director/writer history), and a combined regression model controlling for production company.
5. **Visualize:** Key comparisons built in Tableau.

## Tools

`Python` (pandas) · `SQL` (SQLite) · `Tableau`

## Repo Contents

```
film-funding-factors/
├── README.md
├── DATA_DICTIONARY.md             (column-by-column description of the cleaned dataset)
├── notebooks/
│   └── technical_report.ipynb     (full step-by-step analysis: cleaning to modeling)
├── presentation/
│   └── film_funding_factors.pdf   (stakeholder-facing slide deck)
├── datasets/
│   └── cleaned/
│       └── movies_analysis_final.csv   (final analysis table, 2,465 films)
└── requirements.txt
```

## Limitations

- Reported Return is a simplified measure that excludes major real-world costs (marketing, distribution, financing).
- Only films with usable budget and revenue data were included, a selected, not fully representative, sample.
- Unusually large returns may influence some results despite capping at the 1st to 99th percentiles.
- Director and writer prior history data covers only part of the sample.
- The model was not validated on unseen films.

---

**Abeer Radhi** · [LinkedIn](#) · [GitHub](#)
