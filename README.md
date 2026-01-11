# global_tectonics
Tectonic maps for data analysis applications in solid Earth science

## Documentation

For detailed information about using this dataset, please see:

- **[USER_GUIDE.md](USER_GUIDE.md)** - Complete usage instructions, workflows, and examples
- **[DATA_DICTIONARY.md](DATA_DICTIONARY.md)** - Detailed field definitions and data specifications
- **[DATA_EXPLANATION.md](DATA_EXPLANATION.md)** - Overview of datasets and scientific context

## Quick Start

1. Clone or download this repository
2. Navigate to `plates&provinces/shp/` for shapefiles, `gmt/` for GMT format, or `kml/` for Google Earth
3. Load the files in your preferred GIS software (QGIS, ArcGIS, etc.)
4. Apply pre-made styles from the `styles/` folder for instant visualization
5. See [USER_GUIDE.md](USER_GUIDE.md) for detailed tutorials and examples

## Dataset Overview

The global_tectonics package is a set of shapefiles that can be used for analysis of Earth science data. For each shapefile, there is an associated *.gmt file in Generic Mapping Tools (GMT) vector format. There are several models as part of the package, which includes a present-day plate model and a geologic province model. The models include metadata that permit the creation of several related (and seamless) maps including the lithospheric type (oceanic/continental), last orogenic event, and oceanic domains.

### Primary Datasets

- **plates.shp** - polygons of tectonic plates and crust types (280 features)
- **plate_boundaries.shp** - lines of plate boundary types (571 features)
- **oc_boundaries.shp** - lines demarcating the ocean-continent boundary (93 features)
- **global_gprv.shp** - polygons of global geologic provinces (914 features)
- **cratons.shp** - regions with geochemical samples or that are known to have Archean (>2500 Ma) basement (114 features); the polygons have largely been extracted from global_gprv.shp aside from a few in the western US from Lund et al. (2015, https://doi.org/10.3133%2Fds898); an additional column in the attribute table has been added to identify post-Archean reworking

All datasets are provided in three formats:
- **Shapefile (.shp)** - For use in GIS software (QGIS, ArcGIS, etc.)
- **GMT (.gmt)** - For Generic Mapping Tools and PyGMT
- **KML (.kml)** - For Google Earth and other KML-compatible software

## Updates

**2022 May 22**: Updated plates, boundaries, oc_boundaries and global_gprv following review.

**2021 July 19**: Initial public release of the global_tectonics package.

## Scientific Background

Details about the construction of the models is discussed in:

**Hasterok, D., Halpin, J., Hand, M., Collins, A., Kreemer, C., Gard, M.G., Glorie, S.** (2022)  
*New maps of global geologic provinces and tectonic plates*  
Earth Science Reviews  
Preprint: https://doi.org/10.31223/X5TD1C

## Additional Resources

A static version with additional global geophysical and tectonic datasets can be found with a QGIS project file on the Zenodo data repository:

**Hasterok, D., Halpin, J., Hand, M., Collins, A., Kreemer, C., Glorie, S.** (2022)  
*New maps of global geologic provinces and tectonic plates: global tectonics data and QGIS project file*  
Zenodo: https://doi.org/10.5281/zenodo.5093930

## Contributing

The model is intended to be updated as new knowledge is gained about the locations and processes affecting provinces. To do so requires community effort. The global_tectonics GitHub repository can facilitate these updates.

If you wish to contribute to this project:
- Open an issue to report errors or suggest improvements
- Submit a pull request with your changes
- Contact: derrick.hasterok@adelaide.edu.au

See [USER_GUIDE.md](USER_GUIDE.md) for detailed contribution guidelines.

## Citation

When using this dataset, please cite:

**Hasterok, D., Halpin, J., Hand, M., Collins, A., Kreemer, C., Gard, M.G., Glorie, S.** (2022)  
*New maps of global geologic provinces and tectonic plates*  
Earth Science Reviews  
Preprint: https://doi.org/10.31223/X5TD1C

**Dataset DOI**: https://doi.org/10.5281/zenodo.5093930

## Version History

- **Submitted** to Earth Science Reviews: 18 May 2022
- **Revised**: 23 May 2022
- **Accepted**: 26 May 2022

## License

This project is licensed under the GNU General Public License v3.0 (GPL-3.0). See the [LICENSE](LICENSE) file for details.
