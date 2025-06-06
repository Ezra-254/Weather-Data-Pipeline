# Weather-Data-Pipeline
This project is an Apache Airflow DAG that automates fetching daily weather data for Nairobi (city ID: 184745) from the OpenWeatherMap API and storing it in a PostgreSQL database. The pipeline collects metrics like temperature, humidity, weather description, and wind speed, saving them with timestamps for data analysis.

It solves the common challenge of continuously ingesting and organizing external weather data in a structured, queryable format — enabling data-driven decisions for sectors such as agriculture, logistics, and planning.

# Objectives

- Automate data ingestion from a weather API
- Clean and transform the raw data
- Store structured weather data in PostgreSQL
- Schedule the pipeline to run reliably using Apache Airflow
- Enable future analytics and dashboarding on historical weather trends

# Tech Stack
-Apache Airflow – Orchestrating the workflow
-Python – Writing tasks
-PostgreSQL – Storing the data
-Requests – Fetching data from the weather API
-Psycopg2 – PostgreSQL integration with Python

# Features
-Fetches weather data from OpenWeatherMap API
-Stores data in a PostgreSQL table (weather_data)
-Runs daily at midnight (@daily schedule)
-Supports retries and email notifications on failure
-Uses XCom to pass data between tasks

# Project Structure
weather-data-pipeline/
├── dags/
│   └── ezra_weather_pipeline.py  # DAG for fetching and storing weather data
├── requirements.txt             # Python dependencies
├── README.md                   # Project documentation


# 🚀 Setup Instructions.
### 1. Clone the Repository
git clone https://github.com/yourusername/weather_pipeline.git
cd weather_pipeline
## 2. Create and Activate a Virtual Environment
python -m venv venv
source venv/bin/activate  # or venv\Scripts\activate on Windows
## 3. Install Dependencies
pip install -r requirements.txt
## 4. Configure Environment Variables 
WEATHER_API_KEY=your_api_key_here
POSTGRES_HOST=your_db_host
POSTGRES_DB=your_db_name
POSTGRES_USER=your_db_user
POSTGRES_PASSWORD=your_db_password
## 5. Set Up Airflow
airflow db init
## 6. Start Airflow
airflow db init
airflow webserver --port 8080 # --port 8081 for windows
airflow scheduler
Access the UI at http://localhost:8081.

# 🧠 How It Works
## Task 1: fetch_weather
Uses the OpenWeatherMap API to get weather data for a city (e.g. city ID 184745).
Extracts:
City name
Temperature (°C)
Humidity (%)
Weather description
Wind speed (m/s)
Pushes the data via Airflow’s XCom for downstream tasks.

## Task 2: store_weather
Pulls the data from XCom.
Connects to PostgreSQL.
Creates the weather_data table if it doesn’t exist.
Inserts the new weather data record.

# 🛢️ PostgreSQL Table Schema
CREATE TABLE IF NOT EXISTS weather_data (
    id SERIAL PRIMARY KEY,
    city TEXT,
    temperature REAL,
    humidity INTEGER,
    weather TEXT,
    wind_speed REAL,
    timestamp TIMESTAMPTZ DEFAULT CURRENT_TIMESTAMP
);

# 🕒 Scheduling
-The DAG is configured to run daily:schedule_interval='@daily'
-It also supports retries on failure with:
'retries': 2,
'retry_delay': timedelta(minutes=2)

#  Contact
**Author:** Ezra Kipkoech
📧 ezrahkipkoech@gmail.com

# License
This project is licensed under the MIT License.














