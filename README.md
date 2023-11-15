# Data Engineering - Assignment 2

## Pipeline 1
- Use-case: How many award-winning coaches are in the top three ranked teams each year?
- To what extent do award-winning coaches contribute to team optimal performance.
- Visualisation: histogram with different colours
    - x-axis: teams
    - y-axis: amount of awards - one colour per category

## Pipeline 2
- Use-case: Which goalie performs the best in shootouts for each team between 2000 and 2011? 
- Visualisation: table
    - column 1: team name
    - column 2-4: name of best goalie 1, 2 and 3
    - column 5-7: name of best player 1, 2 and 3

# Setting up the project
STEP 1: Add the datasets to a GCS bucket.

STEP 2: Create a dataset in BigQuery.
- Dataset ID: assignment2.
- Region: us-central1.

STEP 3: Create 2 tables.
``bash
CREATE TABLE IF NOT EXISTS assignment2.goalie (
  name STRING,
  team STRING,
  performance Float64,
)
```
```bash
CREATE TABLE IF NOT EXISTS assignment2.awards (
 team STRING,
 year INT64,
 awards INT64,
 points INT64
)
```

STEP 4:

