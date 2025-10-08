# ECOSTRESS STARS NDVI & Albedo User Guide

**ECOsystem Spaceborne Thermal Radiometer Experiment on Space Station (SBG)**

**Version 2**  
**August 13, 2024**

## Authors

**Gregory Halverson**  
SBG Science Team  
Jet Propulsion Laboratory  
California Institute of Technology

**Kerry Cawse-Nicholson**  
SBG Science Team  
Jet Propulsion Laboratory  
California Institute of Technology

**Margaret Johnson**  
SBG Science Team  
Jet Propulsion Laboratory  
California Institute of Technology

**Claire Villanueva-Weeks**  
SBG Science Team  
Jet Propulsion Laboratory  
California Institute of Technology

---

*© 2024 California Institute of Technology. Government sponsorship acknowledged.*

**National Aeronautics and Space Administration**  
Jet Propulsion Laboratory  
4800 Oak Grove Drive  
Pasadena, California 91109-8099  
California Institute of Technology

National Aeronautics and Space Administration

Jet Propulsion Laboratory

4800 Oak Grove Drive

Pasadena, California 91109-8099

California Institute of Technology

This research was carried out at the Jet Propulsion Laboratory,
California Institute of Technology, under a contract with the National
Aeronautics and Space Administration.

Reference herein to any specific commercial product, process, or service
by trade name, trademark, manufacturer, or otherwise, does not
constitute or imply its endorsement by the United States Government or
the Jet Propulsion Laboratory, California Institute of Technology.

© 2024. California Institute of Technology. Government sponsorship
acknowledged.

## Note

The users' guide is designed to be a living document that describes the SBG data products. The document describes the current state of the art
and is revised as progress is made in the development and assessment of
the SBG data products. The primary purpose of the document is to present
an overview of the SBG data products to the potential user. For more
detailed information on the physical basis and algorithms used to
produce these products, please see the Algorithm Theoretical Basis
Document (ATBD).

**Change History Log**

  ----------------------------------------------------------------------------
  **Revision**   **Effective     **Prepared by**    **Description of Changes**
                 Date**                             
  -------------- --------------- ------------------ --------------------------
  Draft          8/12/2024       Claire             STARS user Guide first
                                 Villanueva-Weeks   draft

  ----------------------------------------------------------------------------

**Contacts**

Readers seeking additional information about this product may contact
the following:

- Gregory Halverson\
  Jet Propulsion Laboratory\
  4800 Oak Grove Dr.\
  Pasadena, CA 91109\
  Email: <gregory.h.halverson@jpl.nasa.gov>\
  Office: (626) 660-6818

<!-- -->

- Kerry Cawse-Nicholson

  MS 183-501\
  Jet Propulsion Laboratory\
  4800 Oak Grove Dr.\
  Pasadena, CA 91109\
  Email: <kerry-anne.cawse-nicholson@jpl.nasa.gov>\
  Office: (818) 354-1594

- Margaret Johnson\
  Jet Propulsion Laboratory\
  4800 Oak Grove Dr.\
  Pasadena, CA 91109\
  Email: <maggie.johnson@jpl.nasa.gov>

  Office: (818) 354-8885

<!-- -->

- Claire Villanueva-Weeks

  Jet Propulsion Laboratory\
  4800 Oak Grove Dr.\
  Pasadena, CA 91109

  Email: <claire.s.villanueva-weeks@jpl.nasa.gov>

  Office: (818) 354-9268

## Table of Contents

1. [Introduction](#introduction)
   - [Cloud-Optimized GeoTIFF Orbit/Scene/Tile Products](#cloud-optimized-geotiff-orbitscenetile-products)
   - [Quality Flags](#quality-flags)
   - [Product Availability](#product-availability)
2. [L2T STARS NDVI and Albedo Product](#l2t-stars-ndvi-and-albedo-product)
3. [Standard Metadata](#standard-metadata)
4. [Acknowledgements](#acknowledgements)
5. [Bibliography](#bibliography)

### List of Tables

- Table 1: Listing of SBG tiled products long names and short names
- Table 2: Listing of the L2T STARS data layers
- Table 8: Name and type of metadata fields contained in the common StandardMetadata group
- Table 9: Name and type of metadata fields contained in the common ProductMetadata group

[Table 5. Listing of the L4T ESI data layers.
[13](#_Toc169011115)](#_Toc169011115)



# 1. Introduction

This is the user guide for the SBG tiled products. SBG acquires data within an orbit, and this orbit path is divided into scenes roughly 935 x 935 km in size. The SBG orbit/scene/tile products are distributed in Cloud-Optimized GeoTIFF (COG) format. The tiled products are listed in Table 1.

| Product Long Name | Product Short Name |
|---|---|
| STARS NDVI/Albedo | L2T STARS |
| Surface Energy Balance | L3T SEB |
| Soil Moisture | L3T SM |
| Meteorology | L3T MET |
| Evapotranspiration Ensemble | L3T ET |
| DisALEXI-JPL Evapotranspiration | L3T ET ALEXI |
| Evaporative Stress Index | L4T ESI |
| DisALEXI-JPL Evaporative Stress Index | L4T ESI ALEXI |
| Water Use Efficiency | L4T WUE |

**Table 1.** Listing of SBG tiled products long names and short names.

## Cloud-Optimized GeoTIFF Orbit/Scene/Tile Products 

To provide an analysis-ready format, the SBG products are distributed in
a tiled form and using the COG format. The tiled products include the
letter T in their level identifiers: L1CT, L2T, L3T, and L4T. The tiling
system used for SBG is borrowed from the modified Military Grid
Reference System (MGRS) tiling scheme used by Sentinel 2. These tiles
divide the Universal Transverse Mercator (UTM) zones into square tiles
109800 m across. SBG uses a 60 m cell size with 1830 rows by 1830
columns in each tile, totaling 3.35 million pixels per tile. This allows
the end user to assume that each 60 m SBG pixel will remain in the same
location at each timestep observed in analysis. The COG format also
facilitates end-user analysis as a universally recognized and supported
format, compatible with open-source software, including QGIS, ArcGIS,
GDAL, the Raster package in R, rioxarray in Python, and Rasters.jl in
Julia.

Each float32 data layer occupies 4 bytes of storage per pixel, which
amounts to an uncompressed size of 13.4 mb for each tiled data layer.
The uint8 quality flag layers occupy a single byte per pixel, which
amounts to an uncompressed size of 3.35 mb per tiled data quality layer.

Each .tif COG data layer in each L2T/L3T/L4T product additionally
contains a rendered browse image in GeoJPEG format with a .jpeg
extension. This image format is universally recognized and supported,
and these files are compatible with Google Earth. Each L2T/L3T/L4T tile
granule includes a .json file containing the Product Metadata and
Standard Metadata in JSON format.

## Quality Flags

Two high-level quality flags are provided in all gridded and tiled
products as thematic/binary masks encoded to zero and one in unsigned
8-bit integer layers. The cloud layer represents the final cloud test
from L2 CLOUD. The water layer represents the surface water body in the
Shuttle Radar Topography Mission (SRTM) Digital Elevation Model. For
both layers, zero means absence, and one means presence. Pixels with the
value 1 in the cloud layer represent detection of cloud in that pixel.
Pixels with the value 1 in the water layer represent open water surface
in that pixel. All tiled product data layers written in float32 contain
a standard not-a-number (NaN) value at each pixel that could not be
retrieved. The cloud and water layers are provided to explain these
missing values.

## Product Availability

The SBG products are available at the NASA Land Processes Distribution
Active Archive Center (LP-DAAC), https://earthdata.nasa.gov/ and can be
accessed via the Earthdata search engine.

# 2. L2T STARS NDVI and Albedo Product

NDVI and albedo are estimated at 60 m SBG standard resolution for each
daytime SBG overpass by fusing temporally sparse but fine spatial
resolution images from the Harmonized Landsat Sentinel (HLS) 2.0 product
with daily, moderate spatial resolution images from the Suomi NPP
Visible Infrared Imaging Radiometer Suite (VIIRS) VNP09GA product. The
data fusion is performed using a variant of the Spatial Timeseries for
Automated high-Resolution multi-Sensor data fusion (STARS) algorithm
developed by Dr. Margaret Johnson and Gregory Halverson at the Jet
Propulsion Laboratory. STARS is a Bayesian timeseries methodology that
provides streaming data fusion and uncertainty quantification through
efficient Kalman filtering.

Operationally, each L2T STARS tile run loads the means and covariances
of the STARS model saved from the most recent tile run, then iteratively
advances the means and covariances forward each day updating with fine
imagery from HLS and/or moderate resolution imagery from VIIRS up to the
day of the target SBG overpass. A pixelwise, lagged 16-day
implementation of the VNP43 algorithm (Schaaf, 2017) is used for a
near-real-time BRDF correction on the VNP09GA products to produce VIIRS
NDVI and albedo.

Operationally, each L2T STARS tile run loads the means and covariances
of the STARS model saved from the most recent tile run, then iteratively
advances the means and covariances forward each day updating with fine
imagery from HLS and/or moderate resolution imagery from VIIRS up to the
day of the target SBG overpass. A pixelwise, lagged 16-day
implementation of the VNP43 algorithm (Schaaf, 2017) is used for a
near-real-time BRDF correction on the VNP09GA products to produce VIIRS
NDVI and albedo. The layers of the L2T STARS product are listed in Table
2. All layers of this product are represented by 32-bit floating point
arrays. The NDVI estimates and 1σ uncertainties (-UQ) are
unitless from -1 to 1. The albedo estimates and 1σ uncertainties
(-UQ) are proportions from 0 to 1.

| Name | Description | Type | Units | Fill Value | No Data Value | Valid Min | Valid Max | Size |
|---|---|---|---|---|---|---|---|---|
| NDVI | Normalized Difference Vegetation Index | float32 | Index | NaN | N/A | -1 | 1 | 12.96 mb |
| NDVI-UQ | NDVI Uncertainty | float32 | Index | NaN | N/A | -1 | 1 | 12.96 mb |
| albedo | Albedo | float32 | Ratio | NaN | N/A | 0 | 1 | 12.96 mb |
| albedo-UQ | Albedo Uncertainty | float32 | Ratio | NaN | N/A | 0 | 1 | 12.96 mb |

**Table 2.** Listing of the L2T STARS data layers.

# 3. Standard Metadata

Each SBG product bundle contains two sets of product metadata:

- ProductMetadata

- StandardMetadata

Each product contains a custom set of ProductMetadata attributes, as
The ProductMetadata attributes are listed in Table 9. The StandardMetadata attributes are consistent across products at each orbit/scene, as listed in Table 8.

| Name | Type |
|---|---|
| AncillaryInputPointer | string |
| AutomaticQualityFlag | string |
| AutomaticQualityFlagExplanation | string |
| BuildID | string |
| CRS | string |
| CampaignShortName | string |
| CollectionLabel | string |
| DataFormatType | string |
| DayNightFlag | string |
| EastBoundingCoordinate | float |
| FieldOfViewObstruction | string |
| ImageLines | float |
| ImageLineSpacing | integer |
| ImagePixels | float |
| ImagePixelSpacing | integer |
| InputPointer | string |
| InstrumentShortName | string |
| LocalGranuleID | string |
| LongName | string |
| NorthBoundingCoordinate | float |
| PGEName | string |
| PGEVersion | string |
| PlatformLongName | string |
| PlatformShortName | string |
| PlatformType | string |
| ProcessingEnvironment | string |
| ProcessingLevelDescription | string |
| ProcessingLevelID | string |
| ProducerAgency | string |
| ProducerInstitution | string |
| ProductionDateTime | string |
| ProductionLocation | string |
| RangeBeginningDate | string |
| RangeBeginningTime | string |
| RangeEndingDate | string |
| RangeEndingTime | string |
| RegionID | string |
| SISName | string |
| SISVersion | string |
| SceneBoundaryLatLonWKT | string |
| SceneID | string |
| ShortName | string |
| SouthBoundingCoordinate | float |
| StartOrbitNumber | string |
| StopOrbitNumber | string |
| WestBoundingCoordinate | float |

**Table 8.** Name and type of metadata fields contained in the common StandardMetadata group in each L2T/L3T/L4T product.

| Name | Type |
|---|---|
| BandSpecification | float |
| NumberOfBands | integer |
| OrbitCorrectionPerformed | string |
| QAPercentCloudCover | float |
| QAPercentGoodQuality | float |
| AuxiliaryNWP | string |

**Table 9.** Name and type of metadata fields contained in the common ProductMetadata group in each L2T/L3T/L4T product.

# 4. Acknowledgements

We would like to thank Joshua Fisher as the initial science lead of the
SBG mission and PI of the ROSES project to re-design the SBG products.

We would like to thank Adam Purdy for contributing the PT-JPL-SM model.

We would like to thank Kaniska Mallick for contributing the STIC model.

We would like to thank Martha Anderson for contributing the DisALEXI-JPL
algorithm.

# 5. Bibliography

Schaaf, C. (2017). *VIIRS BRDF, Albedo, and NBAR Product Algorithm
Theoretical Basis Document (ATBD).* NASA Goddard Space Flight Center.
