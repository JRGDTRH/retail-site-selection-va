# Retail Expansion & Climate-Resilient Merchandising Case Study

## Overview
This repository features an advanced spatial data science notebook (`retail_expansion_and_climate_case_study.ipynb`) that merges demographic data, competitive landscapes, and climate vulnerability indexes into a dual-purpose operations and real estate spatial model. Focused on Virginia, the tool provides dynamic suitability scoring for retail expansion (e.g., Dollar stores) while planning for climate resilience.

---

## Case Study Breakdown

### 1. The Challenge
Modern retail expansion teams cannot rely solely on traditional demographics. They must balance population density, income, and market saturation (competitors) with emerging external risks, such as climate vulnerability. Furthermore, operations teams need to know which *existing* stores face higher risks to proactively adjust merchandising (e.g., stocking up on cooling supplies). This project bridges the gap between real estate strategy and climate-resilient operations.

### 2. Data Sourcing & Engineering (Phases 1 & 2)
The notebook operates completely hands-off by programmatically sourcing and harmonizing data from modern cloud and API endpoints:
- **Census Geometries:** Fetched on-the-fly via `pygris` (Virginia Census Tracts).
- **Demographics:** Population and Median Household Income queried directly from the Virginia Open Data Portal.
- **USDA Food Access:** Automatically downloaded and extracted to map recognized "food desert" tracts.
- **Climate Vulnerability (FEMA):** Queries ArcGIS REST services for FEMA's National Risk Index, extracting specific Heat Wave Risk scores by tract.
- **Competitor Landscape (Overture Maps):** Leverages DuckDB with the `httpfs` and `spatial` extensions to query cloud-native Parquet files directly from S3. It filters millions of global points of interest down to a localized bounding box, extracting competing "Dollar" brands and isolating "Family Dollar" locations.

### 3. Spatial Analytics & Normalization (Phase 3)
To enable accurate physical distance calculations, the data is projected to a localized coordinate reference system (Virginia State Plane - EPSG:3968). 
- **Competitor Density:** DuckDB Spatial SQL is used to calculate the number of competing stores and existing sister stores within a 5km radius of every census tract.
- **Min-Max Scaling:** All raw metrics (population, income, competitor counts, heat risk, food deserts) are normalized on a 0 to 1 scale. This mathematical standardization is necessary to feed into the weighted suitability model later.

### 4. Operational & Merchandising Insights (Phase 4)
Using point-in-polygon spatial joins, existing Family Dollar locations are mapped precisely into the scored census tracts. 
- **Heat Wave Inventory Tiers:** Stores are dynamically assigned to tiers (Tier 1: Critical, Tier 2: Moderate, Tier 3: Low) based on their hyper-local FEMA risk score. 
- **Business Impact:** This allows supply chain and merchandising teams to proactively allocate climate-specific inventory (fans, water, coolers) ahead of summer heat waves, turning risk data into a revenue and customer-retention driver.

### 5. Interactive Real Estate Dashboard (Phase 5)
The climax of the notebook is a dynamic `folium` map embedded directly within Jupyter using `ipywidgets`.
- **Weighted Linear Combination (WLC):** Real estate teams can adjust visual sliders (e.g., Population Weight, Competition Penalty, Heat Risk Weight) to dynamically recalculate a "Suitability Score" for every tract in real-time.
- **Visual Intelligence:** The map overlays competitor locations and existing stores, allowing the team to visually pinpoint highly suitable tracts that are unserved by current locations but feature favorable demographics, high food-desert scores (indicating need), and acceptable climate risks.

---

## Setup and Installation

### Prerequisites
Ensure you have Python 3.9+ installed. You will need the following libraries:
- `pandas`
- `geopandas`
- `duckdb`
- `folium`
- `pygris`
- `requests`
- `ipywidgets`
- `shapely`
- `branca`

### Running the Project
1. Clone the repository or download the project files.
2. Install the dependencies:
   ```bash
   pip install pandas geopandas duckdb folium pygris requests ipywidgets shapely branca
   ```
3. Open Jupyter Notebook or JupyterLab:
   ```bash
   jupyter notebook
   ```
4. Run the `retail_expansion_and_climate_case_study.ipynb` notebook. The widgets and map will generate dynamically at the bottom of the notebook. Note that some data sources (like FEMA and USDA) may take a few moments to download and cache locally on the first run.
