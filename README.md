
# 🌦️ Weather Data Analytics Pipeline using AWS & Open-Meteo API

This project is a fully automated, cloud-based pipeline designed to collect, process, and visualize weather forecast data for multiple U.S. cities using the [Open-Meteo API](https://open-meteo.com/). It integrates various AWS services including Glue, S3, Athena, and connects to Power BI for interactive dashboards.

---

## 📌 Project Overview

**Goal**: Automate weather data collection and analysis using serverless cloud tools.

**Cities Covered**:
- New Haven
- New York
- Boston
- San Francisco
- Los Angeles
- Las Vegas
- Washington D.C.

---

## 🔄 Data Flow Pipeline

```
Open-Meteo API → AWS Glue Job 1 → S3 (CSV) → AWS Glue Job 2 (Parquet Conversion) 
→ S3 (Processed) → AWS Glue Crawler → AWS Athena → Power BI Dashboard
```

---

## 🧰 Tools & Technologies

- **Open-Meteo API** – Free hourly/daily weather forecast API
- **AWS S3** – Storage for raw and processed data
- **AWS Glue Jobs** – ETL logic written in Python and PySpark
- **AWS Glue Crawler** – Automatic schema detection for Athena
- **AWS Athena** – SQL querying over Parquet data
- **Power BI** – Data visualization

---

## ⚙️ Components Explained

### 1. ETL Job: Fetching Raw Data
- Calls Open-Meteo API for each city
- Extracts hourly and daily data
- Adds columns like `location`, `date_only`, `time_only`, `is_day`
- Uploads as `hourly_data.csv` and `daily_data.csv` to S3 bucket:  
  `s3://weather-data-bucket-test1/rawData/`

### 2. ETL Job: CSV to Parquet Conversion
- Reads the raw CSVs from S3
- Converts them into optimized Parquet format
- Overwrites data in `s3://weather-data-bucket-test1/processedData/`

### 3. AWS Glue Crawler
- Crawls the processed S3 location daily
- Updates the Glue Data Catalog (tables: `plshourly`, `plsdaily`)
- Enables Athena to read the latest schema and data

### 4. Amazon Athena
- SQL engine used to query Parquet data
- Output location for results: `s3://<your-athena-query-result-bucket>/`

### 5. Power BI Dashboard
- Connects via Athena ODBC Driver
- Includes visuals like:
  - Rain & snow depth by location
  - Daily temperature trends
  - UV index by city
  - Cloud cover timeline
  - Drill-down: Daily → Hourly

---

## 🕒 Automation Setup

- Both Glue jobs along with the crawlers are triggered **daily at 6 AM** with the help of AWS Workflow feature in AWS Glue
- All previous data in `rawData/` and `processedData/` is **overwritten**
- This ensures dashboards reflect the latest forecasts

---

## 📊 PowerBI Dashboard

![PAGE 1](https://github.com/DeTeam01/weather_analytics_dashboard/blob/main/weather_dashboard_pg_1.png)
![PAGE 2](https://github.com/DeTeam01/weather_analytics_dashboard/blob/main/weather_dashboard_pg_2.png)

---

## 📁 Folder Structure For S3 bucket

```
📦 weather-data-bucket-test1/
├── rawData/
│   ├── hourly_data.csv
│   └── daily_data.csv
├── processedData/
│   ├── hourly/ (Parquet)
│   └── daily/  (Parquet)
```

---

## 🧑‍💻 For Developers

- Scripts are available for both ETL jobs
- Jobs can be monitored in AWS Glue console
- Add more cities in the `location_map` dictionary in Job 1

---

## 🤝 Contributors

- **Aldric Pinto**
- **Tanak Patel**
- **Trevor Hitchcock**

---

## 📬 Contact

For questions or demos, reach out at [deteam591@gmail.com].

---

## 📎 Our Data Pipeline

To add a pipeline screenshot:

![DE PIPELINE](https://github.com/DeTeam01/weather_analytics_dashboard/blob/main/AWS%20DE%20PIPELINE.png)

---

## ✅ Summary

This pipeline showcases how to turn real-time weather forecasts into valuable insights using a serverless data stack. It is scalable, cost-effective, and easy to extend to new locations or additional weather metrics.
