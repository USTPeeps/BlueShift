# BlueShift
### Be An Earth System Trend Detective!

*Understanding the relationship between changing ocean conditions and Philippine fisheries production.*

## The Problem

The Philippines is an island nation that depends heavily on fisheries for food, livelihoods, and economic activity. However, local fisheries operate in an environment where ocean conditions are constantly changing.

Rising sea-surface temperatures and variations in ocean productivity can influence marine ecosystems and fishing conditions. Understanding how these environmental changes relate to local fisheries production is important for researchers, fisheries managers, and policymakers.

Yet, satellite-derived ocean observations and fisheries production statistics are often examined separately, making it difficult to explore their relationships across different locations, species, and time periods.

## Our Research Question

**How are changing ocean conditions associated with local fisheries production across different Philippine regions, provinces, and species?**

## Our Solution: BlueShift

BlueShift is an interactive Earth-observation and fisheries analytics platform that combines NASA satellite observations with Philippine Statistics Authority (PSA) fisheries statistics.

It allows users to explore ocean conditions, track local fisheries production, and investigate how environmental variables relate to changes in fish production over time.

## How It Works

1. **Monitor the Oceans:** Use NASA satellite observations to track sea-surface temperature (SST) and chlorophyll-a concentration, an indicator of ocean productivity.

2. **Track Local Fisheries:** Explore PSA fisheries production statistics by region, province, subsector, species, and time period.

3. **Explore Relationships:** Compare ocean conditions with fisheries production using statistical analysis, correlation measurements, and visualizations.

4. **Discover Regional Insights:** Identify geographic and temporal patterns in ocean conditions and fisheries production across the study area.

## What Users Can Explore

### 1. Ocean Monitor
- View spatial distributions of sea-surface temperature and chlorophyll-a.
- Explore environmental trends across selected locations and time periods.
- Examine changes in ocean temperature and productivity.

### 2. Fisheries Tracker
- View total fisheries production (metric tons) by province, species, and subsector.
- Compare annual and quarterly production trends.
- Identify top-producing species and compare production across locations.

### 3. Ocean-Fisheries Relationship Explorer
- Select a province, species, and environmental variable (SST or chlorophyll-a).
- Compare environmental trends with fish production over the same period.
- Examine Pearson and Spearman correlation coefficients.
- View scatterplots, trend lines, and statistical summaries.
- Explore potential lagged relationships between ocean conditions and fisheries production.

### 4. Regional Insights
- Explore an interactive map of Mindanao.
- Compare environmental conditions and fisheries production across provinces.
- Identify areas with notable environmental changes alongside changes in fisheries production.

*All statistical relationships are exploratory associations and should not automatically be interpreted as evidence of causation.*

## Initial Study Area: Mindanao

BlueShift will initially focus on Mindanao, using available NASA satellite observations and compatible PSA fisheries statistics. This provides a starting point for exploring regional patterns and relationships before potentially expanding to other Philippine fishing regions.

## Expected Impact

BlueShift aims to make environmental and fisheries data more accessible and easier to understand. By bringing these datasets together, the platform can support researchers, fisheries managers, and policymakers in investigating how changing ocean conditions are associated with local fisheries production.

## Our Vision

**Better understanding of our oceans. Stronger local fisheries. More resilient fishing communities.**

BlueShift empowers users to become Earth System Trend Detectives by turning environmental and fisheries data into insights that can support informed decision-making.

---

## Data Sources and Datasets

BlueShift integrates satellite-derived ocean observations, official fisheries production statistics, and geographic boundary data to support its analysis and visualizations.

### 1. NASA Earth Observation Data

Ocean environmental variables are obtained from NASA's Aqua Moderate Resolution Imaging Spectroradiometer (Aqua MODIS) satellite products.

| Predictor | Satellite Product | Variable | Resolution | Access |
|---|---|---|---|---|
| Chlorophyll-a | Aqua MODIS Level-3 Chlorophyll-a | `chlor_a` | 4 km | NASA Earthdata Cloud |
| Sea-Surface Temperature (SST) | Aqua MODIS Level-3 SST Thermal Monthly, Daytime | `sst` | 4 km | NASA PO.DAAC Earthdata Cloud |

**Product identifiers:**

- **Chlorophyll-a**
  - Short name: `MODISA_L3m_CHL`
  - Product code: `CHL.chlor_a.4km`
  - Satellite: `AQUA_MODIS`
  - [NASA Earthdata – Aqua MODIS L3 Chlorophyll-a](https://www.earthdata.nasa.gov/data/catalog/ob-cloud-modisa-l3m-chl-2022.0)

- **Sea-Surface Temperature**
  - Short name: `MODIS_AQUA_L3_SST_THERMAL_MONTHLY_4KM_DAYTIME_V2019.0`
  - Product code: `SST.sst.4km`
  - Satellite: `AQUA_MODIS`
  - [NASA PO.DAAC – Aqua MODIS L3 SST](https://www.earthdata.nasa.gov/data/catalog/pocloud-modis-aqua-l3-sst-thermal-monthly-4km-daytime-v2019.0-2019.0)

### 2. Philippine Statistics Authority (PSA)

Fisheries production data are sourced from the PSA OpenSTAT database.

| Dataset | Description | Source |
|---|---|---|
| Fisheries – Volume of Production by Species, Geolocation, Year and Quarter | Fisheries production statistics, measured in metric tons, categorized by species, geographic location, year, and quarter. | [PSA OpenSTAT](https://openstat.psa.gov.ph/Database/Economic-and-Financial-Statistics/Agriculture-Forestry-Fisheries) |

This dataset provides the fisheries production records used to track changes in local fish production and examine their relationships with ocean conditions.

### 3. Geographic Data

Administrative boundary data are used to support geographic visualization, mapping, and the association of satellite observations with Philippine administrative areas.

| Dataset | Description | Source |
|---|---|---|
| Philippines – Subnational Administrative Boundaries | Geographic boundaries for Philippine administrative divisions, including regions and provinces. | [Humanitarian Data Exchange (HDX)](https://data.humdata.org/dataset/cod-ab-phl) |

**Data integration:** NASA satellite observations provide the environmental measurements, PSA supplies fisheries production statistics, and administrative boundaries provide the geographic reference for mapping and regional comparisons. These datasets are aligned by geographic area and time period for exploratory analysis.
