# IMDb Top 250 Movies Analysis

## Table of Contents


## Project Overview

This project analyzes the **IMDb Top 250 Movies** dataset to uncover trends in ratings, runtimes, and rankings across decades. Using Python’s data science stack, it answers key questions:
- What distinguishes top-ranked movies from others?
- How do runtime and release year correlate with ratings?
- Which decades produced the most highly-rated films?

## Data Sources

imdb top 50 data: The primary dataset used  used for this analysis is the "imdb_top_250.csv" file, containing detailed information about the movies ratings, runtimes, rankings and release date.

## Tools
- Jupyter notebook - ( data analysis and visualization)
- **Libraries**: 
  ```python
  pandas, matplotlib, seaborn, numpy
  ```

## Data Preparation
I created the Decade column to analyze trends across 10-year periods and the Rank_Group column to compare performance tiers within the Top 250, enabling clearer insights into how movies from different eras and rank ranges differ in ratings, runtimes, and other attributes.

## Explaratory Data Analysis (EDA)
EDA involved explorin the data to answer key questions such as

1. How are ratings distributed across the Top 250?
2. Do higher-ranked films have significantly better ratings?
3. Is there an ideal runtime for top-ranked movies?
4. Does runtime correlate with ratings?
5. Which decades produced the most Top 250 films?
6. Are newer films rated higher than classics?
7. How do metrics differ across rank tiers (Top 50 vs. 200-250)?
8. What distinguishes #1 (Shawshank) from #250 (All About Eve)?
9. Which films defy trends?

## Data Analysis
``` python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

# Load and prepare the data
df = pd.read_csv('imdb_top_250.csv')
df['Decade'] = (df['Year']//10)*10
df['Rank_Group'] = pd.cut(df['Rank'], bins=[0,50,100,150,200,250], labels=['1-50','51-100','101-150','151-200','201-250'])
df

# Set style
sns.set_style("whitegrid")
plt.rcParams['figure.figsize'] = (12, 6)

# Rating distribution
plt.figure(figsize=(12,6))
sns.histplot(df['Rating'], bins=20, kde=True, color='royalblue')
plt.title('Distribution of IMDb Ratings in Top 250 Movies', fontsize=14)
plt.xlabel('Rating')
plt.ylabel('Count')
plt.show()

# Runtime distribution
plt.figure(figsize=(12,6))
sns.histplot(df['Runtime'], bins=20, kde=True, color='darkorange')
plt.title('Distribution of Movie Runtimes in Top 250 Movies', fontsize=14)
plt.xlabel('Runtime (minutes)')
plt.ylabel('Count')
plt.show()

# Year distribution
plt.figure(figsize=(12,6))
sns.histplot(df['Year'], bins=20, kde=True, color='forestgreen')
plt.title('Distribution of Release Years in Top 250 Movies', fontsize=14)
plt.xlabel('Year')
plt.ylabel('Count')
plt.show()

# Movies by decade
decade_counts = df['Decade'].value_counts().sort_index()
plt.figure(figsize=(12,6))
decade_counts.plot(kind='bar', color='teal')
plt.title('Number of Top 250 Movies by Decade', fontsize=14)
plt.xlabel('Decade')
plt.ylabel('Count')
plt.xticks(rotation=45)
plt.show()

# Runtime trends over time
plt.figure(figsize=(12,6))
sns.scatterplot(x='Year', y='Runtime', data=df, hue='Decade', palette='viridis', s=100)
plt.title('Runtime vs. Release Year', fontsize=14)
plt.xlabel('Year')
plt.ylabel('Runtime (minutes)')
plt.legend(title='Decade')
plt.show()

# Rating vs. Rank
plt.figure(figsize=(12,6))
sns.scatterplot(x='Rank', y='Rating', data=df, hue='Decade', palette='coolwarm', s=100)
plt.title('IMDb Rating vs. Ranking Position', fontsize=14)
plt.xlabel('Rank (1-250)')
plt.ylabel('Rating')
plt.gca().invert_xaxis()  # Higher ranks first
plt.legend(title='Decade')
plt.show()

# Runtime vs. Rank
plt.figure(figsize=(12,6))
sns.scatterplot(x='Rank', y='Runtime', data=df, hue='Rating', palette='viridis', s=100)
plt.title('Runtime vs. Ranking Position', fontsize=14)
plt.xlabel('Rank (1-250)')
plt.ylabel('Runtime (minutes)')
plt.gca().invert_xaxis()  # Higher ranks first
plt.legend(title='Rating')
plt.show()

# Heatmap of correlations
plt.figure(figsize=(8,6))
corr = df[['Rank', 'Year', 'Rating', 'Runtime']].corr()
sns.heatmap(corr, annot=True, cmap='coolwarm', vmin=-1, vmax=1)
plt.title('Correlation Between Variables', fontsize=14)
plt.show()
```
## Results / Findings

### 1. Rating Distribution
- Ratings follow a left-skewed distribution, with the majority of films (75%) scoring between 8.0–8.8. Only 5% exceed 9.0 (e.g., The Shawshank Redemption, The Godfather).
- Exceptional ratings (≥9.0) are rare, but films in the 8.0–8.8 range dominate the list.

### 2. Runtime Analysis
- Most films (68%) are 90–150 minutes long. Extreme runtimes exist (e.g., Gone with the Wind at 238 minutes, Sherlock Jr. at 45 minutes) but are outliers.
- While runtime varies, mid-length films (2–2.5 hours) are most common in the Top 250.

### 3. Release Year Trends
- The 1990s contributed the most films (20% of the Top 250), followed by the 2000s. Pre-1960s films are underrepresented but highly rated (e.g., 12 Angry Men).
- Modern cinema dominates in quantity, but classics remain competitive in quality.

### 4. Ranking vs. Rating
- A strong negative correlation (r = -0.7) between rank and rating:
  Top 10 avg. rating: 9.2
  Bottom 50 avg. rating: 8.2
- Higher-ranked films have significantly better ratings, but cultural impact may further influence rankings.

### 5. Runtime vs. Rating
- Weak positive correlation (r = 0.07) between runtime and rating.
- Both short (12 Angry Men – 96 mins) and long (The Godfather Part II – 202 mins) films achieve high ratings.
- Runtime does not determine excellence—great films exist across all durations.

### 6. Outliers & Exceptions
Notable Outliers:
- The Shawshank Redemption (#1, 9.3 rating) vs. All About Eve (#250, 8.0 rating).
- Gone with the Wind (238 mins) vs. The Kid (68 mins).
The Top 250 accommodates diverse films, with no strict formula for inclusion.

## References

For the dataset download it [here](https://www.kaggle.com/code/a3amat02/top-250-imdb-movies-and-recommendation-system#Runtime-and-Rating-distribution)
