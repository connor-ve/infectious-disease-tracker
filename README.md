# Infectious Disease Tracker

> **Archived project.** This repository is kept as a record of a 2023 senior design project and is not maintained. The AWS resources (RDS database, Lambda API, Amplify site) are no longer live, so the app will not run as-is. The code is here for reference only.

Senior design capstone, Cleveland State University (Spring 2023), built by a five-person team.

A web app for tracking infectious diseases across the United States. It collects weekly case data from the CDC and the New York Times, stores it in MySQL on AWS RDS, serves it through a serverless API, and shows it on an interactive US map with state rankings, state-to-state comparisons, and predicted case counts for the rest of the year.

[View the project poster (PDF)](web/FInalPoster.pdf)

## Architecture

```
CDC NNDSS / CDC WONDER / NYT COVID data
              |
      data/  (Python scrapers)
              |
      etl/   (Python ETL)  ---->  AWS RDS (MySQL)
                                        |
                          api/  AWS Lambda (Python) + API Gateway
                                        |
                          web/  React app on AWS Amplify
```

## Repository layout

| Folder | What's in it |
|---|---|
| `data/` | Scrapers for CDC weekly disease tables and NYT COVID data, plus raw 2022-2023 datasets and 2020 population data |
| `etl/` | Cleans the data and loads it into MySQL: weekly case tables, population tables, and prediction tables loaded from `preds.csv` |
| `api/` | The Lambda function behind API Gateway. It answers queries by disease, state, year and week. `api/README.md` covers the AWS setup. |
| `web/` | React frontend: US map (D3), charts (Chart.js), state rankings, comparison views and predictions (MUI, Framer Motion) |

## Tech stack

**Data:** Python, pandas, requests, BeautifulSoup, MySQL
**Cloud:** AWS RDS, Lambda, API Gateway, Amplify
**Frontend:** React, D3, Chart.js, MUI, Framer Motion

## How it was run

These steps are kept for reference. Running them now would need a new MySQL/RDS instance and a new Lambda deployment.

**ETL:** copy `etl/.env.example` to `etl/.env` and fill in your MySQL connection, then:
```bash
cd etl
pip install -r requirements.txt
python main.py
```

**API:** deploy `api/lambda/` to Lambda with `pip install -r requirements.txt -t .`, and set the `DB_HOST`, `DB_USER` and `DB_PASSWORD` environment variables.

**Frontend:**
```bash
cd web
npm install
npm start
```

## My role

I owned the cloud infrastructure, data engineering and DevOps for the project:

- **Cloud infrastructure:** set up the AWS environment: MySQL on RDS, the Lambda API behind API Gateway, and hosting the React app on Amplify.
- **Data engineering:** wrote the Python scrapers for CDC and New York Times data, and the ETL that cleans it and loads the weekly case, population and prediction tables into MySQL.
- **DevOps:** handled deployments and environment configuration for the team, from the database to the Lambda packaging to the Amplify builds.

## Data sources

Data comes from the [CDC NNDSS](https://www.cdc.gov/nndss/) and [The New York Times](https://github.com/nytimes/covid-19-data). It was used for a non-commercial educational project, under each source's terms.
