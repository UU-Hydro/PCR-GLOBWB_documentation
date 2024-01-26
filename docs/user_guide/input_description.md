# PCR-GLOBWB input description

To run PCR-GLOBWB requires several inputs, as described in the configuration file (see also the [configure section](../configure)). Here we will describe the most important PCR-GLOBWB inputs, and the variables they contain, per configuration section.

Most input maps an either use a NetCDF file, a PCRaster file or a value (which is then assumed constant over the whole domain). Sometimes you can also use 'None', in which case a default value is assigned.

Input maps can have different spatial extents and spatio-temporal resolutions, as long as the map covers the spatial extent of the simulation (i.e. the cloneMap). Maps are automatically cropped to the cloneMap.

If the spatial resolution of the map is different, values are directly downscaled to the cloneMap.

If the temporal resolution of the map is different, values are extended to the simulation time: multi-year yearly, multi-year monthly, single-year monthly or single-year daily values are kept constant over the accompanying year/month/day. Simulations after the last year or before the first year use the yearly, monthly or daily values of the last or first year respectively.

## Global Options
| Name | Type | Description |
| --- | --- | --- |
| inputDir | Directory | Input directory, all other inputs should be relative to this directory |
| landmask | Directory | Output directory, all outputs are stored here; This directory is created automatically |
| cloneMap | PCRaster file | The spatial extent and resolution of the simulation |
| landmask | PCRaster/NetCDF file or None | The domain mask of the simulation; if None, the domain is limited to cells with a flow-direction value |
| institution | Text | Institution that is added to all output files |
| title | Text | Title  that is added to all output files |
| description | Text | Description that is added to all output files |
| --- | --- | --- |
| startTime | Date | Start of the simulation |
| endTime | Date | End of the simulation |
| maxSpinUpsInYears | Number | Years (count) used to spinup the model; Note that initial states for all storage values can also be set seperately to avoid using spinup |
| minConvForSoilSto | Number | Soil water storage convergence (m) (i.e. the difference between the start and end of the year) value for which the spinup can finish early |
| minConvForGwatSto | Number | Groundwater storage convergence (m) (i.e. the difference between the start and end of the year) value for which the spinup can finish early |
| minConvForChanSto | Number | Channel water storage convergence (m) (i.e. the difference between the start and end of the year) value for which the spinup can finish early |
| minConvForTotlSto | Number | Total water storage convergence (m) (i.e. the difference between the start and end of the year) value for which the spinup can finish early |

## Meteo Options
| Name | Type | Description |
| --- | --- | --- |
| precipitationNC | NetCDF file | Precipitation (m.day-1); should contain a single variable (with any name) with the precipitation over the simulation period |
| temperatureNC | NetCDF file | Temperature (degree_celsius); should contain a single variable (with any name) with the temperature over the simulation period |
| --- | --- | --- |
| referenceETPotMethod | "Input" or "Penman-Monteith" | Method to aquire the reference potential evapotranspiration; "Input" requires a reference potential evapotranspiration file which is used directly, while "Penman-Monteith" requires wind, pressure, shortwave radiation, relative humidity files to calculate the reference potential evapotranspiration |
| refETPotFileNC | NetCDF file | Reference potential evapotranspiration (m.day-1); should contain a single variable (with any name) with the reference potential evapotranspiration over the simulation period |
| wind_speed_10m | NetCDF file | Wind speed (m.s-1.day-1); should contain a single variable (with any name) with the 10 meter wind speed over the simulation period |
| atmospheric_pressure | NetCDF file | Atmospheric pressure (pascal.day-1); should contain a single variable (with any name) with the atmospheric pressure over the simulation period |
| shortwave_radiation | NetCDF file | Shortwave radiation (J.m-2.day-1); should contain a single variable (with any name) with the shortwave radiation over the simulation period |
| relative_humidity | NetCDF file | Relative humidity (m.m-1.day-1); should contain a single variable (with any name) with the relative humidity over the simulation period |

## Meteo Downscaling Options
| Name | Type | Description |
| --- | --- | --- |
| downscalePrecipitation | True/False | Flag to downscale precipitation |
| downscaleTemperature | True/False | Flag to downscale temperature |
| downscaleReferenceETPot | True/False | Flag to downscale reference potential evapotranspiration |
| meteoDownscaleIds | PCRaster/NetCDF file | ID where each (low resolution) meteorology cell has a unique ID |
| highResolutionDEM | PCRaster/NetCDF file | Elevation (m); at the (high) simulation resolution |
| temperLapseRateNC | PCRaster/NetCDF file | Temperature lapse rate (degree_celsius.m-1) |
| precipLapseRateNC | PCRaster/NetCDF file | Precipitation lapse rate (m.m-1) |

## Land Surface Options
| Name | Type | Description |
| --- | --- | --- |
| debugWaterBalance | True/False | Flag to check and log the land surface water balance |
| numberOfUpperSoilLayers | 2 or 3 | Soil layers (count); 2 soil layers uses an upper (30cm) and lower soil layer; 3 soil layers divides the upper soil layer into two layers (5cm and 25 cm) |
| --- | --- | --- |
| topographyNC | NetCDF file or None | Topography properties; requires tanslope (m.m-1), slopeLength (m), orographyBeta, dzRel0001 (m), dzRel0005 (m), dzRel0010 (m), dzRel0020 (m), dzRel0030 (m), dzRel0040 (m), dzRel0050 (m), dzRel0060 (m), dzRel0070 (m), dzRel0080 (m), dzRel0090 (m), dzRel0100 (m) variables described below; if None, variables need to be specified seperately |
| tanslope | PCRaster/NetCDF file or None | Slope (m.m-1); exclusive with topographyNC |
| slopeLength | PCRaster/NetCDF file or None | Slope length (m); exclusive with topographyNC |
| orographyBeta | PCRaster/NetCDF file or None | Orography beta; exclusive with topographyNC |
| dzRel0001 | PCRaster/NetCDF file or None | Relative elevation above floodplain (m), 1st area percentage; exclusive with topographyNC |
| dzRel0005 | PCRaster/NetCDF file or None | Relative elevation above floodplain (m), 5th area percentage; exclusive with topographyNC |
| dzRel0010 | PCRaster/NetCDF file or None | Relative elevation above floodplain (m), 10th area percentage; exclusive with topographyNC |
| dzRel0020 | PCRaster/NetCDF file or None | Relative elevation above floodplain (m), 20th area percentage; exclusive with topographyNC |
| dzRel0030 | PCRaster/NetCDF file or None | Relative elevation above floodplain (m), 30th area percentage; exclusive with topographyNC |
| dzRel0040 | PCRaster/NetCDF file or None | Relative elevation above floodplain (m), 40th area percentage; exclusive with topographyNC |
| dzRel0050 | PCRaster/NetCDF file or None | Relative elevation above floodplain (m), 50th area percentage; exclusive with topographyNC |
| dzRel0060 | PCRaster/NetCDF file or None | Relative elevation above floodplain (m), 60th area percentage; exclusive with topographyNC |
| dzRel0070 | PCRaster/NetCDF file or None | Relative elevation above floodplain (m), 70th area percentage; exclusive with topographyNC |
| dzRel0080 | PCRaster/NetCDF file or None | Relative elevation above floodplain (m), 80th area percentage; exclusive with topographyNC |
| dzRel0090 | PCRaster/NetCDF file or None | Relative elevation above floodplain (m), 90th area percentage; exclusive with topographyNC |
| dzRel0100 | PCRaster/NetCDF file or None | Relative elevation above floodplain (m), 100th area percentage; exclusive with topographyNC |
| --- | --- | --- |
| soilPropertiesNC | NetCDF file or None | Soil properties; requires airEntryValue1 (m), airEntryValue2 (m), poreSizeBeta1, poreSizeBeta2, resVolWC1 (m3.m-3), resVolWC2 (m3.m-3), satVolWC1 (m3.m-3), satVolWC2 (m3.m-3), KSat1 (m.day-1), KSat2 (m.day-1), percolationImp, firstStorDepth (m), secondStorDepth (m), soilWaterStorageCap1 (m), soilWaterStorageCap2 (m) variables described below; if None, variables need to be specified seperately |
| airEntryValue1 | PCRaster/NetCDF file or None | Upper soil layer air entry value (m) according to soil water retention curve of Clapp & Hornberger (1978)[@clapp78]; exclusive with soilPropertiesNC |
| airEntryValue2 | PCRaster/NetCDF file or None | Lower soil layer air entry value (m) according to soil water retention curve of Clapp & Hornberger (1978)[@clapp78]; exclusive with soilPropertiesNC |
| poreSizeBeta1 | PCRaster/NetCDF file or None | Upper soil layer pore size distribution parameter according to Clapp & Hornberger (1978)[@clapp78]; exclusive with soilPropertiesNC |
| poreSizeBeta2| PCRaster/NetCDF file or None | Lower soil layer pore size distribution parameter according to Clapp & Hornberger (1978)[@clapp78]; exclusive with soilPropertiesNC |
| resVolWC1 | PCRaster/NetCDF file or None | Upper soil layer residual volumetric moisture content (m); exclusive with soilPropertiesNC |
| satVolWC1 | PCRaster/NetCDF file or None | Upper soil layer saturated volumetric moisture content (m); exclusive with soilPropertiesNC |
| satVolWC2 | PCRaster/NetCDF file or None | Lower soil layer saturated volumetric moisture content (m); exclusive with soilPropertiesNC |
| KSat1| PCRaster/NetCDF file or None | Upper soil layer saturated hydraulic conductivity (m); exclusive with soilPropertiesNC |
| KSat2 | PCRaster/NetCDF file or None | Lower soil layer saturated hydraulic conductivity (m); exclusive with soilPropertiesNC |
| percolationImp | PCRaster/NetCDF file or None | Fractional area where percolation to groundwater store is impeded; exclusive with soilPropertiesNC |
| firstStorDepth | PCRaster/NetCDF file or None | Upper soil layer depth (m); exclusive with soilPropertiesNC |
| secondStorDepth | PCRaster/NetCDF file or None | Lower soil layer depth (m); exclusive with soilPropertiesNC |
| soilWaterStorageCap1 | PCRaster/NetCDF file or None | Upper soil layer water storage capacity (m); exclusive with soilPropertiesNC |
| soilWaterStorageCap2 | PCRaster/NetCDF file or None | Lower soil layer water storage capacity (m); exclusive with soilPropertiesNC |
| --- | --- | --- |
| includeIrrigation | True/False | Flag to include irrigation water demands |
| historicalIrrigationArea | PCRaster/NetCDF file | Irrigation area (ha) |
| irrigationEfficiency | PCRaster/NetCDF file | Irrigation efficiency (m3.m3-1) |
| --- | --- | --- |
| includeDomesticWaterDemand | True/False | Flag to include domestic water demands |
| includeIndustryWaterDemand | True/False | Flag to include industry water demands |
| includeLivestockWaterDemand | True/False | Flag to include livestock water demands |
| domesticWaterDemandFile | PCRaster/NetCDF file | Domestic water demand (m.day-1) |
| industryWaterDemandFile | PCRaster/NetCDF file | Industry water demand (m.day-1) |
| livestockWaterDemandFile | PCRaster/NetCDF file | Livestock water demand (m.day-1) |
| --- | --- | --- |
| desalinationWater | PCRaster/NetCDF file or None | Desalination potential water supply (m.day-1); if None, there is no deslination water supply |
| allocationSegmentsForGroundSurfaceWater | PCRaster/NetCDF file or None | ID where each (low resolution) water reallocation zone cell has a unique ID; if None, there is no water reallocation |
| irrigationSurfaceWaterAbstractionFractionData | PCRaster/NetCDF file or None | Preferred fraction of surface water resources allocated for irrigation (m3.m3-1); if None, surface water is always preferred |
| irrigationSurfaceWaterAbstractionFractionDataQuality | PCRaster/NetCDF file or None | Data quanlity of the preferred fraction of surface water resources allocated for irrigation and livestock (1-5); if None, surface water is always preferred |
| treshold_to_maximize_irrigation_surface_water | PCRaster/NetCDF file | Threshold when to use the preferred surface water allocation fraction (m3.m3-1) |
| treshold_to_minimize_fossil_groundwater_irrigation | PCRaster/NetCDF file | Threshold when to limit fossil groundwater abstractions based on the irrigation surface water allocation fraction (m3.m3-1) |
| maximumNonIrrigationSurfaceWaterAbstractionFractionData | PCRaster/NetCDF file or None | Maximum fraction of surface water resources allocated for domestic and industry (m3.m3-1); if None, there is no maximum |

## Land Cover Options
| Name | Type | Description |
| --- | --- | --- |
| debugWaterBalance | True/False | Flag to check and log the land cover water balance |
| name | text | Land cover type name |
| --- | --- | --- |
| snowModuleType | "Simple" | Simple snow module; only option for now |
| freezingT | PCRaster/NetCDF file | Freezing temperature (degree_celsius) |
| degreeDayFactor | PCRaster/NetCDF file | Degree day factor (m.degree_celsius-1.d-1) for snow melt |
| snowWaterHoldingCap | PCRaster/NetCDF file | Free water capacity within the snow pack (m) |
| refreezingCoeff | PCRaster/NetCDF file | Refreezing coefficient (m.m-1.d-1) for free water within the snow pack |
| --- | --- | --- |
| minTopWaterLayer | PCRaster/NetCDF file | Minimum top water layer (m) |
| minCropKC | PCRaster/NetCDF file | Minimum evaporation coefficient |
| cropCoefficientNC | NetCDF file | Evaporation coefficient; should contain a variable with the name "kc" |
| interceptCapNC | NetCDF file | Canopy interception capacity (m); should contain a variable with the name "interceptCapInput" |
| coverFractionNC | NetCDF file | Cover fraction (m2.m-2); should contain a variable with the name "coverFractionInput" |
| --- | --- | --- |
| landCoverMapsNC | NetCDF file or None | Land cover properties; requires minSoilDepthFrac (m), maxSoilDepthFrac (m), rootFraction1 (m.m-1), rootFraction2 (m.m-1), maxRootDepth (m) and fracVegCover (m2.m-2) variables and can include the arnoBeta variable described below; if None, variables need to be specified seperately |
| arnoBeta | PCRaster/NetCDF file or None | Beta parameter for the Arno baseflow scheme; if None, arno beta is calculated based on the minSoilDepthFrac and maxSoilDepthFrac |
| minSoilDepthFrac | PCRaster/NetCDF file or None | Minimum soil depth fraction (m.m-1); exclusive with landCoverMapsNC |
| maxSoilDepthFrac | PCRaster/NetCDF file or None | Maximum soil depth fraction (m.m-1); exclusive with landCoverMapsNC |
| rootFraction1 | PCRaster/NetCDF file or None | Upper soil layer root depth fraction (m.m-1); exclusive with landCoverMapsNC |
| rootFraction2 | PCRaster/NetCDF file or None | Lower soil layer root depth fraction (m.m-1); exclusive with landCoverMapsNC |
| maxRootDepth | PCRaster/NetCDF file or None | Maximum root depth (m); exclusive with landCoverMapsNC |
| fracVegCover | PCRaster/NetCDF file or None | Base vegetation cover fraction (m2.m-2); exclusive with landCoverMapsNC |
| --- | --- | --- |
| interceptStorIni | PCRaster/NetCDF file | Initial canopy interception storage (m) |
| snowCoverSWEIni | PCRaster/NetCDF file | Initial snow pack snow water equivalent (m) |
| snowFreeWaterIni | PCRaster/NetCDF file | Initial snow pack free water (m) |
| topWaterLayerIni | PCRaster/NetCDF file | Initial top water layer storage (m) |
| storUppIni | PCRaster/NetCDF file | Initial upper soil layer water storage (m) |
| storLowIni | PCRaster/NetCDF file | Initial lower soil layer water storage (m) |
| interflowIni | PCRaster/NetCDF file | Initial interflow (m) |

## Groundwater Options
| Name | Type | Description |
| --- | --- | --- |
| debugWaterBalance | True/False | Flag to check and log the groundwater balance |
| --- | --- | --- |
| groundwaterPropertiesNC | NetCDF file or None | Groundwater properties; can include specificYield (m.m-1), kSatAquifer (m.day-1) and recessionCoeff (day-1) variables described below; if None, variables need to be specified seperately |
| specificYield | PCRaster/NetCDF file or None | Aquifer specific yield (m.m-1) ; exclusive with groundwaterPropertiesNC |
| kSatAquifer | PCRaster/NetCDF file or None | Aquifer saturated hydraulic conductivity (m.day-1) ; exclusive with groundwaterPropertiesNC |
| recessionCoeff | PCRaster/NetCDF file or None | Aquifer recession coefficient (day-1) ; exclusive with groundwaterPropertiesNC |
| --- | --- | --- |
| minRecessionCoeff | PCRaster/NetCDF file or None | Minimum recession coefficient (day-1); if None, a value of 1e-4 is assumed |
| limitFossilGroundWaterAbstraction | True/False | Flag to limit fossil groundwater abstraction |
| estimateOfRenewableGroundwaterCapacity | PCRaster/NetCDF file or None | Renewable groundwater capacity (m) to calculate fossil groundwater capacity; if None, no renewable groundwater capacity is assumed |
| estimateOfTotalGroundwaterThickness | PCRaster/NetCDF file | Groundwater thickness (m) to calculate fossil groundwater capacity |
| minimumTotalGroundwaterThickness | PCRaster/NetCDF file | Minimum groundwater thickness (m) |
| maximumTotalGroundwaterThickness | PCRaster/NetCDF file or None | Maximum groundwater thickness (m); if None, there is no maximum |
| --- | --- | --- |
| pumpingCapacityNC | PCRaster/NetCDF file or None | Maximum pumping capacity properties; requires region_ids (unique ID for each region) and regional_pumping_limit (m.day-1) variables; if None, there is no maximum |
| allocationSegmentsForGroundwater | PCRaster/NetCDF file or None | ID where each (low resolution) groundwater reallocation zone cell has a unique ID; if None, allocation segments are identical to allocationSegmentsForGroundSurfaceWater |
| --- | --- | --- |
| storGroundwaterIni | PCRaster/NetCDF file | Initial renewable groundwater storage (m) |
| storGroundwaterFossilIni | PCRaster/NetCDF file | Initial fossil groundwater storage (m) |
| baseflowIni | PCRaster/NetCDF file | Initial baseflow (m) |
| avgNonFossilGroundwaterAllocationLongIni | PCRaster/NetCDF file | Average long-term (365 days) renewable groundwater allocation (m.day-1); used for pumping |
| avgNonFossilGroundwaterAllocationShortIni | PCRaster/NetCDF file | Average short-term (7 days) renewable groundwater allocation (m.day-1); used for pumping |
| avgTotalGroundwaterAbstractionIni | PCRaster/NetCDF file | Average long-term (365 days) total groundwater abstraction (m.day-1); used for pumping |
| avgTotalGroundwaterAllocationLongIni | PCRaster/NetCDF file | Average long-term (365 days) total groundwater allocation (m.day-1); used for pumping |
| avgTotalGroundwaterAllocationShortIni | PCRaster/NetCDF file | Average short-term (7 days) total groundwater allocation (m.day-1); used for pumping |
| relativeGroundwaterHeadIni | PCRaster/NetCDF file | Initial relative groundwater head (m.m-1.day-1); only for MODFLOW coupling |

## Routing Options
| Name | Type | Description |
| --- | --- | --- |
| debugWaterBalance | True/False | Flag to check and log the routing balance |
| --- | --- | --- |
| lddMap | PCRaster/NetCDF file | Flow direction; follows the numpad keys (e.g. 8 is north, 6 is east, 2 is south and 4 is west) |
| cellAreaMap | PCRaster/NetCDF file | Cell area (m2) |
| routingMethod | "accuTravelTime" or "kinematicWave" | Flag to use PCRaster's accuTravelTime routing implementation or the kinematicWave routing implementation |
| --- | --- | --- |
| manningsN | PCRaster/NetCDF file | Channel Mannings roughness coefficient (s.m-1/3) |
| dynamicFloodPlain | True/False | Flag to route using dynamic floodplains |
| floodplainManningsN | PCRaster/NetCDF file | Floodplain Mannings roughness coefficient (s.m-1/3) |
| gradient | PCRaster/NetCDF file | Channel gradient (m.m-1) |
| constantChannelDepth | PCRaster/NetCDF file | Channel depth (m) |
| constantChannelWidth | PCRaster/NetCDF file | Channel width (m) |
| minimumChannelWidth | PCRaster/NetCDF file | Minimum channel width (m) |
| bankfullCapacity | PCRaster/NetCDF file | Channel storage capacity (m3) above which water is allocated to floodplains |
| --- | --- | --- |
| relativeElevationFiles | PCRaster/NetCDF file | Relative elevation parameters; requires relative elevation above channel (m) per area fraction, each in a different file as indicated by the relativeElevationLevels; note that pattern matching can be used with the values under relativeElevationLevels |
| relativeElevationLevels | number list | Area fractions used for the relativeElevationFiles |
| cropCoefficientWaterNC | PCRaster/NetCDF file or None | Evaporation coefficient |
| minCropWaterKC | PCRaster/NetCDF file | Minimum evaporation coefficient |
| --- | --- | --- |
| onlyNaturalWaterBodies | True/False | Flag to run only natural water bodies (no reservoirs) |
| waterBodyInputNC | NetCDF file or None | Water body properties; requires fracWaterInp (m2.m-2), waterBodyIds, waterBodyTyp, resMaxCapInp (m3), resSfAreaInp (m2) variables described below; if None, variables need to be specified seperately |
| fracWaterInp | PCRaster/NetCDF file or None | Water body cover fraction (m2.m-2); exclusive with waterBodyInputNC |
| waterBodyIds | PCRaster/NetCDF file or None | Water body ID; exclusive with waterBodyInputNC |
| waterBodyTyp | PCRaster/NetCDF file or None | Water body type (0 is wetlands, 1 is lakes and 2 is reservoirs); exclusive with waterBodyInputNC |
| resMaxCapInp | PCRaster/NetCDF file or None | Reservoir storage capacity (m3); exclusive with waterBodyInputNC |
| resSfAreaInp | PCRaster/NetCDF file or None | Reservoir surface area (m2); exclusive with waterBodyInputNC |
| --- | --- | --- |
| waterBodyStorageIni | PCRaster/NetCDF file | Initial water body storage (m3.day-1) |
| channelStorageIni | PCRaster/NetCDF file | Initial channel storage (m3.day-1) |
| readAvlChannelStorageIni | PCRaster/NetCDF file | Initial channel storage available for abstractions (m3.day-1) |
| avgDischargeLongIni | PCRaster/NetCDF file | Initial average long-term (365 days) discharge (m3.s); used to determine channel characteristics |
| avgDischargeShortIni | PCRaster/NetCDF file | Initial average short-term (7 days) discharge (m3.s); used to determine channel characteristics |
| m2tDischargeLongIni | PCRaster/NetCDF file | Initial average long-term (365 days) discharge variance (m6.s-2); used to determine channel characteristics |
| avgBaseflowLongIni | PCRaster/NetCDF file | Initial average long-term (365 days) baseflow (m3.s); used for partitioning surface and groundwater abstractions |
| riverbedExchangeIni | PCRaster/NetCDF file | Initial river bed exchange (m.day-1) |
| subDischargeIni | PCRaster/NetCDF file | Initial sub-timestep discharge (m3.s) |
| avgLakeReservoirOutflowLongIni | PCRaster/NetCDF file | Initial average long-term (365 days) water body outflow (m3.s); used to constrain water body outflow |
| avgLakeReservoirInflowShortIni | PCRaster/NetCDF file | Initial average short-term (7 days) water body inflow (m3.s); used to constrain water body outflow |
| timestepsToAvgDischargeIni | PCRaster/NetCDF file | Initial timesteps to average discharge (days) |
