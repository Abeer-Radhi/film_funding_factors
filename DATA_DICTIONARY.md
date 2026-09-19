# Data Dictionary — movies_analysis_final.csv

One row = one film. 2,465 rows, films released 2014 to 2023.

## Identifiers and descriptive fields

| Column | Type | Description |
|---|---|---|
| `imdb_id` | text | IMDb title identifier. Unique per film. |
| `title` | text | Film title. |
| `release_year` | integer | Year of release (2014 to 2023). |
| `runtime_minutes` | float | Runtime in minutes. Missing for a small number of films. |
| `genres` | text | Comma separated list of genres as reported by IMDb (e.g. "Action,Crime,Drama"). |
| `original_language` | text | ISO language code of the film's original language. |
| `production_companies` | text | Comma separated list of production companies (from TMDB). Missing for some films. |
| `popularity` | float | TMDB popularity score at time of data collection. Not used as a predictor (post-release signal). |

## Financial and outcome fields

| Column | Type | Description |
|---|---|---|
| `budget` | integer | Reported production budget (USD). |
| `revenue` | integer | Reported revenue (USD). |
| `ROI` | float | **Reported Return**: (Revenue − Budget) ÷ Budget. This is the project's outcome variable. Excludes marketing, distribution, and financing costs, so it is not actual investor profit. |
| `imdb_rating` | float | IMDb average user rating. Post-release; not used as a predictor. |
| `imdb_vote_count` | float | Number of IMDb user votes. Post-release; not used as a predictor. |

## Engineered predictor fields

| Column | Type | Description |
|---|---|---|
| `rec_director` | binary (0/1) | 1 if any credited director on the film had at least one prior directing credit before this film's release year, else 0. |
| `rec_writer` | binary (0/1) | 1 if any credited writer had at least one prior writing credit before this film's release year, else 0. |
| `star_power` | binary (0/1) | 1 if the film's lead actor had at least one prior lead-role credit before this film's release year, else 0. Reported in the presentation as **Previous Lead Experience**. |
| `historical_genre_roi` | float | Average reported return of earlier films (prior years only) sharing this film's genre, used as a proxy for genre track record. Missing where no qualifying prior-year films exist for that genre. |
| `historical_director_roi` | float | Average reported return of the director's earlier films (prior years only). Missing where the director has no qualifying prior credits in the dataset. |
| `historical_writer_roi` | float | Average reported return of the writer's earlier films (prior years only). Missing where the writer has no qualifying prior credits in the dataset. |

**Note on missing history:** `historical_genre_roi`, `historical_director_roi`, and `historical_writer_roi` are `NaN` when no qualifying prior-year record exists (not zero). In the regression model, these were filled with 0 alongside a separate `*_available` flag so the model could distinguish "no history" from "history of zero return." Those `*_available` flags are not included in this exported file; see the notebook's Regression Analysis section to reproduce them.

## Genre indicator columns

| Column pattern | Type | Description |
|---|---|---|
| `genre_Action`, `genre_Adventure`, `genre_Animation`, `genre_Biography`, `genre_Comedy`, `genre_Crime`, `genre_Documentary`, `genre_Drama`, `genre_Family`, `genre_Fantasy`, `genre_History`, `genre_Horror`, `genre_Music`, `genre_Musical`, `genre_Mystery`, `genre_News`, `genre_Romance`, `genre_Sci-Fi`, `genre_Sport`, `genre_Thriller`, `genre_War`, `genre_Western` | binary (0/1) | One column per genre, one-hot encoded from the `genres` field. A film can have more than one genre flag set to 1. |

## Source and derivation

Built from IMDb (`name.basics`, `title.basics`, `title.ratings`, `title.principals`) and the TMDB Movies Dataset 2023 (Kaggle), merged by IMDb title ID, filtered to films with usable reported budget and revenue, and enriched with the engineered fields above. See the main [README](README.md) for the full pipeline and [notebooks/technical_report.ipynb](notebooks/technical_report.ipynb) for the step-by-step code.
