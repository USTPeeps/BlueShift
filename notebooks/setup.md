# BlueShift Data Setup

This guide explains how to prepare the Python environment and how the BlueShift data acquisition and preprocessing workflow is organized.

## 1. Project structure

The project separates source data from processed outputs. Keep downloaded or otherwise unmodified source files in `data/raw/`, and place cleaned, aggregated, or analysis-ready datasets in `data/processed/`.

```text
BlueShift/
├── data/
│   ├── raw/
│   │   ├── phl_admin_boundaries/
│   │   └── psa/
│   └── processed/
│       ├── nasa/
│       ├── ocean_zones/
│       └── psa/
├── notebooks/
│   ├── 01_zone_generation.ipynb
│   ├── 02_fisheries_preprocessing.ipynb
│   ├── 03_download_nasa_data.ipynb
│   ├── 04_aggregate_nasa_data.ipynb
│   └── 05_final_data_integration.ipynb
└── setup.md
```

### Folder conventions

| Folder | Contents |
|---|---|
| `data/raw/phl_admin_boundaries/` | Original Philippine administrative boundary files used to define and spatially reference the study areas. |
| `data/raw/psa/` | Original fisheries production data obtained from the Philippine Statistics Authority (PSA). |
| `data/processed/ocean_zones/` | Generated ocean-zone geometries and related spatial outputs used to associate marine areas with provinces. |
| `data/processed/psa/` | Cleaned and standardized fisheries production data prepared for analysis and integration. |
| `data/processed/nasa/` | Downloaded NASA data after spatial extraction and temporal aggregation, ready for integration with fisheries data. |

Keep raw files unchanged. If a source file needs cleaning or transformation, write the result to the corresponding `processed` folder.

## 2. Environment setup

BlueShift uses **Python 3.12** and the Conda package manager. We recommend installing [Miniforge](https://github.com/conda-forge/miniforge), which provides Conda with the conda-forge package channel.

### Step 1 — Install Miniforge

Download and install the Miniforge distribution for your operating system from:

<https://github.com/conda-forge/miniforge>

After installation, open the Miniforge Prompt (Windows) or a terminal where Conda is available.

### Step 2 — Create the project environment

Create an isolated environment named `blueshift`:

```bash
conda create -n blueshift python=3.12 -y
```

Activate it:

```bash
conda activate blueshift
```

### Step 3 — Install the project libraries

Install the required packages from conda-forge:

```bash
conda install -c conda-forge -y geopandas shapely rasterio scipy pyproj matplotlib xarray netCDF4 h5netcdf regionmask pandas numpy pyogrio earthaccess jupyterlab ipykernel notebook
```

### Step 4 — Register the Jupyter kernel

Register the environment so it can be selected from Jupyter:

```bash
python -m ipykernel install --user --name blueshift --display-name "Python (BlueShift)"
```

Launch Jupyter Notebook:

```bash
jupyter notebook
```

In each project notebook, select the **Python (BlueShift)** kernel.

## 3. Python libraries used

| Library | Purpose in BlueShift |
|---|---|
| `geopandas` | Reads, manages, transforms, and analyzes Philippine administrative boundaries and ocean-zone vector data. |
| `shapely` | Creates and manipulates geometric objects, including points, polygons, and spatial intersections. |
| `rasterio` | Reads and works with geospatial raster data when raster-based spatial operations are needed. |
| `scipy` | Provides scientific-computing utilities used for numerical and scientific data processing. |
| `pyproj` | Handles coordinate reference systems and coordinate transformations. |
| `matplotlib` | Produces plots and visual checks for spatial, temporal, and tabular data. |
| `xarray` | Opens and processes labeled multidimensional NASA datasets, including gridded data with time and spatial dimensions. |
| `netCDF4` | Supports reading NetCDF data files commonly used for Earth science datasets. |
| `h5netcdf` | Provides an additional backend for reading and writing NetCDF files stored using HDF5. |
| `regionmask` | Applies geographic region masks to gridded datasets so data can be summarized over defined ocean zones or regions. |
| `pandas` | Cleans, transforms, groups, aggregates, and joins tabular fisheries and environmental data. |
| `numpy` | Provides arrays and numerical operations used throughout the data-processing workflow. |
| `pyogrio` | Provides fast reading and writing of vector geospatial formats; also serves as a GeoPandas I/O engine. |
| `earthaccess` | Authenticates with and searches/accesses NASA Earth science data services for supported datasets. |
| `jupyterlab` | Provides the interactive notebook environment used to run the project workflow. |
| `ipykernel` | Makes the `blueshift` Conda environment available as a Jupyter kernel. |
| `notebook` | Provides the classic Jupyter Notebook interface and notebook runtime components. |

## 4. Data acquisition and preprocessing workflow

Run the notebooks in numerical order. Each notebook performs one stage of the pipeline and prepares data for the next stage.

### Notebook 01 — `01_zone_generation.ipynb`

**Goal:** Generate the ocean zones used to connect marine environmental measurements with Philippine provinces.

1. Load the Philippine administrative boundary data from `data/raw/phl_admin_boundaries/`.
2. Inspect the boundary layers and their coordinate reference systems.
3. Prepare the relevant coastal/provincial geometries for spatial processing.
4. Generate the ocean-zone geometries associated with the provinces.
5. Save the resulting spatial outputs under `data/processed/ocean_zones/`.

The generated zones provide the spatial framework for extracting or summarizing NASA ocean data by province.

### Notebook 02 — `02_fisheries_preprocessing.ipynb`

**Goal:** Prepare PSA fisheries production records for integration with the NASA environmental data.

1. Load the original fisheries data from `data/raw/psa/`.
2. Inspect the source columns, reporting periods, province names, species, and production values.
3. Clean and standardize the fields used in the analysis.
4. Convert or derive the time fields needed for quarterly analysis.
5. Standardize province and species labels and handle invalid or missing records as required by the source data.
6. Aggregate the fisheries records to the project’s analysis level.
7. Save the prepared fisheries dataset under `data/processed/psa/`.

The prepared fisheries data provides the production outcome used alongside the environmental variables.

### Notebook 03 — `03_download_nasa_data.ipynb`

**Goal:** Acquire the NASA ocean datasets used to derive environmental indicators.

1. Use the NASA data access workflow in the notebook (including `earthaccess` where applicable) to search for and access the selected datasets.
2. Retrieve the required spatial and temporal coverage for the study area.
3. Work with the downloaded scientific data files, such as NetCDF datasets.
4. Keep source downloads organized in the NASA data area and retain the original downloaded files before further processing.

The environmental variables used in the integrated dataset include **chlorophyll-a concentration** and **sea surface temperature (SST)**. Dataset selection, product identifiers, date ranges, and download parameters should be taken from the executed notebook so that the acquisition is reproducible.

### Notebook 04 — `04_aggregate_nasa_data.ipynb`

**Goal:** Convert the gridded NASA observations into province-level quarterly indicators.

1. Open the NASA multidimensional datasets with the relevant NetCDF/xarray tools.
2. Apply the project’s spatial framework using the generated ocean zones.
3. Extract or summarize valid observations for each zone and time period.
4. Calculate the required spatial summaries for chlorophyll-a and sea surface temperature.
5. Aggregate the observations from monthly data to quarterly values.
6. Track valid observation counts and missing months so that incomplete coverage can be identified.
7. Save the aggregated NASA outputs under `data/processed/nasa/`.

The notebook should retain the observation-count or coverage information used to assess whether each quarterly value is adequately supported by the source observations.

### Notebook 05 — `05_final_data_integration.ipynb`

**Goal:** Combine the processed fisheries and NASA datasets into one analysis-ready table.

1. Load the processed fisheries dataset from `data/processed/psa/`.
2. Load the aggregated NASA environmental dataset from `data/processed/nasa/`.
3. Standardize the join keys and align the time periods, province identifiers/names, and other required dimensions.
4. Join the fisheries production records with the corresponding quarterly environmental indicators.
5. Inspect unmatched records, missing values, duplicate keys, and data types.
6. Export the final integrated dataset for downstream analysis.

The final integrated table uses the following fields:

| Column | Description |
|---|---|
| `Year` | Calendar year of the observation. |
| `Quarter` | Quarter represented by the record. |
| `Province` | Philippine province associated with the fisheries and ocean-zone data. |
| `Species` | Fisheries species/category represented by the production record. |
| `Chlorophyll-A Concentration (mg/m³)` | Aggregated NASA chlorophyll-a indicator for the corresponding province and quarter. |
| `Sea Surface Temperature (°C)` | Aggregated NASA sea surface temperature indicator for the corresponding province and quarter. |
| `Yield (Metric Tons)` | Fisheries production/yield value from the processed PSA data. |

## 5. Recommended execution order

Run the notebooks from top to bottom, using the **Python (BlueShift)** kernel:

| Order | Notebook | Main output |
|---:|---|---|
| 1 | `01_zone_generation.ipynb` | Processed ocean-zone spatial data |
| 2 | `02_fisheries_preprocessing.ipynb` | Cleaned and aggregated PSA fisheries data |
| 3 | `03_download_nasa_data.ipynb` | NASA source data downloads |
| 4 | `04_aggregate_nasa_data.ipynb` | Province-level quarterly NASA indicators |
| 5 | `05_final_data_integration.ipynb` | Final integrated fisheries and environmental dataset |

**Important:** Notebooks 01 and 02 establish the spatial and fisheries inputs; Notebook 03 acquires the NASA source data; Notebook 04 derives the quarterly environmental indicators; and Notebook 05 integrates the datasets. If an upstream notebook changes, rerun the downstream notebooks that depend on its outputs.

## 6. Reproducibility notes

- Run the notebooks in the order listed above.
- Keep original source files in `data/raw/` and write transformed outputs to `data/processed/`.
- Do not manually edit generated intermediate files unless the change is documented and can be reproduced.
- Confirm the selected Jupyter kernel is **Python (BlueShift)** before running cells.
- NASA access may require account authentication or acceptance of dataset-specific access conditions. Follow the authentication steps documented in the data-download notebook.
- Record the dataset product names, temporal coverage, spatial bounds, and processing parameters in the relevant notebook whenever they are changed.
