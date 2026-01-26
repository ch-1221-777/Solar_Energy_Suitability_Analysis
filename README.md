### Solar Energy Suitability Analysis  

#### Project Overview

This project explores the **suitability of commercial and industrial buildings in Los Angeles for rooftop solar energy deployment**.  
Using official City of Los Angeles geospatial datasets, the analysis identifies **large building footprints** that fall within 
**commercial and industrial zoning areas**, serving as a first screening for potential solar installations.

---

#### Objectives
- Identify **commercial and industrial zoning areas** within Los Angeles  
- Extract **building footprints** intersecting those zones  
- Filter buildings by **minimum rooftop size** suitable for solar installation  
- Produce a clean, reproducible geospatial dataset for further analysis and mapping  

---

#### Data Sources

#### 1. Building Footprints  
**Source:** Los Angeles GeoHub (ArcGIS REST API)  
**Layer:** Building Footprints (LARIAC5)

**Key attributes used:**
- Geometry (polygon footprints)  
- `AREA` (building footprint area)  

**Access method:**
- ArcGIS REST API queries with pagination using the requests python module 
- Geometry returned in WGS84 (EPSG:4326) and reprojected as needed  

---

#### 2. Zoning Data  
**Source:** data.lacity.org (Socrata / SODA API)

**Key attributes used:**
- `zone_category`  
- `zone_cmplt`  
- Geometry (zoning polygons)  

**Filtering criteria:**
- Commercial zones  
- Industrial zones
  
**Access method:**
- Socrata/SODA3 API query using the requests python module 
- Geometry returned in WGS84 (EPSG:4326) and reprojected as needed 
---

#### Methodology

#### Step 1: Zoning Filter  
Zoning polygons were filtered to include **commercial and industrial categories only**, forming the initial constraint layer for the analysis.

<p align="center">
  <img src="figures/LA_commercial_zoning.png" alt="Commercial & Industrial Zoning — Los Angeles" width="500">
</p>

#### Step 2: Building Footprint Extraction  
Building footprints were queried from the ArcGIS REST API using bounding boxes and pagination method to handle the large dataset. 
The first bounding box encompasses the area of DTLA, where we extract all building footprints intersecting this box.
<p align="center">
  <img src="figures/LA_bldg_full_test_bbox.png" alt="DTLA Building Footprints" width="500">
</p>
Next, extract data interseting bounding boxes for San Fernando Valley and Long Beach areas.   

#### Step 3: Spatial Intersection  
Building footprints were **spatially intersected with commercial/industrial zoning polygons**, retaining only buildings located within these zones.

<p align="center">
  <img src="figures/large_comm_ind_LA_bldg_full_test_bbox.png" alt="DTLA Industrial/Commercial Building Footprints" width="500">
</p>

#### Step 4: Size Threshold Filtering  
To approximate rooftop solar feasibility, buildings were filtered by footprint area:

- **Minimum area:** 10,000 square feet  
- Equivalent to approximately **929 m²**

---

#### Results

- **Total commercial/industrial buildings identified:** 5,913  
- **Buildings ≥ 10,000 sq ft:** 4,241  

The resulting dataset represents **large commercial and industrial structures** that meet both zoning and size criteria, making them strong candidates for further solar suitability analysis.

---

#### Coordinate Reference Systems

- **Source CRS:** EPSG:4326 (WGS 84)  
- **Analysis / Mapping CRS:** EPSG:3857 (Web Mercator)  

Data were stored in **GeoPackage format** to preserve geometry fidelity and CRS information.

---

#### Outputs

- `LA_commercial_industrial_zoning_4326.gpkg`  
- `large_buildings_comm_industrial_full_test_bbox.gpkg`  

These files can be directly loaded into:
- QGIS  
- ArcGIS Pro  
- GeoPandas workflows  

---

##### Next Steps

- Estimate rooftop solar potential using area and orientation  
- Integrate solar irradiance or LiDAR data  
- Produce neighborhood-level solar suitability maps  

---
