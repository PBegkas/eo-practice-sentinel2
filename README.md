# Earth Observation Practice Project

A personal practice project for learning basic Earth Observation (EO) libraries and geospatial data manipulation using Python and Jupyter Notebooks.

## Overview

This project works with Sentinel-2 L2A satellite imagery, queried through the [Element84 STAC API](https://www.element84.com/earth-search/), to practice:

## Goal

Beyond building core EO skills, the longer-term goal of this project is to apply the pipeline to a real-world use case: studying **land cover change over time**, specifically:

- **Deforestation**, detecting vegetation loss in the AOI over a given period
- **Wildfire impact**, identifying burn scars and vegetation stress following fire events
- **Reforestation efforts**, tracking vegetation recovery/regrowth over subsequent seasons/years

This will build on the vegetation index and time-series work in Phases 4–5, extended with change-detection techniques.

This project practices:

- Defining an Area of Interest (AOI)
- Querying and filtering satellite imagery via STAC
- Extracting and manipulating raster bands
- Building multiband arrays and datacubes
- Surface reflectance conversion and contrast enhancement
- Working with Zarr for storing multidimensional array data
- Mosaicking/cropping tiles to an AOI
- Calculating vegetation indices (e.g. NDVI)
- Time-series analysis of vegetation indices across seasons

## Project Structure

```
.
├── EarthObservation.ipynb   # Jupyter notebooks
├── data/
│   ├── aoi/                 # AOI GeoJSON file(s)
│   ├── stac/                # Saved STAC search results (JSON)
│   └── datacube/            # Zarr-stored datacubes
├── README.md
└── requirements.txt
```


## Tools & Libraries

- **STAC / Data access:** `pystac-client` — query Element84 Earth Search
- **Raster & vector I/O:** `rioxarray`, `geopandas`, `shapely`
- **Data cubes:** `xarray`
- **Data handling:** `pandas`, `numpy`
- **Visualization:** `matplotlib`
- **AOI creation:** [geojson.io](https://geojson.io)

See `requirements.txt` for exact pinned versions

## Workflow / TODO

### Phase 1 — Setup & Data Discovery
- [x] Define AOI with geojson.io
- [x] Search Element84 STAC API for Sentinel-2 L2A scenes (AOI + 10-day window)
- [x] Save search results to JSON

### Phase 2 — Single Tile Exploration
- [x] Extract RGB bands from one tile
- [x] Build 3D multiband array (RGB)
- [x] Convert DN to surface reflectance
- [x] Apply contrast enhancement
- [x] Plot raw and enhanced arrays
- [x] Load RGB bands from collection items into a datacube (xarray)
- [x] Save datacube to Zarr

### Phase 3 — AOI-based Mosaic
- [ ] Reproject tiles to a common CRS if needed (check UTM zones across tiles)
- [ ] Merge/mosaic overlapping tiles covering the AOI
- [ ] Clip the mosaic to the exact AOI geometry (mask with the geojson polygon)
- [ ] Re-plot RGB to confirm the AOI-only crop looks correct
- [ ] Update/re-save the Zarr datacube to store the AOI-cropped version instead of per-tile data

### Phase 4 — Vegetation Index
- [ ] Extract additional bands needed (e.g. NIR for NDVI, or Red-Edge for NDRE)
- [ ] Add NIR (and any other required bands) into the datacube
- [ ] Calculate vegetation index
- [ ] Handle edge cases (cloud/no-data masking)
- [ ] Plot NDVI with an appropriate colormap
- [ ] Sanity-check values against known vegetation/water/urban areas in the AOI

### Phase 5 — Temporal Analysis
- [ ] Query STAC for a wider time window (e.g. full growing season or full year)
- [ ] Filter for cloud cover threshold to drop unusable scenes
- [ ] Batch-download/process the expanded set of scenes into the datacube (time dimension)
- [ ] Compute NDVI for each timestamp
- [ ] Aggregate/resample if needed (e.g. monthly composites, handle gaps)
- [ ] Plot NDVI time series (mean over AOI, or per-pixel animation/facet grid)
- [ ] Compare seasonal patterns — identify peak greenness, senescence, etc.

### Phase 6 — Change Detection (Deforestation / Wildfire / Reforestation)
- [ ] Select before/after time periods relevant to a known event or general monitoring window
- [ ] Calculate NBR (Normalized Burn Ratio) for wildfire/burn scar detection, if SWIR band available
- [ ] Compute NDVI (or NBR) difference between time periods
- [ ] Threshold the difference to flag significant vegetation loss (deforestation/fire) or gain (reforestation)
- [ ] Plot change map highlighting loss/gain areas over the AOI
- [ ] Cross-check flagged areas against known events (fire perimeters, land-use records) if available
- [ ] Summarize findings (area affected, recovery trend over time, etc.)

### Optional Stretch Goals
- [ ] Compare NDVI against another index for cross-validation
- [ ] Export final time series as CSV/plot for a short write-up

## Status

Work in progress, currently working on Phase 3 (AOI-based mosaic).
