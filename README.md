
# 🎮 Gaming Churn Prediction

## Project Overview
This project focuses on **predicting player churn** in an online gaming environment.  
By analyzing **January 2025 player data** (training) and predicting churn for **February 2025 players** (testing), this project identifies **high-risk players** and provides actionable insights for **retention strategies**.

The goal is to help gaming companies understand player behavior, reduce churn, and improve engagement using **data-driven decision-making**.

---

## Dataset

- **Training Data:** `player_data_january_400_with_errors.csv` (400 players)  
- **Testing Data:** `player_data_february_400_with_errors.csv` (400 players)  
- **Columns:**

| Column                | Description                     |
| --------------------- | ------------------------------- |
| `player_id`           | Unique player ID                |
| `session_date`        | Date of gameplay                |
| `play_time_mins`      | Minutes played                  |
| `game_genre`          | Game category                   |
| `achievements_unlocked` | Total badges earned           |
| `in_game_purchases`   | Number of in-game purchases     |
| `region`              | Player region                   |
| `churned`             | 1 = stopped playing, 0 = active |

---

## Tools & Technologies Used

- **Python**  
  - `pandas`, `numpy` → Data cleaning, manipulation, aggregation  
  - `scikit-learn` → Machine learning (Logistic Regression)  
  - `matplotlib`, `seaborn` → Data visualization  
- **SQL (via SQLite)**  
  - Store cleaned player data  
  - Basic exploratory queries like **average playtime per region** or **players per genre**  
- **Machine Learning**  
  - Logistic Regression to predict churn probability  
  - Model evaluation using **Classification Report** and **ROC-AUC Score**  
- **Power BI**  
  - Visualize key insights and patterns  
  - Dashboard includes:  
    - Top churn-prone regions  
    - Churn by game genre  
    - Churn vs in-game purchases correlation  
    - High-risk player table for targeted retention  

---

## Project Workflow

### 1️⃣ Data Loading & Exploration
- Load CSVs into Python using `pandas`  
- Explore dataset with `df.head()`, `df.info()` to check for missing values and data types  
- Perform initial summary statistics  

### 2️⃣ Store Data in SQL
- Store cleaned dataset in **SQLite** for querying  
- Example queries:  
```sql
-- Average playtime by region
SELECT region, COUNT(*) AS players, AVG(play_time_mins) AS avg_playtime
FROM player_sessions
GROUP BY region
ORDER BY avg_playtime DESC;

-- Total players per game genre
SELECT game_genre, COUNT(*) AS total_players
FROM player_sessions
GROUP BY game_genre
ORDER BY total_players DESC;
````

### 3️⃣ Data Cleaning

* Convert `session_date` to datetime
* Fix numeric columns (`play_time_mins`, `achievements_unlocked`, `in_game_purchases`)
* Standardize `game_genre` and `region` values
* Handle duplicates and missing values in `churned` column

### 4️⃣ Exploratory Data Analysis (EDA)

* Visualize **churn distribution by genre**
* Analyze **playtime patterns by region**
* Identify high-risk segments using **boxplots and countplots**

### 5️⃣ Feature Engineering

* Aggregate per player:

```python
player = df.groupby('player_id').agg({
    'play_time_mins': ['sum','mean','max'],
    'achievements_unlocked': 'sum',
    'in_game_purchases': 'sum',
    'region': lambda x: x.mode()[0],
    'game_genre': lambda x: x.mode()[0],
    'churned': 'max'
})
```

* Create **Engagement Score** combining playtime, achievements, and purchases

### 6️⃣ Machine Learning – Churn Prediction

* Use **Logistic Regression** to train on January data
* Split dataset: 80% train / 20% test
* Predict churn probability for February players
* Evaluate model performance with **classification report** and **ROC-AUC score**

### 7️⃣ Predictions on February Data

* Clean February data similarly
* Apply trained model to generate **predicted churn probability**
* Highlight high-risk players (e.g., `predicted_churn > 0.7`)

### 8️⃣ Power BI Dashboards

* Visualize top insights:

  * **Top 10 churn-prone regions**
  * **Churn by game genre**
  * **Churn vs in-game purchases**
  * **High-risk players** for actionable retention campaigns

---

## Key Insights

1. **Regional Trends:**

   * North America players show the **highest predicted churn**, Oceania the lowest

2. **Genre Trends:**

   * Battle Royale has the highest churn probability
   * Casual and Puzzle players are most engaged

3. **Engagement Patterns:**

   * Low playtime and few achievements → high churn
   * High playtime & achievements → low churn

4. **In-Game Purchases:**

   * High-spending players usually remain active
   * Some VIP players show medium churn risk → targeted retention needed

5. **Top-Risk Players:**

   * Mostly from Europe and RPG games
   * Should be prioritized for retention campaigns

---

## Recommendations

* Focus **retention campaigns** on high-risk regions and genres
* Reward **high-engagement players** to maintain loyalty
* Offer **personalized incentives** for VIP players at risk
* Monitor monthly churn trends to act proactively

---

## Project Impact

* Demonstrates **how data analytics and machine learning can help gaming companies reduce churn**
* Provides actionable insights for **player retention, engagement optimization, and revenue protection**

---

## Files in Repository

* `player_data_january_400_with_errors.csv` → Raw January training data
* `player_data_february_400_with_errors.csv` → Raw February testing data
* `player_aggregated.csv` → Aggregated player features (training)
* `player_predictions.csv` → Predictions for February with churn probability
