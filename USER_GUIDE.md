# Global Tectonics User Guide

## Table of Contents

1. [Introduction](#introduction)
2. [Getting Started](#getting-started)
3. [Software Requirements](#software-requirements)
4. [Loading the Data](#loading-the-data)
5. [Basic Workflows](#basic-workflows)
6. [Advanced Applications](#advanced-applications)
7. [Working with Different Formats](#working-with-different-formats)
8. [Visualization and Styling](#visualization-and-styling)
9. [Common Use Cases](#common-use-cases)
10. [Troubleshooting](#troubleshooting)
11. [Contributing to the Project](#contributing-to-the-project)
12. [FAQ](#faq)

---

## Introduction

Welcome to the **Global Tectonics** user guide. This document provides step-by-step instructions for working with the global tectonic and geologic province datasets. Whether you're a student, researcher, or professional in Earth sciences, this guide will help you effectively utilize these datasets for your projects.

### What You'll Learn
- How to load and visualize tectonic data in various GIS software
- How to perform spatial and attribute queries
- How to integrate tectonic data with your own datasets
- How to create publication-quality maps
- How to contribute improvements to the dataset

### Prerequisites
- Basic familiarity with GIS concepts (layers, attributes, coordinate systems)
- Access to GIS software (free options available)
- Basic understanding of plate tectonics (recommended)

---

## Getting Started

### Quick Start (5 minutes)

1. **Download the repository**
   ```bash
   git clone https://github.com/RichardScottOZ/global_tectonics.git
   cd global_tectonics
   ```

2. **Open in QGIS** (recommended for beginners)
   - Launch QGIS
   - Drag and drop `plates&provinces/shp/plates.shp` into QGIS
   - Right-click the layer → Properties → Symbology
   - Choose "Categorized" → Select `crust_type` as the value
   - Click "Classify" to automatically create oceanic/continental styling

3. **View in Google Earth** (for quick exploration)
   - Open Google Earth
   - File → Open → Navigate to `plates&provinces/kml/plates.kml`
   - Explore the 3D globe with tectonic boundaries overlaid

### Repository Structure Quick Reference

```
global_tectonics/
├── plates&provinces/
│   ├── shp/              ← Start here! Main shapefiles
│   ├── gmt/              ← For GMT/PyGMT users
│   └── kml/              ← For Google Earth
├── polygon_data/         ← Supplementary data (shorelines, LIPs)
├── styles/               ← Pre-made QGIS styles (.qml files)
├── DATA_EXPLANATION.md   ← Overview of datasets
├── DATA_DICTIONARY.md    ← Field definitions
└── USER_GUIDE.md         ← This document
```

---

## Software Requirements

### Recommended Software (Free and Open Source)

#### QGIS (Best for most users)
- **Download**: https://qgis.org/
- **Platforms**: Windows, Mac, Linux
- **Why**: User-friendly, powerful analysis tools, excellent visualization
- **Minimum Version**: 3.0 or higher

#### Python with GeoPandas (For programmers)
```bash
# Install with conda (recommended)
conda create -n geo_env python=3.9
conda activate geo_env
conda install geopandas matplotlib cartopy
```

#### GMT/PyGMT (For publication-quality cartography)
```bash
# Install GMT
# See: https://www.generic-mapping-tools.org/

# Install PyGMT (Python interface)
conda install pygmt
```

#### Google Earth (For quick visualization)
- **Download**: https://earth.google.com/
- **Platforms**: Windows, Mac, Linux
- **Why**: Easy 3D visualization, no GIS knowledge required

### Commercial Software (Optional)
- **ArcGIS Pro/ArcMap**: Fully compatible
- **MapInfo**: Shapefile support included
- **Global Mapper**: Works well

### Web GIS (No installation required)
- **QGIS Web Client**: Can be configured for these datasets
- **Mapbox/Leaflet**: Can convert to GeoJSON for web mapping

---

## Loading the Data

### QGIS (Detailed Instructions)

#### Method 1: Drag and Drop (Easiest)
1. Open QGIS
2. Open a file browser and navigate to `plates&provinces/shp/`
3. Drag `plates.shp` into the QGIS map canvas
4. Repeat for other shapefiles as needed

#### Method 2: Add Vector Layer
1. Layer → Add Layer → Add Vector Layer (Ctrl+Shift+V)
2. Click "..." next to "Vector Dataset(s)"
3. Navigate to `plates&provinces/shp/plates.shp`
4. Click "Add"

#### Method 3: DB Manager (For multiple files)
1. Database → DB Manager
2. Create a GeoPackage or SpatiaLite database
3. Import all shapefiles into one database
4. Easier management of multiple layers

#### Loading Pre-styled Layers
1. Load the shapefile (e.g., `plates.shp`)
2. Right-click layer → Properties → Symbology
3. At bottom, click "Style" → "Load Style"
4. Navigate to `styles/` folder
5. Choose appropriate .qml file (e.g., `plates_crust_type.qml`)
6. Click "Load Style" → "OK"

### Python (GeoPandas)

```python
import geopandas as gpd
import matplotlib.pyplot as plt

# Read shapefile
plates = gpd.read_file('plates&provinces/shp/plates.shp')

# Quick plot
plates.plot(column='crust_type', legend=True, figsize=(15, 10))
plt.title('Global Crustal Types')
plt.show()

# Basic information
print(plates.head())
print(plates.columns)
print(f"CRS: {plates.crs}")
```

### GMT (Generic Mapping Tools)

```bash
# Simple plot using GMT format
gmt begin tectonic_map png
    gmt coast -R-180/180/-90/90 -JN0/15c -Baf -Gwhite
    gmt plot plates&provinces/gmt/plates.gmt -W0.5p,red
    gmt plot plates&provinces/gmt/boundaries.gmt -W1p,black
gmt end show
```

### PyGMT (Python + GMT)

```python
import pygmt

fig = pygmt.Figure()
fig.basemap(region="g", projection="N15c", frame=True)
fig.coast(land="gray", water="lightblue")
fig.plot(data="plates&provinces/gmt/boundaries.gmt", pen="1p,black")
fig.show()
```

### ArcGIS Pro/ArcMap

1. Open ArcGIS Pro/ArcMap
2. Click "Add Data" button
3. Navigate to `plates&provinces/shp/plates.shp`
4. Click "Add"
5. Shapefiles appear in Table of Contents
6. Apply symbology as desired

---

## Basic Workflows

### Workflow 1: Identify Plate for Your Study Area

**Goal**: Find which tectonic plate(s) your study area is on

**Steps**:
1. Load `plates.shp` in QGIS
2. Zoom to your study area (use geocoding or coordinates)
3. Use "Identify Features" tool (Ctrl+Shift+I)
4. Click on the plate polygon
5. View attributes: `plate`, `plate_code`, `crust_type`, etc.

**Python Alternative**:
```python
import geopandas as gpd
from shapely.geometry import Point

# Load plates
plates = gpd.read_file('plates&provinces/shp/plates.shp')

# Create point for your study area (longitude, latitude)
study_point = gpd.GeoDataFrame(
    geometry=[Point(151.2, -33.9)],  # Sydney, Australia
    crs='EPSG:4326'
)

# Spatial join to find which plate
result = gpd.sjoin(study_point, plates, how='left', predicate='within')
print(f"Study area is on: {result['plate'].values[0]}")
print(f"Crust type: {result['crust_type'].values[0]}")
```

### Workflow 2: Extract Boundaries for Your Region

**Goal**: Get all plate boundaries within a specific region

**QGIS**:
1. Load `plate_boundaries.shp`
2. Vector → Research Tools → Select by Location
3. Create a bounding box or polygon for your region
4. Select features that intersect your region
5. Right-click layer → Export → Save Selected Features As...
6. Choose format and filename

**Python**:
```python
import geopandas as gpd

# Load boundaries
boundaries = gpd.read_file('plates&provinces/shp/plate_boundaries.shp')

# Define region (e.g., Pacific region)
region_bounds = (-180, -60, -100, 60)  # minx, miny, maxx, maxy
region_boundaries = boundaries.cx[region_bounds[0]:region_bounds[2], 
                                   region_bounds[1]:region_bounds[3]]

# Save to new file
region_boundaries.to_file('pacific_boundaries.shp')
```

### Workflow 3: Find All Provinces of a Specific Type

**Goal**: Identify all shield regions, or all orogenic belts, etc.

**QGIS**:
1. Load `global_gprv.shp`
2. Open Attribute Table (F6)
3. Click "Select features using an expression" (Ctrl+F)
4. Enter: `"prov_type" = 'shield'`
5. Click "Select Features"
6. Selected shields are highlighted

**Python**:
```python
import geopandas as gpd

# Load provinces
provinces = gpd.read_file('plates&provinces/shp/global_gprv.shp')

# Filter by type
shields = provinces[provinces['prov_type'] == 'shield']
print(f"Found {len(shields)} shield provinces")

# Plot results
ax = provinces.plot(color='lightgray', edgecolor='black', figsize=(15, 10))
shields.plot(ax=ax, color='red', edgecolor='darkred')
plt.title('Global Shield Provinces')
plt.show()
```

### Workflow 4: Create a Publication Map

**QGIS Print Layout**:
1. Load and style your layers (use styles from `styles/` folder)
2. Project → New Print Layout
3. Add Map: Add Item → Add Map
4. Draw rectangle on canvas to place map
5. Add Legend: Add Item → Add Legend
6. Add Scale Bar: Add Item → Add Scale Bar
7. Add North Arrow: Add Item → Add North Arrow
8. Add Title: Add Item → Add Label
9. Layout → Export as Image/PDF

**Python (Matplotlib)**:
```python
import geopandas as gpd
import matplotlib.pyplot as plt

fig, ax = plt.subplots(1, 1, figsize=(20, 10))

# Load and plot data
plates = gpd.read_file('plates&provinces/shp/plates.shp')
boundaries = gpd.read_file('plates&provinces/shp/plate_boundaries.shp')

# Plot oceanic/continental
plates[plates['crust_type'] == 'oceanic'].plot(
    ax=ax, color='lightblue', edgecolor='none', label='Oceanic Crust'
)
plates[plates['crust_type'] == 'continental'].plot(
    ax=ax, color='tan', edgecolor='none', label='Continental Crust'
)

# Plot boundaries
boundaries.plot(ax=ax, color='red', linewidth=0.5)

# Styling
ax.set_xlim(-180, 180)
ax.set_ylim(-90, 90)
ax.set_xlabel('Longitude', fontsize=12)
ax.set_ylabel('Latitude', fontsize=12)
ax.set_title('Global Tectonic Plates and Crustal Types', fontsize=16, weight='bold')
ax.legend(loc='lower left')
ax.grid(True, alpha=0.3)

plt.tight_layout()
plt.savefig('tectonic_map.png', dpi=300, bbox_inches='tight')
plt.show()
```

---

## Advanced Applications

### Application 1: Spatial Join with Your Data

**Scenario**: You have geochemical sample locations and want to know which geologic province each sample is in.

**Python**:
```python
import geopandas as gpd
import pandas as pd

# Load your sample data (CSV with latitude/longitude)
samples = pd.read_csv('my_samples.csv')
samples_gdf = gpd.GeoDataFrame(
    samples,
    geometry=gpd.points_from_xy(samples.longitude, samples.latitude),
    crs='EPSG:4326'
)

# Load provinces
provinces = gpd.read_file('plates&provinces/shp/global_gprv.shp')

# Spatial join
samples_with_province = gpd.sjoin(samples_gdf, provinces, how='left', predicate='within')

# Now each sample has province information
print(samples_with_province[['sample_id', 'prov_name', 'prov_type', 'lastorogen']])

# Save results
samples_with_province.to_csv('samples_with_provinces.csv', index=False)
```

**QGIS**:
1. Load your sample points (Layer → Add Layer → Add Delimited Text Layer)
2. Load `global_gprv.shp`
3. Vector → Data Management Tools → Join Attributes by Location
4. Input layer: your samples
5. Join layer: global_gprv
6. Geometric predicate: "within"
7. Run

### Application 2: Calculate Distance to Nearest Plate Boundary

**Python**:
```python
import geopandas as gpd
import numpy as np

# Load data
samples = gpd.read_file('my_samples.shp')
boundaries = gpd.read_file('plates&provinces/shp/plate_boundaries.shp')

# Reproject to equal-distance projection for accurate distance
samples_proj = samples.to_crs('EPSG:3857')  # Web Mercator
boundaries_proj = boundaries.to_crs('EPSG:3857')

# Dissolve boundaries into single geometry
boundary_union = boundaries_proj.unary_union

# Calculate distance (in meters)
samples_proj['dist_to_boundary'] = samples_proj.geometry.distance(boundary_union)

# Convert to kilometers
samples_proj['dist_to_boundary_km'] = samples_proj['dist_to_boundary'] / 1000

# Back to original CRS
samples = samples_proj.to_crs('EPSG:4326')

print(samples[['sample_id', 'dist_to_boundary_km']])
```

### Application 3: Identify Conjugate Margins for Palinspastic Reconstruction

**Python**:
```python
import geopandas as gpd

# Load provinces
provinces = gpd.read_file('plates&provinces/shp/global_gprv.shp')

# Find provinces with conjugate relationships
conjugate_pairs = provinces[provinces['conjugate1'].notna()]

# Create list of conjugate pairs
for idx, row in conjugate_pairs.iterrows():
    print(f"{row['prov_name']} (on {row['continent']}) was adjacent to:")
    # Find the conjugate province
    conjugate = provinces[provinces['prov_name'] == row['conjugate1']]
    if not conjugate.empty:
        print(f"  → {row['conjugate1']} (on {conjugate.iloc[0]['continent']})")
        print(f"  → Last orogen: {row['lastorogen']}")
        print()
```

### Application 4: Temporal Analysis Using Orogenic Ages

**Python**:
```python
import geopandas as gpd
import matplotlib.pyplot as plt

# Load provinces
provinces = gpd.read_file('plates&provinces/shp/global_gprv.shp')

# Define orogenic ages (simplified)
orogen_ages = {
    'Archean': 2500,
    'Trans Hudson': 1850,
    'Yavapai-Mazatzal': 1700,
    'Grenville': 1150,
    'Pan-African': 550,
    'Caledonian': 450,
    'Hercynian': 320,
    'Cimmerian': 200,
    'Alpine': 50
}

# Map ages to provinces
provinces['age_ma'] = provinces['lastorogen'].map(orogen_ages)

# Plot colored by age
fig, ax = plt.subplots(1, 1, figsize=(20, 10))
provinces.plot(
    column='age_ma',
    cmap='viridis_r',  # Reverse so older is darker
    legend=True,
    ax=ax,
    edgecolor='black',
    linewidth=0.2,
    legend_kwds={'label': 'Age of Last Orogeny (Ma)'}
)
ax.set_title('Global Geologic Provinces Colored by Orogenic Age', fontsize=16)
plt.show()
```

### Application 5: Buffer Analysis Around Plate Boundaries

**Use Case**: Identify regions within 100 km of active plate boundaries

**Python**:
```python
import geopandas as gpd

# Load boundaries
boundaries = gpd.read_file('plates&provinces/shp/plate_boundaries.shp')

# Filter to major boundaries only
major_boundaries = boundaries[boundaries['level'] == 1]

# Reproject to equal-area for accurate buffer
boundaries_proj = major_boundaries.to_crs('EPSG:3857')

# Create 100 km buffer (100000 meters)
buffer_100km = boundaries_proj.buffer(100000)

# Convert back to GeoDataFrame
buffer_gdf = gpd.GeoDataFrame(geometry=buffer_100km, crs='EPSG:3857')
buffer_gdf = buffer_gdf.to_crs('EPSG:4326')

# Save result
buffer_gdf.to_file('boundary_buffer_100km.shp')

# Now you can use this for spatial analysis
# e.g., count how many earthquakes are within 100 km of boundaries
```

---

## Working with Different Formats

### Converting Between Formats

#### Shapefile to GeoJSON
```python
import geopandas as gpd

plates = gpd.read_file('plates&provinces/shp/plates.shp')
plates.to_file('plates.geojson', driver='GeoJSON')
```

#### Shapefile to GeoPackage (Recommended for complex projects)
```python
import geopandas as gpd

# Create a single GeoPackage with multiple layers
plates = gpd.read_file('plates&provinces/shp/plates.shp')
boundaries = gpd.read_file('plates&provinces/shp/plate_boundaries.shp')
provinces = gpd.read_file('plates&provinces/shp/global_gprv.shp')

# Save all to one file
plates.to_file('global_tectonics.gpkg', layer='plates', driver='GPKG')
boundaries.to_file('global_tectonics.gpkg', layer='boundaries', driver='GPKG')
provinces.to_file('global_tectonics.gpkg', layer='provinces', driver='GPKG')
```

#### GMT to Shapefile
```bash
# Using ogr2ogr (part of GDAL)
ogr2ogr -f "ESRI Shapefile" plates_from_gmt.shp plates&provinces/gmt/plates.gmt
```

#### KML to Shapefile (QGIS)
1. Layer → Add Layer → Add Vector Layer
2. Select the .kml file
3. Right-click layer → Export → Save Features As
4. Format: ESRI Shapefile
5. Choose filename and click OK

### Working with Web Services

#### Serve Data with GeoServer
1. Install GeoServer
2. Add the shapefiles as new data stores
3. Create layers and styles
4. Access via WMS/WFS for web applications

#### Leaflet Web Map Example
```javascript
// After converting to GeoJSON
var map = L.map('map').setView([0, 0], 2);

L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png').addTo(map);

// Load GeoJSON
fetch('plates.geojson')
  .then(response => response.json())
  .then(data => {
    L.geoJSON(data, {
      style: function(feature) {
        return {
          color: feature.properties.crust_type === 'oceanic' ? 'blue' : 'brown',
          weight: 1
        };
      }
    }).addTo(map);
  });
```

---

## Visualization and Styling

### Pre-made QGIS Styles

The `styles/` directory contains ready-to-use .qml style files:

| Style File | Use For | Description |
|------------|---------|-------------|
| `plates_crust_type.qml` | plates.shp | Oceanic (blue) vs Continental (tan) |
| `plates_domains.qml` | plates.shp | Oceanic domain classification |
| `plates_types.qml` | plates.shp | Rigid plates vs microplates |
| `boundaries.qml` | plate_boundaries.shp | Color-coded boundary types |
| `gprv_types.qml` | global_gprv.shp | Province classification |
| `gprv_orogens.qml` | global_gprv.shp | Colored by orogenic age |
| `gprv_crust_type.qml` | global_gprv.shp | Continental vs oceanic |

**How to apply**:
1. Load shapefile in QGIS
2. Right-click → Properties → Symbology
3. Bottom of dialog → Style → Load Style
4. Select appropriate .qml file
5. Click "Load Style" then "OK"

### Custom Styling Tips

#### QGIS Rule-Based Styling
For complex visualizations:
1. Layer Properties → Symbology → Rule-based
2. Add rules with filters, e.g.:
   - `"crust_type" = 'oceanic' AND "domain" = 'Back-Arc'`
   - `"prov_type" = 'orogenic belt' AND "lastorogen" = 'Alpine'`
3. Set colors/symbols for each rule

#### Color Schemes
Recommended color schemes:
- **Crustal type**: Blue (oceanic), Tan/Brown (continental)
- **Orogenic ages**: ColorBrewer "YlOrRd" or "Viridis" (old to young)
- **Province types**: Categorical colors from ColorBrewer "Set3"

#### Label Configuration
To label plates:
1. Right-click layer → Properties → Labels
2. Select "Single Labels"
3. Value: Choose `plate` or `plate_code`
4. Buffer: Check "Draw text buffer" (white background)
5. Placement: "Horizontal" or "Around centroid"

---

## Common Use Cases

### Use Case 1: Earthquake Hazard Analysis

**Objective**: Identify regions near plate boundaries for seismic hazard

**Workflow**:
1. Load `plate_boundaries.shp`
2. Filter for subduction zones and collision zones (high seismicity)
   ```python
   high_hazard = boundaries[boundaries['type'].isin([
       'subduction zone', 'collision zone'
   ])]
   ```
3. Create 50-200 km buffer zones
4. Overlay with population data to assess exposure

### Use Case 2: Geochemical Provenance Study

**Objective**: Determine tectonic setting of sediment sources

**Workflow**:
1. Load sample locations (point data)
2. Spatial join with `global_gprv.shp` to get province info
3. Analyze `prov_type` and `lastorogen` for each sample
4. Group samples by tectonic setting
5. Compare geochemical signatures across settings

### Use Case 3: Mineral Exploration Targeting

**Objective**: Identify prospective terranes based on metallogenic province

**Workflow**:
1. Load `global_gprv.shp`
2. Filter by `prov_type` and `lastorogen` matching known deposits
   - Example: Archean greenstone belts for gold
   - Example: Proterozoic orogenic belts for base metals
3. Cross-reference with `cratons.shp` for stability
4. Overlay with magnetic/gravity anomalies (from Zenodo dataset)

### Use Case 4: Continental Reconstruction

**Objective**: Restore continents to past configuration

**Workflow**:
1. Load `global_gprv.shp` and `cratons.shp`
2. Use `conjugate1` and `conjugate2` fields to identify pairs
3. Use GPlates software with:
   - PLATEID1/PLATEID2 for plate IDs
   - FROMAGE/TOAGE for temporal constraints
   - RECON_METH for reconstruction parameters
4. Validate against geological matches (ages, rock types)

### Use Case 5: Teaching Plate Tectonics

**Objective**: Educational visualization for students

**Workflow**:
1. Open `plates.kml` in Google Earth for 3D globe view
2. Demonstrate plate motions by showing:
   - Spreading centers (mid-ocean ridges)
   - Subduction zones (deep trenches)
   - Transform faults
3. Overlay earthquake data to show seismicity at boundaries
4. Tour famous features (San Andreas, Himalayas, Mid-Atlantic Ridge)

---

## Troubleshooting

### Common Issues and Solutions

#### Issue: Shapefile Won't Load
**Symptoms**: Error message or missing data
**Solutions**:
- Ensure all component files are present (.shp, .shx, .dbf, .prj)
- Check file permissions (must have read access)
- Verify file isn't corrupted (re-download if necessary)
- Try loading in different software to isolate the problem

#### Issue: Coordinate System Problems
**Symptoms**: Data appears in wrong location or severely distorted
**Solutions**:
- Verify CRS is set to EPSG:4326 (WGS84)
- In QGIS: Right-click layer → Set Layer CRS → Select EPSG:4326
- Check that project CRS matches data CRS
- Use "Reproject Layer" if CRS needs changing

#### Issue: Attributes Are Garbled
**Symptoms**: Special characters appear as boxes or question marks
**Solutions**:
- Ensure encoding is set to UTF-8
- In QGIS: Layer Properties → Source → Encoding → Select "UTF-8"
- .cpg files should specify UTF-8 (already included)

#### Issue: Slow Performance with Multiple Layers
**Symptoms**: QGIS/software is sluggish with all layers loaded
**Solutions**:
- Load only necessary layers for your analysis
- Simplify geometries for display (not analysis): Vector → Geometry Tools → Simplify
- Create spatial indexes: Vector → Data Management Tools → Create Spatial Index
- Use zoom-dependent visibility: Layer Properties → Rendering → Scale-dependent visibility

#### Issue: Can't See Small Features
**Symptoms**: Boundaries or small provinces not visible at global scale
**Solutions**:
- Adjust line width/symbol size: Layer Properties → Symbology
- Use zoom-dependent styling to show more detail when zoomed in
- Check layer order (boundaries should be on top of plates)

#### Issue: Gaps or Overlaps in Data
**Symptoms**: Unexpected spaces or overlapping polygons
**Solutions**:
- This is expected at global scale (simplification for file size)
- For local studies, these datasets may not be appropriate
- Use "Topology Checker" plugin in QGIS to identify issues
- Report significant gaps via GitHub issues

#### Issue: Python Can't Find Files
**Symptoms**: FileNotFoundError in Python scripts
**Solutions**:
```python
import os

# Use absolute paths
base_path = '/full/path/to/global_tectonics/'
plates_path = os.path.join(base_path, 'plates&provinces/shp/plates.shp')

# Or use relative paths from script location
import os
script_dir = os.path.dirname(os.path.abspath(__file__))
plates_path = os.path.join(script_dir, '../plates&provinces/shp/plates.shp')
```

---

## Contributing to the Project

The global_tectonics project thrives on community contributions. Here's how you can help improve the datasets.

### Types of Contributions

1. **Bug Reports**: Identify errors in boundaries or attributes
2. **Data Updates**: New geological knowledge or refined boundaries
3. **Documentation**: Improve guides, add examples, fix typos
4. **Styling**: Create new QGIS styles for different applications
5. **Scripts**: Share analysis workflows or visualization code

### Contribution Workflow

#### Step 1: Check Existing Issues
Before contributing, search GitHub Issues to see if your concern is already documented.

#### Step 2: Open an Issue (for discussion)
1. Go to: https://github.com/RichardScottOZ/global_tectonics/issues
2. Click "New Issue"
3. Choose appropriate template (bug report or feature request)
4. Provide clear description with:
   - Location (coordinates or region name)
   - Issue description
   - Supporting evidence (references, citations)
   - Suggested correction

#### Step 3: Make Changes (for code/data contributions)
```bash
# Fork the repository on GitHub

# Clone your fork
git clone https://github.com/YOUR_USERNAME/global_tectonics.git
cd global_tectonics

# Create a branch for your changes
git checkout -b fix-description-of-issue

# Make your changes
# Edit files, test changes

# Commit changes
git add .
git commit -m "Descriptive commit message"

# Push to your fork
git push origin fix-description-of-issue
```

#### Step 4: Submit a Pull Request
1. Go to your fork on GitHub
2. Click "Pull Request" button
3. Provide clear description of changes
4. Reference related issues (#123)
5. Submit for review

### Contribution Guidelines

**Data Changes**:
- Provide literature references (DOI preferred)
- Explain rationale for changes
- Major boundary changes should cite peer-reviewed sources
- Minor corrections (typos in attributes) can reference established databases

**Code Contributions**:
- Comment your code clearly
- Follow existing style conventions
- Test code before submitting
- Include example output or screenshots

**Documentation**:
- Use clear, concise language
- Provide examples where helpful
- Test instructions on a clean installation
- Check spelling and grammar

### Review Process
1. Maintainers will review your contribution
2. May request changes or clarifications
3. Once approved, changes are merged
4. Contributors are acknowledged in release notes

### Contact for Major Contributions
For significant updates or collaborations:
- Email: derrick.hasterok@adelaide.edu.au
- Discuss proposed changes before implementing

---

## FAQ

### General Questions

**Q: What license governs this data?**  
A: GNU General Public License v3.0 (GPL-3.0). You can freely use, modify, and distribute, but derivative works must maintain the same license.

**Q: How do I cite this dataset?**  
A: See the README.md file for complete citation information. Primary reference is Hasterok et al. (2022) in Earth Science Reviews.

**Q: Is this data suitable for local-scale studies?**  
A: No. This is a global compilation with simplified boundaries. For local studies (e.g., city or field scale), use regional geological maps.

**Q: How often is the data updated?**  
A: Updates are ongoing via GitHub. Check the repository for the latest version. Major releases are documented in the version history.

**Q: Can I use this for commercial purposes?**  
A: Yes, under GPL-3.0 terms. You must provide attribution and release derivative works under the same license.

### Technical Questions

**Q: What coordinate system should I use for analysis?**  
A: Data is in WGS84 (EPSG:4326). For accurate distance/area calculations, reproject to an appropriate equal-distance or equal-area projection for your region.

**Q: Why are there gaps between polygons?**  
A: Global-scale generalization. Gaps are typically <1 km and insignificant at the intended scale of use.

**Q: Can I merge this with other datasets?**  
A: Yes! Use spatial joins (for locations) or attribute joins (using plate codes or IDs). See "Advanced Applications" section for examples.

**Q: Which format should I use?**  
A: 
- **Shapefile**: Most compatible, use for QGIS/ArcGIS
- **GMT**: For GMT/PyGMT workflows
- **KML**: For Google Earth visualization

**Q: How do I handle the & character in folder names?**  
A: In terminal/command line, use quotes: `cd "plates&provinces/shp/"` or escape: `cd plates\&provinces/shp/`

### Data Interpretation Questions

**Q: What does "last orogen" mean?**  
A: The most recent major thermal/deformation event recorded in that province. Earlier events may also be present in the rocks.

**Q: How are plate boundaries defined?**  
A: Based on seismicity, GPS velocities, magnetic anomalies, and structural geology from published literature.

**Q: What is the difference between "plate" and "subplate"?**  
A: Plates are major lithospheric fragments. Subplates are smaller, semi-independent blocks within or between major plates.

**Q: Why are some oceanic domains classified as "Back-Arc"?**  
A: These formed in extensional settings behind subduction zones, not at mid-ocean ridges. They have distinct geophysical properties.

**Q: What are conjugate margins?**  
A: Formerly adjacent terranes now separated by ocean basins. Important for reconstructing past continental configurations.

### Troubleshooting Questions

**Q: Why doesn't my spatial join work?**  
A: Check that both datasets have the same CRS. Reproject if necessary. Ensure geometries are valid (use "Fix geometries" tool in QGIS).

**Q: QGIS is very slow with this data. Help!**  
A: Create spatial indexes (Vector → Data Management Tools → Create Spatial Index). Use layer visibility to show/hide layers. Simplify geometries for display only.

**Q: Python says "module not found" for geopandas.**  
A: Install with: `conda install geopandas` or `pip install geopandas`. GeoPandas has complex dependencies; conda is recommended.

**Q: The .prj file seems incorrect.**  
A: The data is in EPSG:4326 (WGS84 Geographic). If issues persist, manually set CRS in your GIS software.

---

## Additional Resources

### Documentation
- **DATA_EXPLANATION.md**: Overview of datasets and scientific context
- **DATA_DICTIONARY.md**: Complete field definitions and value ranges
- **README.md**: Quick start and citation information

### External Resources
- **Zenodo Repository**: https://doi.org/10.5281/zenodo.5093930 (includes QGIS project with additional data)
- **EarthByte Portal**: https://www.earthbyte.org/ (plate reconstruction tools)
- **GPlates Software**: https://www.gplates.org/ (plate tectonic reconstruction)
- **QGIS Tutorials**: https://www.qgistutorials.com/
- **GeoPandas Documentation**: https://geopandas.org/

### Learning Resources
- **Plate Tectonics Primer**: USGS - https://www.usgs.gov/programs/earthquake-hazards/plate-tectonics
- **GIS Fundamentals**: QGIS Training Manual - https://docs.qgis.org/
- **Python Geospatial**: Python for Geospatial Data Analysis - https://pythongis.org/

### Community
- **GitHub Issues**: Report problems, ask questions
- **Email**: derrick.hasterok@adelaide.edu.au for major inquiries
- **Twitter**: Follow #globaltectonics for updates (check with maintainers for current social media)

---

## Version History and Changelog

See README.md and GitHub releases for detailed version history.

**Major Updates**:
- **v1.0 (2021-07-19)**: Initial release
- **v1.1 (2022-05-22)**: Updated plates, boundaries, oc_boundaries, global_gprv post-review
- **v1.2 (2022-05-23)**: Accepted version aligned with published paper
- **Current**: Ongoing community contributions

---

## Appendix: Quick Reference Commands

### QGIS Keyboard Shortcuts
- `Ctrl+Shift+V`: Add Vector Layer
- `Ctrl+Shift+I`: Identify Features
- `F6`: Open Attribute Table
- `Ctrl+F`: Select by Expression
- `Ctrl+I`: Invert Selection

### Python One-Liners
```python
# Quick load and plot
import geopandas as gpd; gpd.read_file('plates&provinces/shp/plates.shp').plot()

# Count features by type
gpd.read_file('plates&provinces/shp/global_gprv.shp')['prov_type'].value_counts()

# Get field names
gpd.read_file('plates&provinces/shp/plates.shp').columns.tolist()
```

### GMT Quick Commands
```bash
# Info about GMT file
gmt info plates&provinces/gmt/plates.gmt

# Quick plot
gmt psxy plates&provinces/gmt/boundaries.gmt -R-180/180/-90/90 -JM15c > map.ps
```

---

**End of User Guide**

For questions not covered here, please:
1. Check DATA_DICTIONARY.md for field definitions
2. Check DATA_EXPLANATION.md for dataset context
3. Search GitHub Issues for similar questions
4. Open a new issue on GitHub
5. Contact derrick.hasterok@adelaide.edu.au

*Last updated: 2026*  
*User Guide version: 1.0*
