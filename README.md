🧠 BRFSS ETL & Mental Health Analysis Dashboard
This project aims to process and analyze the 2023 BRFSS dataset (Behavioral Risk Factor Surveillance System) to uncover insights into mental health trends across the U.S. It enriches survey responses with historical weather data and visualizes mental health risk using Power BI.

🧩 Challenges Faced
Large Dataset (1.12 GB): Handling and processing such a large dataset was resource-intensive.

SAS File Format: The original dataset was in .xpt SAS format, which is not easily compatible with most tools.

System Limitations: On systems with low RAM, direct processing of the full dataset wasn’t feasible.

Historical Weather Data: Fetching weather data using APIs was expensive due to limited free API call quotas.

✅ Solutions Implemented
Chunked Processing:

Created a script to split the large .xpt SAS file into three CSV chunks.

These chunks were hosted on Hugging Face Datasets (instead of GitHub) due to GitHub's file size limitations.

ETL Breakdown:

Each chunk is processed via its own extract, transform, and load step.

This approach ensures memory-efficient processing and pipeline modularity.

Weather Integration:

Used the WorldWeatherOnline API (100 free calls/day) to pull historical weather data.

Weather data was then merged with the health data on a per-state basis.

🔧 Transformations Applied
Referencing transform.py, here are key transformations:

Categorical Mapping: Raw integer codes were mapped to meaningful labels for fields like MENTHLTH, EMPLOY1, INCOME3, GENHLTH, etc.

Missing Value Handling:

Rows with ambiguous values like Don't know/Refused were removed.

Nulls in income were filled using the mode.

Mental health (MENTHLTH) nulls were filled using the median.

Weather Data Merging: Pivoted 7-day historical weather (temp, humidity, wind, type) and merged by State.

Feature Engineering:

Created binary PoorMentalHealth flag (days ≥ 14).

Categorized mental health into High Distress, Moderate, and Low.

🗂️ Folder Structure
graphql
Copy
Edit
.
├── flows/                        # Kestra workflow YAMLs
├── dataset/                     # Dataset chunks and weather data
├── scripts/
│   ├── extract1.py              # Extract chunk 1
│   ├── extract2.py
│   ├── extract3.py
│   ├── transform.py             # Transform logic used for all chunks
│   ├── load.py                  # Loads CSV to PostgreSQL
│   ├── get_weather_data.py
│   └── split_dataset.py
├── BRFSS_Analysis.pdf           # Final documentation
├── BRFSS_ETL.ipynb              # Jupyter notebook for exploration
├── brfss_analysis.pbix          # Power BI dashboard
├── all sql cmnds.sql            # SQL commands
├── Dockerfile                   # Custom Kestra image with Python deps
├── docker-compose.yml           # Kestra + PostgreSQL setup
├── README.md                    # You are here!
└── .gitignore                   # Ignores dataset folder
▶️ How to Run
🔄 Start Kestra + PostgreSQL
bash
Copy
Edit
docker-compose up --build
Then visit http://localhost:8080 to access the Kestra UI.

📊 Start Metabase (Optional Analytics Layer)
bash
Copy
Edit
docker run -d -p 3000:3000 --name metabase metabase/metabase
Open http://localhost:3000 to set up and connect to your PostgreSQL.

📈 Visualizations Built in Power BI
Heatmaps of Poor Mental Health % by State

Time Series Risk Scores using rolling averages

Income vs. Mental Health correlation

Filters for Gender, Race, Income

Weather-Linked Risk Indicators

<!-- Replace this with a real image from your Power BI -->

