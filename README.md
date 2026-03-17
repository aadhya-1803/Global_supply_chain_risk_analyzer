# Global_supply_chain_risk_analyzer
This is a data analytics project that monitors macroeconomic and logistics indicators across over 150 countries to generate a composite supply chain disruption risk score that helps procurement teams identify high-risk supplier countries before disruptions occur.
# Why This Project
Most supply chain tools are reactive. By the time a shipment is delayed or a supplier fails, the damage is already done. This project takes a different approach: using publicly available government data to score supplier country risk proactively, so that sourcing decisions can be made with a clearer picture of where risk is building.
Countries like Sudan, Burkina Faso, and Iran all showed measurable deterioration in their risk indicators between 2018 and 2023, well before those signals would have reached a procurement desk. The model captures exactly that kind of early movement.
# Dashboard Preview
<img width="1021" height="562" alt="image" src="https://github.com/user-attachments/assets/44213796-26af-40fb-8ced-14a6ab15b4b5" />

# Key Findings
1. Highest risk country (2023) - Sudan — risk score 93.12, political stability index of -2.47.
2. Sharpest deterioration - (2018–2023)Sudan (+14.31 pts), Burkina Faso (+13.98), Iran (+13.37).
3. Most consistently high risk - Afghanistan with an average risk score of 88.35 across 13 years.
4. Critical risk countries (2023) - Sudan, Afghanistan, Libya - all scoring above 85.
5. High risk countries (2023) - 17 countries flagged above the 70-point threshold.
6. Lowest logistics performance - Afghanistan and Libya both scored 1.9 on the LPI vs. global average of 2.94.

# Tech Stack
1. Python- Numpy, Pandas
2. SQLite- structured storage and analytical querying
3. SQL - Window Functions and Querying
4. Power BI- Interactive Dashboard and geographic risk visualization
5. Google colab- development environment

# Data Source
All data is sourced from official government and intergovernmental organizations. No synthetic datasets were used for the core risk model.
1. World Bank Worldwide Governance Indicators (WGI) — Political Stability (PV.EST) and Government Effectiveness (GE.EST), 2011–2023.
2. World Bank World Development Indicators (WDI) — Inflation (FP.CPI.TOTL.ZG) and GDP Growth (NY.GDP.MKTP.KD.ZG), 2011–2023
3. World Bank Logistics Performance Index (LPI) — Overall LPI score by country, 2007–2023.

# Risk Score Formula
Each indicator was normalized to a 0–1 range using min-max scaling. Governance and logistics scores are inverted so that lower performance maps to higher risk. The final score is scaled to 0–100.
Risk Score = (1 - normalize(PV.EST))  × 0.35
           + (1 - normalize(GE.EST))  × 0.25
           + (1 - normalize(LPI))     × 0.30
           + normalize(Inflation Volatility) × 0.10
Inflation volatility is calculated as a rolling 3-year standard deviation per country rather than the raw inflation rate, which captures how erratic a country's price environment has been over time.

# SQL analytics performed
Four analytical queries are written and executed directly in the notebook:
1. Window function - ranks every country by risk score within each year using RANK() OVER (PARTITION BY year ORDER BY risk_score DESC)
2. CTE - identifies countries with the sharpest risk deterioration between 2018 and 2023 by joining two subqueries on country code
3. Aggregation - calculates average, minimum, and maximum risk score per country across all years to surface chronically high-risk regions
4. CASE statement - classifies every country-year into Critical (85+), High (70–85), Moderate (50–70), or Low (<50) risk tiers

# Limitations
The WGI governance indicators are perception-based scores derived from expert surveys, not hard measurements, and carry a reporting lag. The LPI is published every two years, so odd-year values are forward-filled from the most recent survey. The risk score weights are assigned based on domain research rather than derived statistically.
A natural extension would be to learn optimal weights from historical disruption event data using regression, and to integrate real-time news sentiment signals from sources like ACLED or GDELT.

Author
aadhya-1803 — github.com/aadhya-1803
