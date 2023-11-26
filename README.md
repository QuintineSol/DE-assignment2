# Data Engineering - Assignment 2

## Pipeline 1
- Use-case: Which goalie performs the best in shootouts for each team between 2000 and 2011? 
- Visualisation: table
    - column 1: team name
    - column 2-4: name of best goalie 1, 2 and 3

## Pipeline 2
- Use-case: How many award-winning coaches are in the top three ranked teams each year?
- To what extent do award-winning coaches contribute to team optimal performance.
- Visualisation: histogram with different colours
    - x-axis: teams
    - y-axis: amount of awards - one colour per category

## ALternatives
Which three coaches were best for each team over the years?

## ALternatives
Which coach was best for each team over the years?

# Setting up the project
STEP 1: Add the datasets to a GCS bucket.

STEP 2: Create a dataset in BigQuery.
- Dataset ID: assignment2.
- Region: us-central1.

STEP 3: Create 2 tables.
```sql
CREATE TABLE IF NOT EXISTS assignment2.goalies (
  playerID STRING,
  player_name STRING,
  age STRING,
  totalSA INT,
  totalGA INT,
  performance FLOAT64,
  playingYears STRING,
  denseRank INT,
  team_name STRING
)
```
```sql
CREATE TABLE IF NOT EXISTS assignment2.coaches (
  year INT64,
  tmID STRING,
  name STRING,
  Pts INT64,
  ROW INT64,
  dense_rank INT64,
  no_awards INT64
)
```

STEP 4: In your VM
```bash
git clone https://github.com/QuintineSol/DE-assignment2.git
cd installation_script
sh docker.sh
sh docker_compose.sh
cd ..
nano .env
```
- EXTERNAL_IP: of the VM
- USER_HOME: everything before @ of green name in terminal

STEP 5: Add the pipeline notebooks to your notebooks folder.
```bash
cp goalies.ipynb ../notebooks
cp coaches.ipynb ../notebooks
```

# Working on the project
STEP 1: Remove volumes of lab containers (since they overlap with the volumes of our containers)
```bash
sudo docker volume rm deployment_notebooks deployment_spark-checkpoint deployment_spark-data
```

STEP 2: Start the docker container
```bash
sudo docker compose build
sudo docker compose up -d
```

STEP 3: Start the spark master and workers
```bash
sudo docker logs spark-driver-app
```
- Copy the URL at the bottom of the output

STEP 4: In the URL, change the IP (http://IP:port/lab?...) to the external IP of your VM

STEP 5: Remove volumes of containers (optional: when you want to follow the labs again)
```bash
sudo docker volume rm de-assignment2_notebooks de-assignment2_spark-checkpoint de-assignment2_spark-data
```

# Dashboard
Goalies:
1. Table
- Dimension: team_name, player_name, age, playingYears - turn drill down off.
- Metric: performance.
- Sort: team_name (ascending).
- Secondary sort: performance (descending).
2. Histogram
- Dimension: team_name - turn drill down off.
- Breakdown dimension: denseRank.
- Metric: performance.
- Sort: team_name (ascending).
- Secondary sort: performance (descending).

Coaches:
1. Table
- Dimension: year, tmID, dense_rank - turn drill down off.
- Metric: no_awards.
- Sort: year (descending).
- Secondary sort: dense_rank (ascending).
2. Histogram
- Dimension: year - turn drill down off.
- Breakdown dimension: dense_rank.
- Metric: no_awards.
- Sort: year (ascending).
- Secondary sort: dense_rank (ascending).