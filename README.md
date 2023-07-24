# Getting spatial variables with Google Earth Engine

Code that can be used to extract various metrics to point count locations using Google Earth Engine, and export the data as a .csv file. The code should be used with earth engine's online code editor.


`all_metrics.js` is the main  script you should work with. Edit it to define your study area, point count locations, buffer size for zonal stats, and time period of interests. The script will call upon functions stored in the `functions` folder. The path to the functions .js files should be edited accordingly. `landsat_functions.js` calculates various spectral indices from landsat imagery. `HLI.js` contains functions for calculating heat load index. `TPI.js` contains functions for calculating standard terrain metrics including topographic position index. `timeseries_functions.js` contains functions that summarizes different metrics (landsat spectral indicees, climate, and landcover classifications) accross user defined time series. 



## Included metrics

| Variable                  | Description                                                                                                                                            | Resolution (m) | Source                                                      |
|:--------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------|---------------:|:------------------------------------------------------------|
| elevation                 | NA                                                                                                                                                     |          23.19 | Canada ([2015](#ref-natural2015canadian))                   |
| slope                     | NA                                                                                                                                                     |          23.19 | Canada ([2015](#ref-natural2015canadian))                   |
| aspect                    | NA                                                                                                                                                     |          23.19 | Canada ([2015](#ref-natural2015canadian))                   |
| northness                 | cosign of aspect in radians                                                                                                                            |          23.19 | Canada ([2015](#ref-natural2015canadian))                   |
| TPI                       | Topographic Position Index, TPI neigborhoods range from 50-1000 meters.                                                                                |          23.19 | Canada ([2015](#ref-natural2015canadian))                   |
| HLI                       | Heat Load Index                                                                                                                                        |          23.19 | Canada ([2015](#ref-natural2015canadian))                   |
| TWI                       | Topographic Wetness Index: ln(α/tanβ)) where α=cumulative upslope drainage area and β is slope ([Sørensen and Seibert 2007](#ref-sorensen2007effects)) |          92.77 | Yamazaki et al. ([2019](#ref-yamazaki2019merit))            |
| NDVI                      | Normalized Difference Vegetation Index: (NIR - R) / (NIR + R)                                                                                          |          30.00 | Survey ([2018](#ref-geologicalsurveyLandsat47Surface2018))  |
| EVI                       | Enhanced Vegetation Index: G \* ((NIR - R) / (NIR + C1 \* R – C2 \* B + L))                                                                            |          30.00 | Survey ([2018](#ref-geologicalsurveyLandsat47Surface2018))  |
| BSI                       | Bare Soil Index: ((R+SWIR) – (NIR+B)) / ((R+SWIR) + (NIR+B))                                                                                           |          30.00 | Survey ([2018](#ref-geologicalsurveyLandsat47Surface2018))  |
| SAVI                      | Soil Adjusted Vegetation Index: ((NIR - R) / (NIR + R + L)) \* (1 + L)                                                                                 |          30.00 | Survey ([2018](#ref-geologicalsurveyLandsat47Surface2018))  |
| NDMI                      | Normalized Difference Moisture Index: (NIR - SWIR) / (NIR + SWIR)                                                                                      |          30.00 | Survey ([2018](#ref-geologicalsurveyLandsat47Surface2018))  |
| SI                        | Shadow index: (1 - B) \* (1 - G) \* (1 - R)                                                                                                            |          30.00 | Survey ([2018](#ref-geologicalsurveyLandsat47Surface2018))  |
| LAI                       | Leaf Area Index: 3.618 *(2.5 * ((NIR - R) / (NIR + 6 \* R - 7.5 \* B + 1)))-0.118                                                                      |          30.00 | Survey ([2018](#ref-geologicalsurveyLandsat47Surface2018))  |
| NDSI                      | Normalized Difference Snow Index: (G - SWIR) / (G- SWIR)                                                                                               |          30.00 | Survey ([2018](#ref-geologicalsurveyLandsat47Surface2018))  |
| snow                      | percent of snow cover                                                                                                                                  |          30.00 | Survey ([2018](#ref-geologicalsurveyLandsat47Surface2018))  |
| discrete_classification   | Land cover classification                                                                                                                              |         100.00 | Buchhorn et al. ([2020](#ref-buchhorn2020copernicus))       |
| forest_type               | Forest type for all pixels with tree cover \> 1 %                                                                                                      |         100.00 | Buchhorn et al. ([2020](#ref-buchhorn2020copernicus))       |
| tree-coverfraction        | Percent of forest vegetation cover                                                                                                                     |         100.00 | Buchhorn et al. ([2020](#ref-buchhorn2020copernicus))       |
| cloud_fraction            | Fractional cloud cover                                                                                                                                 |       11132.00 | Heidinger et al. ([2014](#ref-heidinger2014noaa))           |
| cloud_probabilty          | probability of cloud cover                                                                                                                             |       11132.00 | Heidinger et al. ([2014](#ref-heidinger2014noaa))           |
| aet                       | Actual evapotranspiration                                                                                                                              |        4638.30 | Abatzoglou et al. ([2018](#ref-abatzoglou2018terraclimate)) |
| def                       | Climate water deficit                                                                                                                                  |        4638.30 | Abatzoglou et al. ([2018](#ref-abatzoglou2018terraclimate)) |
| pdsi                      | Palmer Drought Severity Index                                                                                                                          |        4638.30 | Abatzoglou et al. ([2018](#ref-abatzoglou2018terraclimate)) |
| pet                       | Reference evapotranspiration (ASCE Penman-Montieth)                                                                                                    |        4638.30 | Abatzoglou et al. ([2018](#ref-abatzoglou2018terraclimate)) |
| pr                        | Precipitation accumulation                                                                                                                             |        4638.30 | Abatzoglou et al. ([2018](#ref-abatzoglou2018terraclimate)) |
| ro                        | Runof                                                                                                                                                  |        4638.30 | Abatzoglou et al. ([2018](#ref-abatzoglou2018terraclimate)) |
| soil                      | Soil moisture                                                                                                                                          |        4638.30 | Abatzoglou et al. ([2018](#ref-abatzoglou2018terraclimate)) |
| srad                      | Downward surface shortwave radiation                                                                                                                   |        4638.30 | Abatzoglou et al. ([2018](#ref-abatzoglou2018terraclimate)) |
| swe                       | Snow water equivalent                                                                                                                                  |        4638.30 | Abatzoglou et al. ([2018](#ref-abatzoglou2018terraclimate)) |
| vap                       | Vapor pressure                                                                                                                                         |        4638.30 | Abatzoglou et al. ([2018](#ref-abatzoglou2018terraclimate)) |
| vpd                       | Vapor pressure deficit                                                                                                                                 |        4638.30 | Abatzoglou et al. ([2018](#ref-abatzoglou2018terraclimate)) |
| vs                        | Wind-speed at 10m                                                                                                                                      |        4638.30 | Abatzoglou et al. ([2018](#ref-abatzoglou2018terraclimate)) |
| CHILI                     | Continuous Heat-Insolation Load Index                                                                                                                  |          90.00 | Theobald et al. ([2015](#ref-theobald2015ecologically))     |
| canopy_height             | model trained with GEDI LiDAR to retrieve canopy height from Sentinel-2 images                                                                         |          10.00 | Lang et al. ([2022](#ref-lang2022high))                     |
| canopy_standard_deviation | model trained with GEDI LiDAR to retrieve standard deviation of canopy height from Sentinel-2 images                                                   |          10.00 | Lang et al. ([2022](#ref-lang2022high))                     |

Spatial covariates evaluated and used in our analysis.


# References

<div id="refs" class="references csl-bib-body hanging-indent">

<div id="ref-abatzoglou2018terraclimate" class="csl-entry">

Abatzoglou, John T, Solomon Z Dobrowski, Sean A Parks, and Katherine C
Hegewisch. 2018. “TerraClimate, a High-Resolution Global Dataset of
Monthly Climate and Climatic Water Balance from 1958–2015.” *Scientific
Data* 5 (1): 1–12.

</div>

<div id="ref-buchhorn2020copernicus" class="csl-entry">

<div id="ref-buchhorn2020copernicus" class="csl-entry">

Buchhorn, Marcel, Myroslava Lesiv, Nandin-Erdene Tsendbazar, Martin
Herold, Luc Bertels, and Bruno Smets. 2020. “Copernicus Global Land
Cover Layers—Collection 2.” *Remote Sensing* 12 (6): 1044.

</div>

<div id="ref-natural2015canadian" class="csl-entry">

Canada, Natural Resources. 2015. “Canadian Digital Elevation Model,
1945–2011.” Natural Resources Canada Ottawa, ON, Canada.

</div>

<div id="ref-heidinger2014noaa" class="csl-entry">

<div id="ref-lang2022high" class="csl-entry">

Lang, Nico, Walter Jetz, Konrad Schindler, and Jan Dirk Wegner. 2022. “A
High-Resolution Canopy Height Model of the Earth.” *arXiv Preprint
arXiv:2204.08322*.

</div>


Heidinger, AK, Michael J Foster, Andi Walther, and Xuepeng Zhao. 2014.
“NOAA Climate Data Record (CDR) of Cloud Properties from AVHRR
Pathfinder Atmospheres-Extended (PATMOS-x), Version 5.3.” *NOAA National
Centers for Environmental Information: NOAA CDR Program*.

</div>

Survey, U. S. Geological. 2018. “Landsat 4-7 Surface Reflectance
(Ledaps) Product Guide.” Sioux Falls, SD, USA: Department of the
Interior, U.S. Geological Survey.

</div>

<div id="ref-theobald2015ecologically" class="csl-entry">

Theobald, David M, Dylan Harrison-Atlas, William B Monahan, and
Christine M Albano. 2015. “Ecologically-Relevant Maps of Landforms and
Physiographic Diversity for Climate Adaptation Planning.” *PloS One* 10
(12): e0143619.

</div>


Yamazaki, Dai, Daiki Ikeshima, Jeison Sosa, Paul D Bates, George H
Allen, and Tamlin M Pavelsky. 2019. “MERIT Hydro: A High-Resolution
Global Hydrography Map Based on Latest Topography Dataset.” *Water
Resources Research* 55 (6): 5053–73.

</div>

</div>

<!--chapter:end:index.Rmd-->

