# Netflix Data Engineering Pipeline

An end-to-end data engineering project built using **PySpark and Databricks** to transform the Netflix Titles dataset into validated, business-oriented analytical datasets using a **Bronze–Silver–Gold architecture**.

## 🚀 Project Overview

This project implements a complete data engineering pipeline for the Netflix Titles dataset.

The pipeline follows a **Bronze → Silver → Gold** architecture:

- **Bronze:** Raw data ingestion
- **Silver:** Data cleaning, transformation, and validation
- **Gold:** Purpose-built analytical datasets for business analysis

The project demonstrates practical data engineering concepts including data quality checks, missing-value handling, transformations, aggregations, Delta Lake persistence, and analytical reporting.

---

## 🛠️ Tech Stack

- **Python**
- **PySpark**
- **Databricks**
- **Delta Lake**
- **Matplotlib**

---

## 📊 Dataset

The project uses the **Netflix Titles dataset** containing information about Movies and TV Shows available on Netflix.

**Dataset source:** [Netflix Titles Dataset – Kaggle](https://www.kaggle.com/datasets/padmapriyatr/netflix-titles)

Key columns include:

`show_id`, `type`, `title`, `director`, `cast`, `country`, `date_added`, `release_year`, `rating`, `duration`, and `listed_in`.

---

## 🏗️ Data Pipeline

```text
Netflix Titles Dataset
          │
          ▼
┌─────────────────────┐
│    BRONZE LAYER     │
│    Raw Ingestion    │
│                     │
│  Raw CSV → Spark DF │
└──────────┬──────────┘
           │
           ▼
┌────────────────────────────┐
│       SILVER LAYER         │
│                            │
│  • Data Quality Checks     │
│  • Missing Value Handling  │
│  • Date Transformation     │
│  • Duration Transformation │
│  • Validation              │
└────────────┬───────────────┘
             │
             ▼
┌────────────────────────────┐
│        GOLD LAYER          │
│                            │
│  gold_country              │
│  gold_genre                │
│  gold_rating               │
│  gold_added_year           │
│  gold_duration             │
└────────────┬───────────────┘
             │
             ▼
      Business Analysis
             │
             ▼
       Visualizations
```

---

## 🥉 Bronze Layer — Data Ingestion

The Bronze layer contains the raw Netflix dataset as ingested from the source CSV.

The dataset is loaded into a Spark DataFrame and preserved as the starting point for downstream processing.

The raw dataset is stored in the Databricks environment using Delta Lake.

---

## 🥈 Silver Layer — Data Cleaning & Transformation

The Silver layer contains the cleaned and transformed Netflix dataset.

### Data Quality Checks

The pipeline performs:

- Row count validation
- Duplicate detection using `show_id`
- Null-value analysis
- Data-quality checks for inconsistent values

### Missing Value Handling

Missing values in `director`, `cast`, `country`, and `rating` are represented as `Unknown` rather than removing the corresponding titles.

Records with missing `date_added` are retained because the remaining title-level information is still usable.

### Date Transformation

The `date_added` column is converted from string to a Spark `date` type.

A separate `added_year` column is created for time-based analysis.

### Duration Transformation

The original `duration` field is transformed into:

```text
duration_value
duration_unit
```

Movie durations are represented in minutes, while TV Show durations are represented in seasons.

The pipeline also handles three records where duration values were incorrectly placed in the `rating` column.

### Silver Validation

The final Silver dataset contains **8,807 records**.

The cleaned dataset is validated for duplicates, nulls, and transformation consistency before being persisted as a Delta dataset.

---

## 🥇 Gold Layer — Analytical Data Modeling

The Gold layer contains **five purpose-built analytical datasets** derived from the validated Silver dataset.

### `gold_country`

Analyzes Netflix content by:

- Country
- Content type
- Number of titles

Multi-country values are split and exploded so that individual countries can be analyzed.

### `gold_genre`

Analyzes Netflix content by:

- Genre
- Content type
- Number of titles

The `listed_in` column is split and exploded to analyze individual genres.

### `gold_rating`

Analyzes content distribution by:

- Rating
- Content type
- Number of titles

### `gold_added_year`

Analyzes catalog additions over time using:

- Year added
- Content type
- Number of titles

Records with missing `added_year` are excluded because they cannot be assigned to a specific year.

### `gold_duration`

Summarizes content duration using:

- Duration unit
- Content type
- Total titles
- Average duration
- Minimum duration
- Maximum duration

Movie durations and TV Show durations are analyzed separately because they use different units.

---

## 📈 Business Analysis

The Gold datasets are used to answer business-oriented questions such as:

### 🌎 Country Analysis

- Which countries have the highest number of Movies?
- How are Movies and TV Shows distributed across countries?

### 🎬 Genre Analysis

- What are the most common genres?
- How are genres distributed between Movies and TV Shows?

### 🔖 Rating Analysis

- Which content ratings are most common?
- How are ratings distributed across Movies and TV Shows?

### 📅 Year Analysis

- How has the Netflix catalog changed over time?
- How does the number of Movies and TV Shows added vary by year?

### ⏱️ Duration Analysis

- What are the typical duration ranges for Movies?
- What are the typical duration ranges for TV Shows?

---

## 📊 Visualizations

The project includes visualizations for:

- Top 10 countries by number of Movies
- Top 10 genres
- Most common content ratings
- Movies vs TV Shows added by year

The visualizations are created using **Matplotlib** after performing the required aggregations with PySpark.

---

## 🔍 Key Findings

- Movies make up the larger share of the dataset, with **6,131 Movies** compared with **2,676 TV Shows**.
- The **United States** has the highest number of Movies in the country-level analysis.
- **International Movies, Dramas, and Comedies** are among the most common genres.
- **TV-MA** and **TV-14** are among the most common content ratings.
- The number of Movies and TV Shows added to Netflix varies across years.
- Movie durations are measured in minutes, while TV Show durations are measured in seasons, so their duration statistics are analyzed separately.

---

## 💾 Data Storage

The cleaned Silver dataset and analytical Gold datasets are persisted using **Delta Lake** in Databricks.

### Silver

```text
silver/silver_dataset
```

### Gold

```text
gold/gold_country
gold/gold_genre
gold/gold_rating
gold/gold_added_year
gold/gold_duration
```

The final Gold datasets are read back from their persisted Delta locations to verify successful storage and expected record counts.

---

## 📁 Repository Structure

```text
netflix-data-engineering-pipeline/
│
├── README.md
├── netflix_content_data_pipeline.ipynb
└── .gitignore
```

---

## 🚀 Skills Demonstrated

- PySpark DataFrame API
- Data ingestion
- Data cleaning
- Data quality validation
- Missing-value handling
- String transformations
- Date transformations
- `split()` and `explode()`
- Aggregations
- `groupBy()` and `pivot()`
- Bronze–Silver–Gold architecture
- Delta Lake
- Databricks
- Analytical data modeling
- Business analysis
- Data visualization

---

## 🔄 Reproducibility

To run this project:

1. Download the Netflix Titles dataset from [Kaggle](https://www.kaggle.com/datasets/padmapriyatr/netflix-titles).
2. Upload `netflix_titles.csv` to a Databricks Volume.
3. Update the input path in the notebook to match your Databricks environment.
4. Run the notebook in Databricks.

The notebook uses Databricks Volume paths for reading the source data and persisting the Silver and Gold Delta datasets.

---

## 🎯 Conclusion

This project demonstrates an end-to-end data engineering workflow using **PySpark and Databricks**.

The Netflix dataset is ingested into the Bronze layer, cleaned and transformed in the Silver layer, and modeled into purpose-built analytical datasets in the Gold layer.

The pipeline combines data quality validation, transformation logic, analytical aggregations, Delta Lake persistence, and visualization to provide a structured foundation for analyzing Netflix content by country, genre, rating, year, and duration.
