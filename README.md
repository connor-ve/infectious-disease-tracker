# Infectious Disease Tracker

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

## Running locally

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

<!-- Fill in: e.g. built the data pipeline, ETL, RDS setup, and the Lambda API -->

## Data sources

Data comes from the [CDC NNDSS](https://www.cdc.gov/nndss/) and [The New York Times](https://github.com/nytimes/covid-19-data). It was used for a non-commercial educational project, under each source's terms.
