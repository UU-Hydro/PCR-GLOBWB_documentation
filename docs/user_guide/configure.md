# How to configure

PCR-GLOBWB requires a [.ini](https://en.wikipedia.org/wiki/INI_file) configuration file to run. The configuration file details, for example, the simulation period, the location of the inputs and the location where the outputs should be stored. Here we provide a short guide to the configuration file, including some examples. *Note that a detailed description of all model inputs and outputs is given in the [input description section](../input_description) and [output description section](../output_description) respectively*

## Structure

The configuration file is split into several option sections:

* *globalOptions* detail the input and output directories, simulation domain, simulation period and simulation description.
* *meteoOptions* detail the model weather forcing, including downscaling.
* *landSurfaceOptions* detail the soil, topography and water-use inputs.
* *various landCoverOptions* (e.g. forestOptions and grasslandOptions) detail the vegetation inputs for each land cover type. Note that the number of landCoverOptions is up to the user.
* *groundwaterOptions* detail the groundwater inputs.
* *routingOptions* detail the routing inputs.
* *reportingOptions* detail the simulation outputs and the reporting frequency and aggregation for each output.

## Options

To run PCR-GLOBWB the configuration file needs to specify all of the required option details. Several option details just require some text, a date, true/false, a number or a path to a file location for example:

`institution = Department of Physical Geography, Utrecht University`

`startTime = 2000-01-01`

`maxSpinUpsInYears = 0`

`debugWaterBalance = True`

`topographyNC = global_30min/landSurface/topography/topography_parameters_30_arcmin_october_2015.nc`

Note that there are at least two things that must always be adjusted:

* Make sure that you edit or set the *outputDir* (output directory) to the directory that you have access. You do not need to create this directory manually.
* Make sure that the *cloneMap* file is stored locally in your computing machine. The *cloneMap* file defines the spatial resolution and extent of your study area and must be in the pcraster format. Some examples are given on [our GitHub repository](https://github.com/UU-Hydro/PCR-GLOBWB_model/tree/master/clone_landmask_maps)

## Inputs
PCR-GLOBWB input and output files for the runs made in Sutanudjaja et al. (2018)[@sutanudjaja18] are available on [Yoda](https://geo.data.uu.nl/research-pcrglobwb/pcr-globwb_gmd_paper_sutanudjaja_et_al_2018/). For requesting access, please send an e-mail to E.H.Sutanudjaja@uu.nl.

The input files (and some output files) are also available on the [OPeNDAP server](https://opendap.4tu.nl/thredds/catalog/data2/pcrglobwb/catalog.html). The [OPeNDAP protocol](https://www.opendap.org) allow users to access PCR-GLOBWB input files from the remote server and perform PCR-GLOBWB runs without the need to download the input files (with total size ~250 GB for the global extent).

## Examples

Several example configuration file are given on [our GitHub repository](https://github.com/UU-Hydro/PCR-GLOBWB_model/tree/master/config). The example configuration files will use PCR-GLOBWB input files from the OPeNDAP 4TU.ResearchData server:

`inputDir = https://opendap.4tu.nl/thredds/dodsC/data2/pcrglobwb/version_2019_11_beta/pcrglobwb2_input/`

The *inputDir* can be adjusted to any local location if you have the input files stored locally in your computing machine.

```yml
[globalOptions]
inputDir     = https://opendap.4tu.nl/thredds/dodsC/data2/pcrglobwb/version_2019_11_beta/pcrglobwb2_input/
outputDir    = /scratch/sutan101/pcrglobwb2_output/30min_global/

cloneMap    = /data/hydroworld/pcrglobwb2_input_release/version_2019_11_beta/pcrglobwb2_input/global_30min/cloneMaps/clone_global_30min.map
landmask    = None

institution = Department of Physical Geography, Utrecht University
title       = PCR-GLOBWB 2 output, with human factors (non-natural)
description = PCR-GLOBWB run with human factors (non-natural) at 30 arcmin resolution

startTime = 2000-01-01
endTime   = 2010-12-31

maxSpinUpsInYears = 0
minConvForSoilSto = 0.0
minConvForGwatSto = 0.0
minConvForChanSto = 0.0
minConvForTotlSto = 0.0

[meteoOptions]
precipitationNC = global_30min/meteo/forcing/daily_precipitation_cru_era-interim_1979_to_2010.nc
temperatureNC   = global_30min/meteo/forcing/daily_temperature_cru_era-interim_1979_to_2010.nc
referenceETPotMethod = Input
refETPotFileNC  = global_30min/meteo/forcing/daily_referencePotET_cru_era-interim_1979_to_2010.nc

[landSurfaceOptions]
debugWaterBalance = True
numberOfUpperSoilLayers = 2

topographyNC     = global_30min/landSurface/topography/topography_parameters_30_arcmin_october_2015.nc
soilPropertiesNC = global_30min/landSurface/soil/soilProperties.nc

includeIrrigation        = True
historicalIrrigationArea = global_30min/waterUse/irrigation/irrigated_areas/irrigationArea30ArcMin.nc
irrigationEfficiency     = global_30min/waterUse/irrigation/irrigation_efficiency/efficiency.nc

includeDomesticWaterDemand  = True
includeIndustryWaterDemand  = True
includeLivestockWaterDemand = True
domesticWaterDemandFile     = global_30min/waterUse/waterDemand/domestic_water_demand_version_october_2014.nc
industryWaterDemandFile     = global_30min/waterUse/waterDemand/industrial_water_demand_version_october_2014.nc
livestockWaterDemandFile    = global_30min/waterUse/waterDemand/livestock_water_demand_1960-2012.nc

desalinationWater                       = global_30min/waterUse/desalination/desalination_water_use_version_october_2014.nc
allocationSegmentsForGroundSurfaceWater = global_30min/waterUse/abstraction_zones/abstraction_zones_60min_30min.nc

irrigationSurfaceWaterAbstractionFractionData           = global_30min/waterUse/source_partitioning/surface_water_fraction_for_irrigation/AEI_SWFRAC.nc
irrigationSurfaceWaterAbstractionFractionDataQuality    = global_30min/waterUse/source_partitioning/surface_water_fraction_for_irrigation/AEI_QUAL.nc
treshold_to_maximize_irrigation_surface_water           = 0.50
treshold_to_minimize_fossil_groundwater_irrigation      = 0.70
maximumNonIrrigationSurfaceWaterAbstractionFractionData = global_30min/waterUse/source_partitioning/surface_water_fraction_for_non_irrigation/max_city_sw_fraction.nc

[forestOptions]
name = forest
debugWaterBalance = True

snowModuleType      =  Simple
freezingT           =  0.0
degreeDayFactor     =  0.0025
snowWaterHoldingCap =  0.1
refreezingCoeff     =  0.05

minTopWaterLayer  = 0.0
minCropKC         = 0.2
cropCoefficientNC = global_30min/landSurface/landCover/naturalTall/Global_CropCoefficientKc-Forest_30min.nc
interceptCapNC    = global_30min/landSurface/landCover/naturalTall/interceptCapInputForest366days.nc
coverFractionNC   = global_30min/landSurface/landCover/naturalTall/coverFractionInputForest366days.nc
landCoverMapsNC   = global_30min/landSurface/landCover/naturalTall/forestProperties.nc

interceptStorIni = global_30min/initialConditions/non-natural/consistent_run_201903XX/1999/interceptStor_forest_1999-12-31.nc
snowCoverSWEIni  = global_30min/initialConditions/non-natural/consistent_run_201903XX/1999/snowCoverSWE_forest_1999-12-31.nc
snowFreeWaterIni = global_30min/initialConditions/non-natural/consistent_run_201903XX/1999/snowFreeWater_forest_1999-12-31.nc
topWaterLayerIni = global_30min/initialConditions/non-natural/consistent_run_201903XX/1999/topWaterLayer_forest_1999-12-31.nc
storUppIni       = global_30min/initialConditions/non-natural/consistent_run_201903XX/1999/storUpp_forest_1999-12-31.nc
storLowIni       = global_30min/initialConditions/non-natural/consistent_run_201903XX/1999/storLow_forest_1999-12-31.nc
interflowIni     = global_30min/initialConditions/non-natural/consistent_run_201903XX/1999/interflow_forest_1999-12-31.nc

[grasslandOptions]
name = grassland
debugWaterBalance = True

snowModuleType      =  Simple
freezingT           =  0.0
degreeDayFactor     =  0.0025
snowWaterHoldingCap =  0.1
refreezingCoeff     =  0.05

minTopWaterLayer = 0.0
minCropKC        = 0.2
cropCoefficientNC = global_30min/landSurface/landCover/naturalShort/Global_CropCoefficientKc-Grassland_30min.nc
interceptCapNC    = global_30min/landSurface/landCover/naturalShort/interceptCapInputGrassland366days.nc
coverFractionNC   = global_30min/landSurface/landCover/naturalShort/coverFractionInputGrassland366days.nc
landCoverMapsNC  = global_30min/landSurface/landCover/naturalShort/grasslandProperties.nc

interceptStorIni = global_30min/initialConditions/non-natural/consistent_run_201903XX/1999/interceptStor_grassland_1999-12-31.nc
snowCoverSWEIni  = global_30min/initialConditions/non-natural/consistent_run_201903XX/1999/snowCoverSWE_grassland_1999-12-31.nc
snowFreeWaterIni = global_30min/initialConditions/non-natural/consistent_run_201903XX/1999/snowFreeWater_grassland_1999-12-31.nc
topWaterLayerIni = global_30min/initialConditions/non-natural/consistent_run_201903XX/1999/topWaterLayer_grassland_1999-12-31.nc
storUppIni       = global_30min/initialConditions/non-natural/consistent_run_201903XX/1999/storUpp_grassland_1999-12-31.nc
storLowIni       = global_30min/initialConditions/non-natural/consistent_run_201903XX/1999/storLow_grassland_1999-12-31.nc
interflowIni     = global_30min/initialConditions/non-natural/consistent_run_201903XX/1999/interflow_grassland_1999-12-31.nc

[irrPaddyOptions]
name = irrPaddy
debugWaterBalance = True

snowModuleType      =  Simple
freezingT           =  0.0
degreeDayFactor     =  0.0025
snowWaterHoldingCap =  0.1
refreezingCoeff     =  0.05
landCoverMapsNC  = global_30min/landSurface/landCover/irrPaddy/paddyProperties.nc

minTopWaterLayer = 0.05
minCropKC        = 0.2
cropDeplFactor   = 0.2
minInterceptCap  = 0.0002
cropCoefficientNC = global_30min/landSurface/landCover/irrPaddy/Global_CropCoefficientKc-IrrPaddy_30min.nc

interceptStorIni = global_30min/initialConditions/non-natural/consistent_run_201903XX/1999/interceptStor_irrPaddy_1999-12-31.nc
snowCoverSWEIni  = global_30min/initialConditions/non-natural/consistent_run_201903XX/1999/snowCoverSWE_irrPaddy_1999-12-31.nc
snowFreeWaterIni = global_30min/initialConditions/non-natural/consistent_run_201903XX/1999/snowFreeWater_irrPaddy_1999-12-31.nc
topWaterLayerIni = global_30min/initialConditions/non-natural/consistent_run_201903XX/1999/topWaterLayer_irrPaddy_1999-12-31.nc
storUppIni       = global_30min/initialConditions/non-natural/consistent_run_201903XX/1999/storUpp_irrPaddy_1999-12-31.nc
storLowIni       = global_30min/initialConditions/non-natural/consistent_run_201903XX/1999/storLow_irrPaddy_1999-12-31.nc
interflowIni     = global_30min/initialConditions/non-natural/consistent_run_201903XX/1999/interflow_irrPaddy_1999-12-31.nc

[irrNonPaddyOptions]
name = irrNonPaddy
debugWaterBalance = True

snowModuleType      =  Simple
freezingT           =  0.0
degreeDayFactor     =  0.0025
snowWaterHoldingCap =  0.1
refreezingCoeff     =  0.05
landCoverMapsNC  = global_30min/landSurface/landCover/irrNonPaddy/nonPaddyProperties.nc

minTopWaterLayer = 0.0
minCropKC        = 0.2
cropDeplFactor   = 0.5
minInterceptCap  = 0.0002
cropCoefficientNC = global_30min/landSurface/landCover/irrNonPaddy/Global_CropCoefficientKc-IrrNonPaddy_30min.nc

interceptStorIni = global_30min/initialConditions/non-natural/consistent_run_201903XX/1999/interceptStor_irrNonPaddy_1999-12-31.nc
snowCoverSWEIni  = global_30min/initialConditions/non-natural/consistent_run_201903XX/1999/snowCoverSWE_irrNonPaddy_1999-12-31.nc
snowFreeWaterIni = global_30min/initialConditions/non-natural/consistent_run_201903XX/1999/snowFreeWater_irrNonPaddy_1999-12-31.nc
topWaterLayerIni = global_30min/initialConditions/non-natural/consistent_run_201903XX/1999/topWaterLayer_irrNonPaddy_1999-12-31.nc
storUppIni       = global_30min/initialConditions/non-natural/consistent_run_201903XX/1999/storUpp_irrNonPaddy_1999-12-31.nc
storLowIni       = global_30min/initialConditions/non-natural/consistent_run_201903XX/1999/storLow_irrNonPaddy_1999-12-31.nc
interflowIni     = global_30min/initialConditions/non-natural/consistent_run_201903XX/1999/interflow_irrNonPaddy_1999-12-31.nc

[groundwaterOptions]
debugWaterBalance = True
groundwaterPropertiesNC = global_30min/groundwater/properties/groundwaterProperties.nc

minRecessionCoeff = 1.0e-4

limitFossilGroundWaterAbstraction      = True
estimateOfRenewableGroundwaterCapacity = 0.0
estimateOfTotalGroundwaterThickness    = global_30min/groundwater/aquifer_thickness_estimate/thickness_30min.nc
minimumTotalGroundwaterThickness       = 100.
maximumTotalGroundwaterThickness       = None

pumpingCapacityNC = global_30min/waterUse/groundwater_pumping_capacity/regional_abstraction_limit.nc

storGroundwaterIni                        = global_30min/initialConditions/non-natural/consistent_run_201903XX/1999/storGroundwater_1999-12-31.nc
storGroundwaterFossilIni                  = global_30min/initialConditions/non-natural/consistent_run_201903XX/1999/storGroundwaterFossil_1999-12-31.nc

avgNonFossilGroundwaterAllocationLongIni  = global_30min/initialConditions/non-natural/consistent_run_201903XX/1999/avgNonFossilGroundwaterAllocationLong_1999-12-31.nc
avgNonFossilGroundwaterAllocationShortIni = global_30min/initialConditions/non-natural/consistent_run_201903XX/1999/avgNonFossilGroundwaterAllocationShort_1999-12-31.nc
avgTotalGroundwaterAbstractionIni         = global_30min/initialConditions/non-natural/consistent_run_201903XX/1999/avgTotalGroundwaterAbstraction_1999-12-31.nc
avgTotalGroundwaterAllocationLongIni      = global_30min/initialConditions/non-natural/consistent_run_201903XX/1999/avgTotalGroundwaterAllocationLong_1999-12-31.nc
avgTotalGroundwaterAllocationShortIni     = global_30min/initialConditions/non-natural/consistent_run_201903XX/1999/avgTotalGroundwaterAllocationShort_1999-12-31.nc

relativeGroundwaterHeadIni                = global_30min/initialConditions/non-natural/consistent_run_201903XX/1999/relativeGroundwaterHead_1999-12-31.nc
baseflowIni                               = global_30min/initialConditions/non-natural/consistent_run_201903XX/1999/baseflow_1999-12-31.nc

allocationSegmentsForGroundwater = global_30min/waterUse/abstraction_zones/abstraction_zones_30min_30min.nc

[routingOptions]
debugWaterBalance = True

lddMap              = global_30min/routing/ldd_and_cell_area/lddsound_30min.nc
cellAreaMap         = global_30min/routing/ldd_and_cell_area/cellarea30min.nc
routingMethod       = accuTravelTime
manningsN           = 0.04
dynamicFloodPlain   = True
floodplainManningsN = 0.07

gradient             = global_30min/routing/channel_properties/channel_gradient.nc
constantChannelDepth = global_30min/routing/channel_properties/bankfull_depth.nc
constantChannelWidth = global_30min/routing/channel_properties/bankfull_width.nc
minimumChannelWidth  = global_30min/routing/channel_properties/bankfull_width.nc
bankfullCapacity     = None

relativeElevationFiles  = global_30min/routing/channel_properties/dzRel%04d.nc
relativeElevationLevels = 0.0, 0.01, 0.05, 0.10, 0.20, 0.30, 0.40, 0.50, 0.60, 0.70, 0.80, 0.90, 1.00

cropCoefficientWaterNC = global_30min/routing/kc_surface_water/cropCoefficientForOpenWater.nc
minCropWaterKC         = 1.00
waterBodyInputNC       = global_30min/routing/surface_water_bodies/waterBodies30min.nc
onlyNaturalWaterBodies = False

waterBodyStorageIni            = global_30min/initialConditions/non-natural/consistent_run_201903XX/1999/waterBodyStorage_1999-12-31.nc
channelStorageIni              = global_30min/initialConditions/non-natural/consistent_run_201903XX/1999/channelStorage_1999-12-31.nc
readAvlChannelStorageIni       = global_30min/initialConditions/non-natural/consistent_run_201903XX/1999/readAvlChannelStorage_1999-12-31.nc
avgDischargeLongIni            = global_30min/initialConditions/non-natural/consistent_run_201903XX/1999/avgDischargeLong_1999-12-31.nc
avgDischargeShortIni           = global_30min/initialConditions/non-natural/consistent_run_201903XX/1999/avgDischargeShort_1999-12-31.nc
m2tDischargeLongIni            = global_30min/initialConditions/non-natural/consistent_run_201903XX/1999/m2tDischargeLong_1999-12-31.nc
avgBaseflowLongIni             = global_30min/initialConditions/non-natural/consistent_run_201903XX/1999/avgBaseflowLong_1999-12-31.nc
riverbedExchangeIni            = global_30min/initialConditions/non-natural/consistent_run_201903XX/1999/riverbedExchange_1999-12-31.nc
subDischargeIni                = global_30min/initialConditions/non-natural/consistent_run_201903XX/1999/subDischarge_1999-12-31.nc
avgLakeReservoirInflowShortIni = global_30min/initialConditions/non-natural/consistent_run_201903XX/1999/avgLakeReservoirInflowShort_1999-12-31.nc
avgLakeReservoirOutflowLongIni = global_30min/initialConditions/non-natural/consistent_run_201903XX/1999/avgLakeReservoirOutflowLong_1999-12-31.nc
timestepsToAvgDischargeIni     = global_30min/initialConditions/non-natural/consistent_run_201903XX/1999/timestepsToAvgDischarge_1999-12-31.nc

[reportingOptions]
outDailyTotNC = discharge,totalRunoff,gwRecharge,totalGroundwaterAbstraction,surfaceWaterStorage

outMonthTotNC = actualET,irrPaddyWaterWithdrawal,irrNonPaddyWaterWithdrawal,domesticWaterWithdrawal,industryWaterWithdrawal,livestockWaterWithdrawal,runoff,totalRunoff,baseflow,directRunoff,interflowTotal,totalGroundwaterAbstraction,desalinationAbstraction,surfaceWaterAbstraction,nonFossilGroundwaterAbstraction,fossilGroundwaterAbstraction,irrGrossDemand,nonIrrGrossDemand,totalGrossDemand,nonIrrWaterConsumption,nonIrrReturnFlow,precipitation,gwRecharge,surfaceWaterInf,referencePotET,totalEvaporation,totalPotentialEvaporation,totLandSurfaceActuaET,totalLandSurfacePotET,waterBodyActEvaporation,waterBodyPotEvaporation

outMonthAvgNC = discharge,temperature,dynamicFracWat,surfaceWaterStorage,interceptStor,snowFreeWater,snowCoverSWE,topWaterLayer,storUppTotal,storLowTotal,storGroundwater,storGroundwaterFossil,totalActiveStorageThickness,totalWaterStorageThickness,satDegUpp,satDegLow,channelStorage,waterBodyStorage

outMonthEndNC = storGroundwater,storGroundwaterFossil,waterBodyStorage,channelStorage,totalWaterStorageThickness,totalActiveStorageThickness

outAnnuaTotNC = totalEvaporation,precipitation,gwRecharge,totalRunoff,baseflow,desalinationAbstraction,surfaceWaterAbstraction,nonFossilGroundwaterAbstraction,fossilGroundwaterAbstraction,totalGroundwaterAbstraction,totalAbstraction,irrGrossDemand,nonIrrGrossDemand,totalGrossDemand,nonIrrWaterConsumption,nonIrrReturnFlow,runoff,actualET,irrPaddyWaterWithdrawal,irrNonPaddyWaterWithdrawal,irrigationWaterWithdrawal,domesticWaterWithdrawal,industryWaterWithdrawal,livestockWaterWithdrawal,precipitation_at_irrigation,netLqWaterToSoil_at_irrigation,evaporation_from_irrigation,transpiration_from_irrigation,referencePotET

outAnnuaAvgNC = temperature,discharge,surfaceWaterStorage,waterBodyStorage,interceptStor,snowFreeWater,snowCoverSWE,topWaterLayer,storUppTotal,storLowTotal,storGroundwater,storGroundwaterFossil,totalWaterStorageThickness,satDegUpp,satDegLow,channelStorage,waterBodyStorage,fractionWaterBodyEvaporation,fractionTotalEvaporation,fracSurfaceWaterAllocation,fracDesalinatedWaterAllocation,gwRecharge
outAnnuaEndNC = surfaceWaterStorage,interceptStor,snowFreeWater,snowCoverSWE,topWaterLayer,storUppTotal,storLowTotal,storGroundwater,storGroundwaterFossil,totalWaterStorageThickness
outMonthMaxNC = channelStorage,dynamicFracWat,floodVolume,floodDepth,surfaceWaterLevel,discharge,totalRunoff
outAnnuaMaxNC = None
```
