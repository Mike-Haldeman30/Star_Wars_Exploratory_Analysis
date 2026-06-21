# Star Wars Universe Analysis

An exploratory data analysis of the Star Wars universe using data pulled from the [SWAPI API](https://swapi.py4e.com/). This project demonstrates end-to-end data pipeline skills: API ingestion, multi-endpoint data merging, cleaning, and visual analysis via Tableau Public.

📊 **[View Live Dashboard →](https://public.tableau.com/app/profile/michael.haldeman/viz/StarWarsDashoard/Dashboard1?publish=yes)**
---

## Project Overview

This project answers three core questions about the Star Wars universe:

1. Which species appears most across the films?
2. How do species compare physically — average height and lifespan?
3. Which planets are home to the most species represented in the films?

---

## Data Source

Data was pulled from three SWAPI endpoints:

- `/people` — Characters appearing across the Star Wars films
- `/species` — Biological and physical attributes of each species
- `/planets` — Homeworld data for people and species

**Important limitation:** SWAPI covers the main saga films only. TV series such as The Clone Wars, The Mandalorian, and Andor are not included. This means species and character counts are understated for groups with significant presence outside the films.

---

## Technical Stack

- **Python** (Pandas, NumPy, Matplotlib) — API ingestion, data cleaning, EDA
- **Google Colab** — Development environment
- **Tableau Public** — Interactive dashboard
- **GitHub** — Version control and portfolio hosting

---

## Data Pipeline

### 1. API Ingestion
Used paginated GET requests via the `requests` library to collect all records across multiple pages from each endpoint. Results were accumulated using a `while` loop that followed the `next` URL field until reaching `None`.

### 2. Data Merging
Connected the three DataFrames using URL-based foreign keys — the same pattern as SQL joins:
- People joined to Species on the species URL field
- Result joined to Planets on the homeworld URL field

### 3. Data Cleaning
Several issues required resolution before analysis:

- Numeric fields (`height`, `mass`, `average_lifespan`, `population`, `diameter`, `rotation_period`, `orbital_period`, `surface_water`) were stored as strings and converted to float
- Values of `unknown`, `none`, and `indefinite` were replaced with `NaN`
- Comma-formatted numbers (e.g. `1,358`) were stripped before type conversion
- Species field in the people DataFrame was stored as a list — extracted first element with null handling for the five characters with no listed species
- Redundant URL and join key columns were dropped post-merge
- Duplicate name columns from the merge (`name_x`, `name_y`, `name`) were renamed to `person_name`, `species_name`, `planet_name`

---

## Analysis

### Species Representation
Humans dominate the film cast by a significant margin, which aligns with the human-centric storytelling of the main saga. The top five most represented species account for the majority of named characters.

### Physical Comparison by Species
Average height and lifespan vary significantly across species:
- Hutts have an average lifespan of 1,000 years — providing context for Jabba's long-established criminal empire
- Yoda's species averages 900 years, consistent with the lore around Jedi Master Yoda
- Significant height variation exists across species, from Ewoks at 100cm to Wookiees at 210cm

### Homeworld Distribution
Most planets in the dataset are home to characters from a single species, reflecting the film narrative's tendency to associate specific worlds with specific groups. A small number of planets show higher species diversity.

---

## Dashboard

[View on Tableau Public](#) ← *link to be added*

The dashboard includes:
- KPI cards: total species, planets, and characters
- Bar chart: species by number of characters
- Bar charts: average height and average lifespan by species
- Homeworld breakdown: species diversity per planet

---

## Repository Structure

```
star-wars-analysis/
│
├── star_wars_analysis.ipynb   # Full analysis notebook
├── data/
│   └── star_wars_cleaned.csv  # Cleaned, merged dataset
└── README.md
```

---

## Key Skills Demonstrated

- Paginated REST API ingestion without a dedicated client library
- Multi-source data merging using URL-based foreign keys
- Real-world data cleaning: type conversion, null handling, nested list extraction
- Exploratory analysis with grouped aggregations and sorting
- Analytical restraint — ML was evaluated and excluded due to insufficient sample size and class imbalance, with reasoning documented
