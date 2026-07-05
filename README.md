# Fandango Movie Ratings — Bias Analysis

A data analysis project investigating whether Fandango's displayed movie ratings were inflated compared to other review platforms, replicating and verifying the findings of FiveThirtyEight's 2015 investigation.

## Background

If you're planning to see a movie, how much can you trust an online rating — especially when the company showing that rating also profits from selling the tickets? This project asks whether **Fandango had a bias toward rating movies higher than warranted in order to sell more tickets**, based on the investigation described in FiveThirtyEight's article [*Be Suspicious Of Online Movie Ratings, Especially Fandango's*](http://fivethirtyeight.com/features/fandango-movies-ratings/).

## Goal

Using `pandas` and data visualization, determine whether:
1. Fandango's **displayed star rating** differs from the **true underlying user rating**.
2. Fandango's ratings, once normalized, run systematically higher than ratings on Rotten Tomatoes, Metacritic, and IMDB.

## Data

Two datasets, originally published by FiveThirtyEight ([source repo](https://github.com/fivethirtyeight/data)):

**`fandango_scrape.csv`** — every film pulled from Fandango.com

| Column | Definition |
|---|---|
| FILM | The movie |
| STARS | Star rating displayed on Fandango.com |
| RATING | The actual average rating pulled from the page HTML |
| VOTES | Number of reviews for the film at time of scraping |

**`all_sites_scores.csv`** — aggregate ratings from other platforms, for films with an RT rating, RT user rating, Metacritic score, Metacritic user score, IMDB score, and ≥30 Fandango fan reviews (pulled Aug. 24, 2015)

| Column | Definition |
|---|---|
| FILM | The film in question |
| RottenTomatoes | RT Tomatometer (critics) score |
| RottenTomatoes_User | RT user score |
| Metacritic | Metacritic critic score |
| Metacritic_User | Metacritic user score |
| IMDB | IMDb user score |
| Metacritic_user_vote_count | Number of Metacritic user votes |
| IMDB_user_vote_count | Number of IMDb user votes |

## Methodology

1. **Explore Fandango data** — inspect structure, plot Rating vs. Votes, extract release `YEAR` from film titles, and filter out unreviewed films (zero votes).
2. **Quantify displayed vs. true rating** — compare KDE distributions of `STARS` (displayed) vs. `RATING` (true), then compute `STARS_DIFF = STARS - RATING` to measure the rounding bias.
3. **Explore other platforms** — compare RT critic vs. RT user scores, Metacritic critic vs. user scores, and IMDB/Metacritic vote counts, flagging outlier films.
4. **Merge & normalize** — inner-merge the Fandango and all-sites tables on `FILM`, then rescale every platform's score onto Fandango's 0–5 star scale so distributions are directly comparable.
5. **Compare distributions across platforms** — KDE plots and a clustermap to see how Fandango's rating distribution compares to RT, Metacritic, and IMDB, and to see how the *worst*-reviewed films score across every platform.

## Key Findings

- Fandango consistently **displays a higher star rating than the true average rating** — the `STARS_DIFF` distribution skews positive, with at least one film showing close to a full star of inflation.
- Once normalized to a 0–5 scale, **Fandango's ratings are far less evenly distributed than Rotten Tomatoes critic scores**, clustering tightly around 3–4.5 stars regardless of how a film performed elsewhere.
- Looking at the 10 worst-reviewed films by Rotten Tomatoes critics, Fandango still displayed disproportionately high ratings.
- **Taken 3** is the starkest example: Fandango displayed **4.5 stars**, while its average rating across other platforms was only **1.86**.

## Conclusion

The analysis supports FiveThirtyEight's original conclusion: in 2015, **Fandango's displayed ratings were biased upward** relative to both its own true user ratings and to ratings on competing platforms — consistent with an incentive to inflate scores in order to drive ticket sales.

## Tech Stack

- **Python** — pandas, NumPy
- **Visualization** — Matplotlib, Seaborn (scatterplots, KDE plots, histograms, count plots, clustermap)
- **Environment** — Jupyter Notebook

## Repository Structure

```
fandango-ratings-bias/
├── data/
│   ├── fandango_scrape.csv
│   └── all_sites_scores.csv
├── notebooks/
│   └── Project.ipynb
├── README.md
└── OVERVIEW.md
```

## How to Run

1. Clone the repo and install dependencies:
   ```bash
   pip install pandas numpy matplotlib seaborn jupyter
   ```
2. Place `fandango_scrape.csv` and `all_sites_scores.csv` in the working directory.
3. Launch the notebook:
   ```bash
   jupyter notebook notebooks/Project.ipynb
   ```

## Data Source & Credit

Data originally collected and published by [FiveThirtyEight](https://github.com/fivethirtyeight/data) to accompany Walt Hickey's article *Be Suspicious Of Online Movie Ratings, Especially Fandango's* (2015).

