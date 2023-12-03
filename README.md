# Data Engineering - Assignment 2
The project uses the following dataset: https://www.kaggle.com/datasets/open-source-sports/professional-hockey-database/data.

Pipeline 1: How many award-winning coaches are in the top three ranked teams each year?

Pipeline 2: Which goalie performs the best in shootouts for each team? 

Dashboard: https://lookerstudio.google.com/reporting/c0b649fc-5801-4cb5-ba72-a96ab03f758c.

# Setting up the project
STEP 1: Add the datasets to a GCS bucket.

STEP 2: Create a dataset in BigQuery.
- Dataset ID: assignment2.
- Region: us-central1.

STEP 3: Create 2 tables.
```sql
CREATE TABLE IF NOT EXISTS assignment2.goalies (
  tmID STRING,
  playerID STRING,
  totalSA INT,
  totalGA INT,
  performance FLOAT64,
  denseRank INT,
  player_name STRING,
  age STRING,
  playingYears STRING,
  team_name STRING
)
```
```sql
CREATE TABLE IF NOT EXISTS assignment2.coaches (
  tmID STRING,
  year INT,
  name STRING,
  Pts INT,
  ROW INT,
  dense_rank INT,
  award STRING,
  award_count BIGINT
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
- Dimension: tmID - turn drill down off.
- Breakdown dimension: denseRank.
- Metric: performance.
- Sort: tmID (ascending).
- Secondary sort: performance (descending).

Coaches:
1. Table
- Dimension: year, name, award, dense_rank - turn drill down off.
- Metric: award_count.
- Sort: year (ascending).
- Secondary sort: dense_rank (ascending).
2. Column Chart
- Dimension: dense_rank - turn drill down off.
- Breakdown dimension: award.
- Metric: award_count.
- Sort: dense_rank (ascending).
- Secondary sort: award (ascending).