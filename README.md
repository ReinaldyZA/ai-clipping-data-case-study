# AI Game-Clipping Platform: Data Case Study

An independent data case study for an AI tool that turns game streams into short clips.
It started as a take-home test for an AI & Data role, which I completed on my own. It has been **anonymized** for this portfolio:
the company name is removed and its private data is not included.

**Slides:** [deck/AI_Data_Optimization_Case_Study.pdf](deck/AI_Data_Optimization_Case_Study.pdf)

| Task | Question | Skills |
|---|---|---|
| [1. Data strategy & scraping](task1_steam_scraping/task1_steam_scraping.ipynb) | Which popular games should the platform build AI models for next? | Python, REST APIs, retry and rate-limit handling, data cleaning |
| [2. Game knowledge](task2_game_knowledge/README.md) | What makes a Call of Duty: Warzone moment worth clipping? | Product thinking, SQL, turning user behaviour into training labels |
| [3. Data analysis](task3_sql_analysis/analysis.ipynb) | Five metrics to track every week, from sign-up to Premium | SQL (CTEs, window functions, joins), funnel and churn analysis, matplotlib |

## Key findings

1. **Coverage depends on one game.** 62% of players in the top 20 Steam games play a supported game, but without Counter-Strike it drops to 46%.
2. **Warframe, Team Fortress 2 and Valheim** are the biggest games with no model yet. Survival games are the clearest genre gap.
3. **Users get clips but rarely use them.** 97% of users who submit a stream receive clips, only 36% ever download or share one.
4. **Vertical beats horizontal.** 93% of clips are horizontal highlights with a 1.6% download rate. Vertical TikTok clips: 18.7%.
5. **Kill chains get saved, wins do not.** Killstreaks are downloaded about 4 times more often than single kills.
6. **Monthly Premium churns before it is used.** Half of monthly plans cancel (around day 11), and 86% never submit a stream while subscribed.

## Repository structure

```
task1_steam_scraping/
    task1_steam_scraping.ipynb   collect top 100 Steam games and live player counts, clean, analyse
    steam_top100_raw.csv         raw SteamSpy data (public)
    steam_games_clean.csv        cleaned data used in the analysis
task2_game_knowledge/
    README.md                    full write-up: UI signals, clipping tiers, training data, clip score
    warzone_event_engagement.ipynb   download rate per Warzone event and tier check (query 06)
task3_sql_analysis/
    sql/                         SQL Server queries (00 creates the tables, 01 to 06 are the metrics)
    data/                        synthetic dataset with the same structure as the original
    generate_synthetic_data.py   script that creates data/
    run_queries.py               runs the SQL on data/ with DuckDB, no database server needed
    results/                     query outputs
    analysis.ipynb               charts for each metric
deck/                            final presentation (PPTX and PDF)
```

## About the data

**Task 1** uses public data from the SteamSpy API and the official Steam Web API, collected on 17 October 2026.

**Tasks 2 and 3** were done on the company's private database, which cannot be shared.
To keep the SQL runnable, `task3_sql_analysis/data/` contains a **synthetic dataset**: same tables and columns, random values
that follow similar patterns. Results from it are close to, but not the same as, the real figures shown in the slides and above.

| Table | Rows | Description |
|---|---|---|
| streams | ~12.5k | streams submitted by users |
| clips | ~155k | clips generated from streams, with clip type, game and event |
| clip_downloads | ~4.5k | clip downloads |
| clip_shares | ~0.9k | clip shares to social platforms |
| subscriptions | ~120 | Premium subscriptions |

## How to run

```bash
pip install -r requirements.txt

# Tasks 2 and 3: run all queries on the synthetic data, then open the notebooks
cd task3_sql_analysis
python run_queries.py
jupyter notebook analysis.ipynb
jupyter notebook ../task2_game_knowledge/warzone_event_engagement.ipynb

# Task 1: re-collect fresh Steam data (takes 1 to 2 minutes)
jupyter notebook task1_steam_scraping/task1_steam_scraping.ipynb
```

The SQL files are written for SQL Server. To run them there, create the tables with `sql/00_create_tables.sql` and load the CSV files.

---
Reinaldy Zulfananda Arkaan · [Portfolio](https://reinaldyza-portfolio.vercel.app/projects/ai-clipping-case-study) · [GitHub](https://github.com/ReinaldyZA)
