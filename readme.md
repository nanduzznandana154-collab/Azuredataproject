#  Formula 1 Data Engineering & Analytics Platform

An end-to-end **Azure Data Engineering and Business Intelligence project** that ingests Formula 1 racing data from a REST API, stores and processes the data using **Azure Data Factory, Azure Data Lake Storage Gen2, and Azure Databricks**, and delivers analytical insights through **Power BI dashboards**.

The project demonstrates a modern cloud-based data pipeline following a **Raw → Ingested → Presentation → Analytics** architecture.

---

## 📊 Project Overview

This project builds a complete data pipeline for Formula 1 racing data.

The pipeline automatically extracts F1 race information from a REST API, stores the raw data in Azure Data Lake Storage Gen2, transforms and cleans the data using Azure Data Factory and Azure Databricks, creates analytical datasets, and visualizes the results in Power BI.

### End-to-End Architecture

```text
                    ┌─────────────────────┐
                    │   F1 REST API       │
                    │ api.jolpi.ca/ergast │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │ Azure Data Factory   │
                    │      (ADF)           │
                    └──────────┬──────────┘
                               │
                               ▼
              ┌────────────────────────────────┐
              │ Azure Data Lake Storage Gen2   │
              │                                │
              │  Raw → Ingested → Presentation │
              └───────────────┬────────────────┘
                              │
                              ▼
                    ┌─────────────────────┐
                    │ Azure Databricks    │
                    │                     │
                    │ PySpark             │
                    │ Data Transformation │
                    │ Data Analysis       │
                    └──────────┬──────────┘
                               │
                               ▼
                  ┌────────────────────────┐
                  │ Analytical Outputs     │
                  │                        │
                  │ races_per_season       │
                  │ races_by_country       │
                  └────────────┬───────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │     Power BI        │
                    │                     │
                    │ Interactive         │
                    │ Dashboards           │
                    └─────────────────────┘
🎯 Project Objectives

The main objectives of this project are:

●Build an end-to-end cloud data engineering pipeline.
●Extract Formula 1 racing data from a REST API.
●Implement data ingestion using Azure Data Factory.
●Store data in Azure Data Lake Storage Gen2.
●Organize data into multiple processing layers.
●Transform and analyze data using Azure Databricks and PySpark.
●Generate analytical datasets for reporting.
●Build interactive Power BI dashboards.
●Demonstrate practical experience with Azure data engineering technologies.

🛠️ Technologies Used
Technology	Purpose
●Azure Data Factory	Data ingestion and pipeline orchestration
●Azure Data Lake Storage Gen2	Cloud data storage
●Azure Databricks	Data transformation and analysis
●Apache Spark / PySpark	Distributed data processing
●Power BI	Data visualization and dashboarding
●REST API	Source of Formula 1 racing data
●Parquet	Analytical data storage format
●GitHub	Version control and project documentation

🏗️ Data Architecture

The project follows a layered data architecture.

                    SOURCE
                      │
                      ▼
                F1 REST API
                      │
                      ▼
              ┌───────────────┐
              │     RAW       │
              │               │
              │ Original API  │
              │ response data │
              └───────┬───────┘
                      │
                      ▼
              ┌───────────────┐
              │   INGESTED    │
              │               │
              │ Structured /  │
              │ processed data│
              └───────┬───────┘
                      │
                      ▼
              ┌───────────────┐
              │ PRESENTATION   │
              │               │
              │ Cleaned and   │
              │ analytics-ready│
              │ data          │
              └───────┬───────┘
                      │
                      ▼
                ANALYTICS
                      │
             ┌────────┴────────┐
             ▼                 ▼
      races_per_season   races_by_country
             │                 │
             └────────┬────────┘
                      ▼
                   Power BI
☁️ Azure Data Lake Storage

The project uses Azure Data Lake Storage Gen2 as the central storage layer.

The main container used in the project is:

azuredata

The Presentation layer contains analytical datasets generated using Databricks.

azure-data/
│
└── Presentation/
    │
    ├── races_per_season/
    │
    └── races_by_country/
🔄 Azure Data Factory Pipeline

Azure Data Factory is used to ingest data from the Formula 1 REST API and move it into Azure Data Lake Storage.

Pipeline Flow
F1 REST API
     │
     ▼
REST Linked Service
     │
     ▼
Copy Activity
     │
     ▼
ADLS Raw Layer
     │
     ▼
Data Transformation
     │
     ▼
ADLS Presentation Layer

Key ADF Components
●REST API connection
●Azure Data Lake Storage Gen2 linked service
●REST dataset
●JSON dataset
●Copy Activity
●Data Flow
●Transformation pipeline

 Data Transformation
The data is processed through Azure Data Factory and Azure Databricks.

The transformation process includes:
●Reading ingested data
●Flattening nested JSON structures
●Selecting relevant fields
●Cleaning the data
●Structuring race information
●Preparing analytical datasets
●Writing processed data in Parquet format

Example analytical fields include:
season
round
raceName
circuitId
circuitName
country
locality
latitude
longitude

 Azure Databricks
Azure Databricks is used for Spark-based transformation and analysis.

The project uses PySpark to read the Presentation layer and generate analytical outputs.

Presentation Data
presentation_path = "abfss://azure-data@azure00014.dfs.core.windows.net/Presentation/"

df_presentation = spark.read.parquet(presentation_path)

 Analytical Processing
1. Races per Season

The project calculates the number of races conducted in each season.

df_races_per_season = (
    df_presentation
    .groupBy("season")
    .count()
    .withColumnRenamed("count", "race_count")
    .orderBy("season")
)

The resulting dataset contains:

season
race_count

It is saved to:

Presentation/races_per_season/
2. Races by Country

The project also calculates the number of races conducted in each country.

df_races_by_country = (
    df_presentation
    .groupBy("country")
    .count()
    .withColumnRenamed("count", "race_count")
    .orderBy("country")
)

The resulting dataset contains:

country
race_count

It is saved to:

Presentation/races_by_country/

💾 Analytical Output Structure
Presentation/
│
├── races_per_season/
│   └── Parquet files
│
└── races_by_country/
    └── Parquet files
races_per_season
Column	Description
season-	F1 championship season
race_count-	Number of races in that season

races_by_country
Column	Description
country	-Country where races were held
race_count -Number of races held in that country

📊 Power BI Dashboard
The analytical outputs are connected to Power BI to create an interactive reporting layer.

The dashboard is designed as a two-page report.

📄 Page 1 — F1 Racing: Season Overview

The Season Overview page provides insights into the historical F1 race calendar.

KPI Cards
●Total Races
●Total Seasons
●Average Races per Season
●Maximum Races in a Season

Visualizations
●Number of Races by Season
●F1 Race Calendar Growth by Season
●Season slicer

Example Layout
┌──────────────────────────────────────────────────────┐
│          F1 RACING - SEASON OVERVIEW                 │
│                                                      │
│  Total     Total      Average       Maximum          │
│  Races     Seasons    Races/Season  Races/Season     │
│                                                      │
│  ┌────────────────────┐  ┌────────────────────────┐  │
│  │ Races by Season    │  │ Season Filter          │  │
│  │                    │  │                        │  │
│  │ Column Chart       │  │ 1950                   │  │
│  │                    │  │ 1951                   │  │
│  └────────────────────┘  └────────────────────────┘  │
│                                                      │
│  ┌────────────────────────────────────────────────┐  │
│  │ F1 Race Calendar Growth by Season              │  │
│  │                  Line Chart                    │  │
│  └────────────────────────────────────────────────┘  │
└──────────────────────────────────────────────────────┘
🌍 Page 2 — F1 Racing: Global Overview

The Global Overview focuses on the geographical distribution of Formula 1 races.

KPI Cards
●Total Countries
●Total Races
●Average Races per Country
●Maximum Races per Country

Visualizations
●Number of F1 Races by Country
●World Map
●Country-level race distribution

Key Insights
The dashboard allows users to analyze:

●How the F1 race calendar has evolved over time.
●The number of races held in each season.
●Countries that have hosted the highest number of races.
●The geographical distribution of Formula 1 races.
●Historical growth in the number of races per season.

Suggested Project Structure
F1-Azure-Data-Engineering/
│
├── README.md
│
├
├── pipelines
├── datasets
├── linked-services
│
├── databricks/
│   ├──01_data_processing.py
│   ├── 02_transformation_f1.py
│   └── 03_analyze_f1.py
│
├── powerbi/
│   ├── F1_Racing_Dashboard.pbix
│   └── screenshots/
│
├── architecture/
│   └── architecture-diagram.png


🚀 Data Pipeline Workflow

The complete workflow can be summarized as:

Step 1 — Extract
Formula 1 data is retrieved from the REST API.

Step 2 — Ingest
Azure Data Factory copies the API data into Azure Data Lake Storage Gen2.

Step 3 — Store
Data is organized into Raw, Ingested, and Presentation layers.

Step 4 — Transform
Azure Data Factory and Azure Databricks transform the raw JSON data into structured datasets.

Step 5 — Analyze
PySpark is used to generate analytical datasets such as:

races_per_season
races_by_country

Step 6 — Visualize
Power BI connects to the analytical outputs and presents the results through interactive dashboards.

 Security & Cloud Practices

The project uses Azure cloud services for data storage, processing, and orchestration.

Sensitive credentials such as:
●Storage account keys
●API keys
●Access tokens
●Connection strings

should never be committed to GitHub.

Use:
.env

or Azure-managed authentication mechanisms where appropriate.

Add sensitive files to:

.gitignore

 Future Enhancements

The project can be extended with additional analytical datasets and dashboards.

Potential enhancements include:

●Driver performance analysis
●Constructor performance analysis
●Driver championship standings
●Points by driver and season
●Wins by driver
●Podium finishes
●Circuit performance analysis
●Driver nationality analysis
●Race location mapping using latitude and longitude
●Year-over-year race calendar analysis
●Incremental data ingestion
●Automated pipeline scheduling
●Azure Key Vault integration
●CI/CD for ADF and Databricks
●Advanced Power BI DAX measures

 Skills Demonstrated
This project demonstrates practical experience in:

Data Engineering
●ETL / ELT pipelines
●Data ingestion
●Data transformation
●Data lake architecture
●Data orchestration
●Parquet data processing

Azure
●Azure Data Factory
●Azure Data Lake Storage Gen2
●Azure Databricks
●Azure cloud architecture

Programming & Analytics
●Python
●PySpark
●Apache Spark
●SQL concepts
●Data aggregation
●Data transformation

Business Intelligence
●Power BI
●KPI development
●Interactive dashboards
●Data visualization
●Analytical reporting

 Project Outcome
This project demonstrates an end-to-end cloud data engineering workflow:

API
 ↓
Azure Data Factory
 ↓
Azure Data Lake Storage Gen2
 ↓
Azure Databricks
 ↓
PySpark Analytics
 ↓
Parquet Analytical Outputs
 ↓
Power BI
 ↓
Interactive F1 Dashboard

It showcases how raw API data can be transformed into structured, analytics-ready information and ultimately delivered as business-friendly dashboards.

👩‍💻 Author
Nandana



