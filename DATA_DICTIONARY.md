# Global Tectonics Data Dictionary

## Overview

This document provides detailed field definitions, data types, valid values, and examples for all attributes in the global_tectonics datasets. Each field is described with its purpose, format, and usage guidelines.

## Table of Contents

1. [Plates (plates.shp)](#plates-platesshp)
2. [Plate Boundaries (plate_boundaries.shp)](#plate-boundaries-plate_boundariesshp)
3. [Ocean-Continent Boundaries (oc_boundaries.shp)](#ocean-continent-boundaries-oc_boundariesshp)
4. [Global Geologic Provinces (global_gprv.shp)](#global-geologic-provinces-global_gprvshp)
5. [Cratons (cratons.shp)](#cratons-cratonsshp)
6. [GSHHS Shoreline (GSHHS_I_L1.shp)](#gshhs-shoreline-gshhs_i_l1shp)
7. [Large Igneous Provinces (Johansson_etal_2018_EarthByte_LIPs_v2.shp)](#large-igneous-provinces-johansson_etal_2018_earthbyte_lips_v2shp)

---

## Plates (plates.shp)

**Geometry Type**: Polygon  
**Record Count**: 280  
**Coordinate System**: WGS84 (EPSG:4326)

### Fields

#### plate
- **Type**: Text (100 characters)
- **Description**: Name of the major tectonic plate
- **Required**: Yes
- **Examples**: 
  - "Eurasian Plate"
  - "Pacific Plate"
  - "Arabian Plate"
  - "Antarctic Plate"
- **Notes**: Follows Bird (2003) plate nomenclature with updates for newly recognized plates

#### plate_code
- **Type**: Text (3 characters)
- **Description**: Two or three-letter abbreviation for the plate or subplate
- **Required**: Yes
- **Examples**:
  - "PA" (Pacific)
  - "EU" (Eurasia)
  - "AR" (Arabia)
  - "WM" (Western Mediterranean)
- **Notes**: Codes are unique identifiers; consistent with international plate kinematic models

#### subplate
- **Type**: Text (128 characters)
- **Description**: Name of subplate or microplate subdivision within the major plate
- **Required**: Conditional (for microplates)
- **Examples**:
  - "Western Mediterranean"
  - "Sunda"
  - "Bering Sea"
  - "Galapagos"
- **Notes**: Used when a major plate contains distinct kinematic domains

#### poly_name
- **Type**: Text (100 characters)
- **Description**: Descriptive name of the specific polygon feature
- **Required**: Yes
- **Examples**:
  - "Western Mediterranean Sea"
  - "South China Sea"
  - "Red Sea (east)"
  - "Philippine Sea"
- **Notes**: Often corresponds to geographic sea names for oceanic crust; may indicate mainland regions for continental areas

#### plate_type
- **Type**: Text (64 characters)
- **Description**: Classification of plate kinematic behavior
- **Required**: Yes
- **Valid Values**:
  - "rigid plate" - Major plates with coherent motion
  - "microplate" - Small plates with independent motion
  - "continental block" - Deforming continental regions
  - "diffuse zone" - Distributed deformation areas
- **Examples**: "rigid plate", "microplate"
- **Notes**: Rigid plates typically >10^6 km² with minimal internal deformation

#### crust_type
- **Type**: Text (12 characters)
- **Description**: Fundamental crustal composition
- **Required**: Yes
- **Valid Values**:
  - "oceanic" - Mafic crust formed at spreading centers
  - "continental" - Felsic crust of continental origin
- **Examples**: "oceanic", "continental"
- **Notes**: Based on crustal structure, composition, and formation environment

#### sea_name
- **Type**: Text (100 characters)
- **Description**: Geographic name of the ocean or sea basin
- **Required**: Conditional (for oceanic crust)
- **Examples**:
  - "Mediterranean Sea"
  - "South China Sea"
  - "Red Sea"
  - "Pacific Ocean"
- **Notes**: Empty for continental polygons

#### domain
- **Type**: Text (100 characters)
- **Description**: Classification of oceanic crustal origin
- **Required**: Conditional (for oceanic crust)
- **Valid Values**:
  - "Atlantic" - Atlantic-type slow-spreading crust
  - "Pacific" - Pacific-type fast-spreading crust
  - "Indian" - Indian Ocean spreading crust
  - "Mediterranean" - Complex Mediterranean basins
  - "Back-Arc" - Back-arc basin crust
  - "Marginal" - Marginal sea basins
- **Examples**: "Back-Arc", "Pacific", "Mediterranean"
- **Notes**: Reflects spreading rate and tectonic setting; important for geophysical properties

#### area
- **Type**: Numeric (24.15 format)
- **Description**: Area of polygon in square kilometers
- **Required**: Yes (auto-calculated)
- **Units**: km²
- **Range**: 10 to ~10,000,000
- **Example**: 194416.735635925753741
- **Notes**: Calculated in WGS84 geographic coordinates; use equal-area projection for accurate area-based analysis

#### plate_ref
- **Type**: Text (100 characters)
- **Description**: Primary reference(s) for plate definition
- **Required**: Yes
- **Format**: DOI or citation
- **Examples**:
  - "doi:10.1029/2001gc000252 and doi:10.1144/sp504-2020-218"
  - "doi:10.1785/0120150232"
- **Notes**: Multiple references separated by "and"; enables traceability to source literature

#### plate_id
- **Type**: Integer (8 digits)
- **Description**: Unique numerical identifier for the plate
- **Required**: Yes
- **Range**: 100-9999
- **Examples**: 301, 1100, 330
- **Notes**: Consistent across related datasets; used for joining with plate motion models

#### id
- **Type**: Integer (10 digits)
- **Description**: Unique sequential identifier for each polygon
- **Required**: Yes
- **Range**: 1-280
- **Examples**: 1, 2, 3
- **Notes**: Primary key for the shapefile; sequential order has no geographic meaning

---

## Plate Boundaries (plate_boundaries.shp)

**Geometry Type**: Polyline  
**Record Count**: 571  
**Coordinate System**: WGS84 (EPSG:4326)

### Fields

#### feature_id
- **Type**: Integer (10 digits)
- **Description**: Unique identifier for each boundary segment
- **Required**: Yes
- **Range**: 1-571
- **Example**: 1, 2, 3
- **Notes**: Primary key; may be non-sequential due to updates

#### feature
- **Type**: Text (254 characters)
- **Description**: Named geological feature or plate boundary
- **Required**: Conditional
- **Examples**:
  - "Galapagos Ridge"
  - "San Andreas Fault"
  - "Papuan Fold and Thrust Belt"
  - "Himalayan Collision Zone"
- **Notes**: May be empty for unnamed or generic boundary segments

#### type
- **Type**: Text (100 characters)
- **Description**: Classification of plate boundary kinematics
- **Required**: Yes
- **Valid Values**:
  - "spreading center" - Divergent boundary (mid-ocean ridge)
  - "subduction zone" - Convergent boundary with slab descent
  - "collision zone" - Continent-continent convergence
  - "transform" - Strike-slip boundary
  - "dextral transform" - Right-lateral strike-slip
  - "sinistral transform" - Left-lateral strike-slip
  - "deformation zone" - Diffuse boundary
- **Examples**: "spreading center", "collision zone", "dextral transform"
- **Notes**: Kinematics based on relative plate motions; some boundaries show complex behavior

#### comment
- **Type**: Text (254 characters)
- **Description**: Additional notes about the boundary segment
- **Required**: No
- **Examples**:
  - "Inactive spreading center"
  - "Poorly constrained"
  - "Transitional zone"
- **Notes**: Used for special cases, uncertainties, or clarifications

#### plate1
- **Type**: Text (100 characters)
- **Description**: Name of the first adjacent plate
- **Required**: Yes
- **Examples**: "Galapagos", "Australian", "Pacific"
- **Notes**: Plate name or subplate name; see plates.shp for full list

#### plate2
- **Type**: Text (100 characters)
- **Description**: Name of the second adjacent plate
- **Required**: Yes
- **Examples**: "Cocos", "Philippine", "North American"
- **Notes**: Order of plate1/plate2 is arbitrary for most boundaries

#### level
- **Type**: Integer (1 digit)
- **Description**: Hierarchical importance of the boundary
- **Required**: Yes
- **Valid Values**:
  - 1 - Major plate boundary (well-defined, high seismicity)
  - 2 - Secondary boundary (microplate, diffuse zone)
  - 3 - Tertiary boundary (poorly constrained)
- **Examples**: 1, 2
- **Notes**: Use level=1 for primary tectonic analysis; higher levels for comprehensive studies

#### length
- **Type**: Numeric (24.15 format)
- **Description**: Length of the boundary segment
- **Required**: Yes (auto-calculated)
- **Units**: kilometers (approximate in WGS84)
- **Range**: 1 to >10,000 km
- **Examples**: 48.231534946570726, 633.711340323171385
- **Notes**: Calculated in geographic coordinates; use equal-distance projection for accurate length

---

## Ocean-Continent Boundaries (oc_boundaries.shp)

**Geometry Type**: Polyline  
**Record Count**: 93  
**Coordinate System**: WGS84 (EPSG:4326)

### Fields

#### id
- **Type**: Integer (10 digits)
- **Description**: Unique sequential identifier for each boundary segment
- **Required**: Yes
- **Range**: 1-93
- **Example**: 1, 2, 3
- **Notes**: Primary key; represents distinct segments of the global ocean-continent transition

#### length
- **Type**: Numeric (24.15 format)
- **Description**: Length of the boundary segment
- **Required**: Yes (auto-calculated)
- **Units**: kilometers (approximate in WGS84)
- **Range**: 10 to >5,000 km
- **Notes**: Represents the edge of continental crust; critical for crustal type mapping

**Usage Notes**: 
- This dataset delineates where oceanic crust transitions to continental crust
- Important for studies of passive margins, rifted margins, and continental shelves
- Combines with plates.shp crust_type field to create seamless crustal maps
- Does not represent modern coastlines (see GSHHS data for that purpose)

---

## Global Geologic Provinces (global_gprv.shp)

**Geometry Type**: Polygon  
**Record Count**: 914  
**Coordinate System**: WGS84 (EPSG:4326)

### Fields

#### id
- **Type**: Integer (10 digits)
- **Description**: Unique sequential identifier for each province
- **Required**: Yes
- **Range**: 1-914
- **Example**: 1, 2, 3
- **Notes**: Primary key for the dataset

#### prov_name
- **Type**: Text (80 characters)
- **Description**: Official or commonly used name of the geologic province
- **Required**: Yes
- **Examples**:
  - "Gariep Belt"
  - "Rehoboth Block"
  - "Okwa Terrane"
  - "Superior Province"
- **Notes**: Names follow regional geological literature; may include "Belt", "Block", "Terrane", "Province"

#### prov_type
- **Type**: Text (80 characters)
- **Description**: Classification of province based on tectonic setting and evolution
- **Required**: Yes
- **Valid Values**:
  - "shield" - Stable cratonic area with exposed Precambrian rocks
  - "platform" - Cratonic region with thin sedimentary cover
  - "orogenic belt" - Linear zone of mountain building
  - "basin" - Sedimentary accumulation area
  - "rift" - Extensional zone
  - "craton" - Ancient stable continental nucleus
  - "large igneous province" - Massive volcanic accumulation
  - "terrane" - Fault-bounded crustal block
- **Examples**: "orogenic belt", "shield", "basin"
- **Notes**: Multiple criteria used including structure, metamorphism, age, and origin

#### prov_ref
- **Type**: Text (80 characters)
- **Description**: Primary literature reference for province definition
- **Required**: Yes
- **Format**: DOI or author/year
- **Examples**:
  - "doi:10.1144/jgs2012-059"
  - "doi:10.1016/j.precamres.2015.04.015"
- **Notes**: Enables verification and further reading; critical for scientific reproducibility

#### prov_group
- **Type**: Text (80 characters)
- **Description**: Higher-order grouping (e.g., craton name)
- **Required**: Conditional
- **Examples**:
  - "Kalahari Craton"
  - "North American Craton"
  - "Amazonian Craton"
- **Notes**: Empty for provinces not part of a recognized craton or larger entity

#### lastorogen
- **Type**: Text (80 characters)
- **Description**: Last major orogenic event affecting the province
- **Required**: Conditional (primarily for continental provinces)
- **Examples**:
  - "Kuunga" (~500 Ma, Pan-African)
  - "Yavapai-Mazatzal" (~1.8-1.6 Ga, Paleoproterozoic)
  - "Trans Hudson" (~1.9-1.8 Ga)
  - "Grenville" (~1.3-1.0 Ga)
  - "Archean" (>2.5 Ga)
- **Notes**: Represents the timing of last major thermal/deformation event; earlier events may be recorded in the rocks

#### continent
- **Type**: Text (80 characters)
- **Description**: Current continental affinity
- **Required**: Yes
- **Valid Values**:
  - "Africa"
  - "Antarctica"
  - "Asia"
  - "Australia"
  - "Europe"
  - "North America"
  - "South America"
  - "Oceanic" (for oceanic provinces)
- **Examples**: "Africa", "North America"
- **Notes**: Present-day continental association; use conjugate fields for palinspastic reconstruction

#### conjugate1
- **Type**: Text (80 characters)
- **Description**: Primary conjugate province across a rifted margin
- **Required**: Conditional (for rifted margins)
- **Examples**:
  - Province name on opposite side of ocean basin
- **Notes**: Critical for plate reconstruction; identifies formerly adjacent terranes before continental breakup

#### comment
- **Type**: Text (254 characters)
- **Description**: Additional notes, uncertainties, or special characteristics
- **Required**: No
- **Examples**:
  - "Boundary uncertain in this region"
  - "Multiple orogenic overprints"
- **Notes**: Important qualifications or caveats about the province definition

#### crust_type
- **Type**: Text (12 characters)
- **Description**: Fundamental crustal composition
- **Required**: Yes
- **Valid Values**:
  - "continental"
  - "oceanic"
- **Examples**: "continental", "oceanic"
- **Notes**: Matches crust_type in plates.shp for consistency

#### conjugate2
- **Type**: Text (64 characters)
- **Description**: Secondary conjugate province (for complex rift systems)
- **Required**: No
- **Examples**: Province name
- **Notes**: Used in multi-phase rifting scenarios

#### area
- **Type**: Numeric (24.15 format)
- **Description**: Area of province polygon
- **Required**: Yes (auto-calculated)
- **Units**: km²
- **Range**: ~100 to >1,000,000
- **Examples**: 42463.623846736009000, 280239.959225072525442
- **Notes**: Calculated in WGS84; use equal-area projection for statistical analysis

---

## Cratons (cratons.shp)

**Geometry Type**: Polygon  
**Record Count**: 114  
**Coordinate System**: WGS84 (EPSG:4326)

### Fields

#### id
- **Type**: Text (80 characters)
- **Description**: Unique identifier for craton polygon
- **Required**: Yes
- **Notes**: May be text-based for compatibility with source datasets

#### prov_name
- **Type**: Text (80 characters)
- **Description**: Name of the cratonic province
- **Required**: Yes
- **Examples**:
  - "Superior Province"
  - "Kaapvaal Craton"
  - "Yilgarn Craton"
- **Notes**: Archean (>2500 Ma) basement regions

#### prov_type
- **Type**: Text (80 characters)
- **Description**: Province classification
- **Required**: Yes
- **Valid Values**: Typically "shield" or "craton"
- **Notes**: Extracted from global_gprv.shp with additional constraints

#### prov_ref
- **Type**: Text (80 characters)
- **Description**: Primary literature reference
- **Required**: Yes
- **Format**: DOI or citation
- **Notes**: Source for age determination and extent

#### prov_group
- **Type**: Text (80 characters)
- **Description**: Craton or superterrane name
- **Required**: Conditional
- **Examples**: "Superior Craton", "Kalahari Craton"

#### lastorogen
- **Type**: Text (80 characters)
- **Description**: Last major orogenic event
- **Required**: Yes
- **Notes**: For cratons, often Archean or Paleoproterozoic

#### continent
- **Type**: Text (80 characters)
- **Description**: Current continental location
- **Required**: Yes

#### crust_type
- **Type**: Text (80 characters)
- **Description**: Crustal composition
- **Required**: Yes
- **Value**: Typically "continental"

#### area
- **Type**: Numeric (24.15 format)
- **Description**: Polygon area
- **Required**: Yes (auto-calculated)
- **Units**: km²

#### PLATEID1
- **Type**: Integer (9 digits)
- **Description**: Plate reconstruction identifier (from GPlates)
- **Required**: Conditional
- **Notes**: Used for plate tectonic reconstructions through time

#### GPGIM_TYPE
- **Type**: Text (80 characters)
- **Description**: GPlates Geological Information Model type
- **Required**: Conditional
- **Notes**: Standardized feature type for plate reconstruction software

#### FROMAGE
- **Type**: Numeric (24.15 format)
- **Description**: Oldest age constraint for the craton
- **Required**: Conditional
- **Units**: Million years (Ma)
- **Range**: Typically >2500 Ma
- **Notes**: Defines Archean basement

#### TOAGE
- **Type**: Numeric (24.15 format)
- **Description**: Youngest age constraint
- **Required**: Conditional
- **Units**: Million years (Ma)
- **Notes**: May be 0 (present) or age of last reworking

#### conjugate1
- **Type**: Text (80 characters)
- **Description**: Conjugate craton across rifted margin
- **Required**: Conditional
- **Notes**: For paleogeographic reconstruction

#### PLATEID2
- **Type**: Integer (9 digits)
- **Description**: Secondary plate ID (for conjugate)
- **Required**: Conditional

#### comment
- **Type**: Text (80 characters)
- **Description**: Additional notes
- **Required**: No

#### RECON_METH
- **Type**: Text (80 characters)
- **Description**: Reconstruction method used
- **Required**: Conditional
- **Notes**: Describes the plate reconstruction technique

#### L_PLATE
- **Type**: Integer (9 digits)
- **Description**: Left plate ID (for boundaries)
- **Required**: Conditional

#### R_PLATE
- **Type**: Integer (9 digits)
- **Description**: Right plate ID (for boundaries)
- **Required**: Conditional

#### SPREAD_ASY
- **Type**: Numeric (24.15 format)
- **Description**: Spreading asymmetry parameter
- **Required**: Conditional
- **Notes**: Used in ocean basin reconstructions

#### IMPORT_AGE
- **Type**: Numeric (24.15 format)
- **Description**: Age used for import into reconstruction model
- **Required**: Conditional
- **Units**: Ma

#### reworked
- **Type**: Text (3 characters)
- **Description**: Indicates post-Archean reworking
- **Required**: Yes
- **Valid Values**:
  - "yes" - Significant post-Archean metamorphism/deformation
  - "no" - Pristine Archean
- **Examples**: "yes", "no"
- **Notes**: Critical for identifying tectonically stable vs. reactivated cratons

---

## GSHHS Shoreline (GSHHS_I_L1.shp)

**Geometry Type**: Polygon  
**Record Count**: 6,042  
**Coordinate System**: WGS84 (EPSG:4326)

### Fields

#### id
- **Type**: Text (80 characters)
- **Description**: Unique identifier for each shoreline polygon
- **Required**: Yes

#### level
- **Type**: Integer (9 digits)
- **Description**: Hierarchical level
- **Required**: Yes
- **Values**:
  - 1 - Continental land masses and ocean
  - 2 - Lakes
  - 3 - Islands in lakes
  - 4 - Ponds on islands in lakes
- **Notes**: Nested hierarchy for complete topological representation

#### source
- **Type**: Text (80 characters)
- **Description**: Data source for the shoreline segment
- **Required**: Yes
- **Examples**: "WDB", "GSHHG"
- **Notes**: World Database (WDB) or Global Self-consistent Hierarchical High-resolution Geography

#### parent_id
- **Type**: Integer (9 digits)
- **Description**: ID of the containing polygon
- **Required**: Conditional
- **Notes**: Used to establish hierarchical relationships (e.g., lake within continent)

#### sibling_id
- **Type**: Integer (9 digits)
- **Description**: ID of related sibling polygon
- **Required**: Conditional
- **Notes**: For complex polygons with multiple components

#### area
- **Type**: Numeric (24.15 format)
- **Description**: Polygon area
- **Required**: Yes (auto-calculated)
- **Units**: Approximately km² in WGS84
- **Notes**: High-resolution (~1 km) shoreline suitable for regional to global mapping

---

## Large Igneous Provinces (Johansson_etal_2018_EarthByte_LIPs_v2.shp)

**Geometry Type**: Polygon  
**Record Count**: 2,526  
**Coordinate System**: WGS84 (EPSG:4326)

### Fields

#### PLATEID1
- **Type**: Integer (9 digits)
- **Description**: Primary plate ID for reconstruction
- **Required**: Yes
- **Notes**: Links to GPlates rotation model

#### TYPE
- **Type**: Text (80 characters)
- **Description**: Classification of igneous province
- **Required**: Yes
- **Valid Values**:
  - "Large Igneous Province"
  - "Oceanic Plateau"
  - "Continental Flood Basalt"
  - "Volcanic Rifted Margin"
- **Notes**: Based on size, composition, and tectonic setting

#### FROMAGE
- **Type**: Numeric (24.15 format)
- **Description**: Oldest age of magmatism
- **Required**: Yes
- **Units**: Million years (Ma)
- **Range**: 0 to ~3500 Ma
- **Notes**: Start of main magmatic pulse

#### TOAGE
- **Type**: Numeric (24.15 format)
- **Description**: Youngest age of magmatism
- **Required**: Yes
- **Units**: Million years (Ma)
- **Notes**: End of main magmatic activity; may be 0 for active provinces

#### NAME
- **Type**: Text (80 characters)
- **Description**: Name of the Large Igneous Province
- **Required**: Yes
- **Examples**:
  - "Deccan Traps"
  - "Ontong Java Plateau"
  - "Central Atlantic Magmatic Province"
  - "Siberian Traps"
- **Notes**: Established names from LIP literature

#### DESCR
- **Type**: Text (80 characters)
- **Description**: Brief description or additional name
- **Required**: No
- **Notes**: May include alternative names or key characteristics

#### GPGIM_TYPE
- **Type**: Text (80 characters)
- **Description**: GPlates Information Model feature type
- **Required**: Yes
- **Notes**: Standardized classification for plate reconstruction

#### FEATURE_ID
- **Type**: Text (80 characters)
- **Description**: Unique feature identifier
- **Required**: Yes
- **Notes**: Consistent with GPlates feature collection

#### REF
- **Type**: Text (80 characters)
- **Description**: Primary reference for LIP data
- **Required**: Yes
- **Format**: Typically DOI or author/year
- **Notes**: Source for age, extent, and classification

#### PLATEID2
- **Type**: Integer (9 digits)
- **Description**: Secondary plate ID (for conjugate margins)
- **Required**: Conditional
- **Notes**: Used when LIP spans multiple plates or has conjugate components

#### RECON_METH
- **Type**: Text (80 characters)
- **Description**: Plate reconstruction method
- **Required**: Conditional
- **Values**: "Half-stage rotation", "Spreading history", etc.
- **Notes**: Describes how the feature is reconstructed through time

#### L_PLATE
- **Type**: Integer (9 digits)
- **Description**: Left plate in reconstruction
- **Required**: Conditional

#### R_PLATE
- **Type**: Integer (9 digits)
- **Description**: Right plate in reconstruction
- **Required**: Conditional

#### SPREAD_ASY
- **Type**: Numeric (24.15 format)
- **Description**: Spreading asymmetry parameter
- **Required**: Conditional
- **Range**: 0-1 (0.5 = symmetric)
- **Notes**: For oceanic plateaus formed at spreading centers

#### IMPORT_AGE
- **Type**: Numeric (24.15 format)
- **Description**: Age used for import into plate model
- **Required**: Conditional
- **Units**: Ma
- **Notes**: Typically uses FROMAGE or midpoint age

---

## Data Type Reference

### Field Type Abbreviations
- **C** (Character/Text): String data
- **N** (Numeric): Integer or floating-point numbers
- **D** (Date): Date values (not used in these datasets)
- **L** (Logical): Boolean true/false (not used in these datasets)

### Coordinate System Information
All datasets use:
- **Projection**: Geographic (unprojected)
- **Datum**: WGS84
- **EPSG Code**: 4326
- **Units**: Decimal degrees

For accurate area and distance calculations, reproject to an appropriate equal-area or equal-distance projection.

### Null/Empty Values
- Text fields: Empty string "" indicates no data
- Numeric fields: 0 or specific null value indicators (varies by software)
- Always check for empty/null values before analysis

---

## Usage Guidelines

### Joining Datasets
- Use `plate_code` or `plate_id` to join plates.shp with external plate motion data
- Use `plate1`/`plate2` in plate_boundaries.shp to join with plates.shp
- Use `prov_name` to join global_gprv.shp with geochronological databases
- Use `PLATEID1` to join cratons.shp and LIPs.shp with GPlates rotation files

### Attribute Queries
Examples of useful attribute queries:
```sql
-- Select all cratons that have been reworked
SELECT * FROM cratons WHERE reworked = 'yes'

-- Select spreading centers
SELECT * FROM plate_boundaries WHERE type = 'spreading center'

-- Select orogenic belts from a specific orogeny
SELECT * FROM global_gprv WHERE lastorogen = 'Grenville'

-- Select oceanic crust older than 100 Ma
SELECT * FROM plates WHERE crust_type = 'oceanic' AND domain = 'Atlantic'

-- Select Large Igneous Provinces from the Cretaceous (66-145 Ma)
SELECT * FROM LIPs WHERE FROMAGE >= 66 AND TOAGE <= 145
```

### Quality Assurance
- Always visualize data before analysis to check for unexpected geometries
- Verify coordinate system is appropriate for your application
- Check for topology errors (gaps, overlaps) when using for spatial analysis
- Cross-reference attribute values with the literature using provided DOIs

### Common Pitfalls
- **Area calculations**: Areas in geographic coordinates (degrees) are distorted; reproject first
- **Text field case**: Field names and some values are case-sensitive
- **Null vs. empty**: Different GIS software handles empty fields differently
- **Geometry precision**: Global-scale data has simplified boundaries; not suitable for local studies
- **Version control**: Always note which version of the dataset you're using

---

## Updates and Corrections

If you identify errors or have suggestions for improvements:
1. Check the GitHub repository for existing issues
2. Open a new issue with clear description and evidence
3. For major corrections, provide literature references
4. See USER_GUIDE.md for contribution workflow

---

## Additional Resources

- **Complete methodology**: See Hasterok et al. (2022) in Earth Science Reviews
- **Usage examples**: See USER_GUIDE.md
- **Dataset overview**: See DATA_EXPLANATION.md
- **Project information**: See README.md
- **Zenodo repository**: https://doi.org/10.5281/zenodo.5093930 (includes QGIS project)

---

*Last updated: 2026 (dataset version aligned with repository)*  
*For questions: derrick.hasterok@adelaide.edu.au*
