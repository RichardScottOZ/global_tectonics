# Global Tectonics Data Explanation

## Overview

The **global_tectonics** repository contains comprehensive geospatial datasets for analyzing Earth's tectonic structure and geological provinces. This data package supports solid Earth science applications including geodynamic modeling, geochemical studies, tectonic reconstructions, and regional geological analysis.

## Repository Structure

```
global_tectonics/
├── plates&provinces/          # Main tectonic datasets
│   ├── shp/                   # Shapefiles (primary format)
│   ├── gmt/                   # Generic Mapping Tools format
│   └── kml/                   # Google Earth/KML format
├── polygon_data/              # Additional reference datasets
├── styles/                    # QGIS style files (.qml)
├── README.md                  # Project overview and citations
├── DATA_DICTIONARY.md         # Detailed field definitions
└── USER_GUIDE.md              # Usage instructions and examples
```

## Core Datasets

The repository includes five primary shapefiles representing different aspects of global tectonics:

### 1. Plates (plates.shp)
- **Purpose**: Present-day tectonic plate boundaries and crustal domains
- **Records**: 280 polygon features
- **Coverage**: Global, including major and micro-plates
- **Key Features**:
  - Plate names and codes following Bird (2003) convention
  - Crustal type classification (oceanic/continental)
  - Oceanic domain classification (spreading centers, back-arc, etc.)
  - Subplate and microplate distinctions
  - Geographic sea/region names

### 2. Plate Boundaries (plate_boundaries.shp)
- **Purpose**: Linear features representing plate boundary interactions
- **Records**: 571 line features
- **Coverage**: Global plate boundaries
- **Key Features**:
  - Boundary types (spreading centers, subduction zones, transforms, collision zones)
  - Adjoining plate identification
  - Hierarchical levels (major vs. minor boundaries)
  - Named features (e.g., "Galapagos Ridge", "San Andreas Fault")

### 3. Ocean-Continent Boundaries (oc_boundaries.shp)
- **Purpose**: Delineates the transition between oceanic and continental crust
- **Records**: 93 line features
- **Coverage**: Global continental margins
- **Key Features**:
  - Represents the fundamental crustal boundary
  - Important for studies of continental rifting and passive margins
  - Seamlessly integrates with plates.shp crustal classification

### 4. Global Geologic Provinces (global_gprv.shp)
- **Purpose**: Comprehensive map of geological provinces and terranes
- **Records**: 914 polygon features
- **Coverage**: Global continental and oceanic provinces
- **Key Features**:
  - Province names and types (shields, platforms, orogenic belts, basins, rifts, etc.)
  - Last major orogenic event affecting the province
  - Continental affinity
  - Conjugate margin relationships for palinspastic reconstruction
  - Crustal type
  - Province groupings (e.g., cratons)

### 5. Cratons (cratons.shp)
- **Purpose**: Archean (>2500 Ma) cratonic regions and basement exposures
- **Records**: 114 polygon features
- **Coverage**: Global ancient continental cores
- **Key Features**:
  - Derived primarily from global_gprv.shp with additional refinements
  - Includes Archean basement regions identified through geochemical sampling
  - Post-Archean reworking classification
  - Integration with western US data from Lund et al. (2015)
  - Critical for understanding early Earth evolution and continental stability

## Additional Datasets

### Polygon Data
Located in the `polygon_data/` directory:

#### GSHHS_I_L1 (High-Resolution Shoreline Data)
- **Source**: Global Self-consistent, Hierarchical, High-resolution Geography Database
- **Records**: 6,042 polygons
- **Purpose**: High-resolution coastline and water body boundaries
- **Use Case**: Basemap layer for displaying tectonic features in geographic context

#### Johansson_etal_2018_EarthByte_LIPs_v2 (Large Igneous Provinces)
- **Source**: Johansson et al. (2018), EarthByte research group
- **Records**: 2,526 polygons
- **Purpose**: Global compilation of Large Igneous Provinces (LIPs)
- **Features**:
  - LIP names and types
  - Age constraints (from-age, to-age)
  - Plate reconstruction parameters
  - Integration with GPlates plate motion model

### Style Files
The `styles/` directory contains QGIS style files (.qml) for visualizing the datasets:
- **Thematic styling**: Color schemes for province types, orogen ages, crustal types
- **Boundary symbology**: Line styles for different boundary types
- **Domain visualization**: Specialized styles for oceanic domains
- **Integration ready**: Pre-configured for quick visualization in QGIS

## Data Formats

All primary datasets are provided in three formats to maximize compatibility:

### Shapefile (.shp)
- **Primary format** with complete attribute data
- Compatible with all major GIS software (QGIS, ArcGIS, etc.)
- Includes .dbf (attributes), .shx (index), .prj (projection), .cpg (encoding)
- Coordinate System: WGS84 Geographic (EPSG:4326)

### Generic Mapping Tools (.gmt)
- Optimized for GMT/PyGMT workflows
- ASCII vector format
- Efficient for command-line cartographic processing
- Maintains geometric precision for global-scale mapping

### Keyhole Markup Language (.kml)
- Google Earth compatible
- Useful for field validation and educational outreach
- Preserves attribute data and styling
- Accessible to non-GIS users

## Scientific Context

### Methodological Foundation
The tectonic datasets are based on the comprehensive review and compilation by Hasterok et al. (2022):

> Hasterok, D., Halpin, J., Hand, M., Collins, A., Kreemer, C., Gard, M.G., Glorie, S., (2022) 
> New maps of global geologic provinces and tectonic plates, *Earth Science Reviews*.

This work represents:
- Integration of hundreds of regional geological studies
- Harmonization of province boundaries across political borders
- Synthesis of geochronological data for orogenic age assignments
- Incorporation of modern plate kinematic models
- Community review and validation process

### Key Innovations
1. **Seamless Global Coverage**: Eliminates artificial boundaries at continental margins or political borders
2. **Hierarchical Classification**: Multi-level organization from plates to microplates to crustal domains
3. **Temporal Information**: Orogenic age assignments enable time-dependent analysis
4. **Conjugate Relationships**: Explicit links between formerly adjacent terranes for plate reconstruction
5. **Multi-format Distribution**: Accessibility across different software ecosystems

### Applications
The datasets support diverse research applications:
- **Geodynamic Modeling**: Boundary conditions for mantle convection and lithospheric deformation
- **Geochemical Analysis**: Tectonic context for isotopic and trace element studies
- **Natural Hazards**: Seismotectonic framework for earthquake and volcanic hazard assessment
- **Resource Exploration**: Metallogenic province analysis and basin evolution
- **Paleotectonic Reconstruction**: Constraints for plate reconstruction models (e.g., GPlates)
- **Earth Science Education**: Teaching tools for tectonic processes and global geology

## Data Quality and Limitations

### Strengths
- Peer-reviewed compilation based on published literature
- Global consistency in classification and nomenclature
- Regular community-driven updates
- Multiple format options for broad accessibility
- Comprehensive attribute metadata

### Known Limitations
- Province boundaries are simplified for global-scale mapping (not suitable for local-scale studies)
- Orogenic age assignments represent "last major event" (earlier events may be present)
- Submarine plateaus and oceanic microplates are continually being refined
- Some remote regions (e.g., Antarctica beneath ice) have limited geological constraints
- Transitional zones (e.g., extended continental margins) are represented as discrete boundaries

### Updates and Contributions
This is a **living dataset** designed for community improvement. The GitHub repository facilitates:
- Issue tracking for boundary refinements
- Pull requests for new data integration
- Discussion of classification schemes
- Version control for reproducible research

Users are encouraged to contribute corrections and improvements. See `USER_GUIDE.md` for contribution guidelines.

## Licensing and Citation

### License
The global_tectonics dataset is released under the **GNU General Public License v3.0 (GPL-3.0)**.
- Free to use, modify, and distribute
- Derivative works must maintain the same license
- See LICENSE file for complete terms

### Citation
When using this dataset in publications, please cite:

**Primary Reference:**
```
Hasterok, D., Halpin, J., Hand, M., Collins, A., Kreemer, C., Gard, M.G., Glorie, S., (2022)
New maps of global geologic provinces and tectonic plates, Earth Science Reviews.
https://doi.org/10.31223/X5TD1C
```

**Dataset DOI:**
```
Hasterok, D., Halpin, J., Hand, M., Collins, A., Kreemer, C., Glorie, S., (2022)
New maps of global geologic provinces and tectonic plates: global tectonics data and QGIS project file,
Zenodo, https://doi.org/10.5281/zenodo.5093930
```

### Additional References
- **Bird, P. (2003)**: An updated digital model of plate boundaries. *Geochemistry, Geophysics, Geosystems*, 4(3). https://doi.org/10.1029/2001gc000252
- **Lund, K. et al. (2015)**: Basement domain map of the conterminous United States. *USGS Data Series 898*. https://doi.org/10.3133/ds898

## Related Resources

### Zenodo Repository
A static version with additional geophysical datasets (gravity, magnetics, topography) and a complete QGIS project file is available at:
- https://doi.org/10.5281/zenodo.5093930

### Contact
For questions, contributions, or collaboration:
- **Email**: derrick.hasterok@adelaide.edu.au
- **GitHub Issues**: Use the repository issue tracker for technical questions or data corrections

## Version History

- **v1.0** (2021-07-19): Initial public release
- **v1.1** (2022-05-22): Updated plates, boundaries, oc_boundaries, and global_gprv following review
- **v1.2** (2022-05-23): Revised manuscript accepted
- **Current**: Ongoing community updates via GitHub

## Next Steps

For detailed information about:
- **Field definitions and data values**: See `DATA_DICTIONARY.md`
- **Usage instructions and examples**: See `USER_GUIDE.md`
- **Quick start**: See `README.md`
