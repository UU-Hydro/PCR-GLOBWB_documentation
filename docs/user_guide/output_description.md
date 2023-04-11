# PCR-GLOBWB output description

PCR-GLOBWB provides a large number of output variables that capture the terrestrial water cycle and water resources. Here we will describe the most important PCR-GLOBWB output variables. For the complete and latest list of the PCR-GLOBWB output variables, please study the models' output variable definition file on [our GitHub repository](https://github.com/UU-Hydro/PCR-GLOBWB_model/blob/master/model/variable_list.py)

Note that the output frequency (e.g. yearly, daily or monthly) and aggregation (e.g. sum, average, minimum or maximum) are specified in the configuration file under the reportingOptions (see also the [configure section](../configure)). For example, all output variables under *outMonthTotNC* will be provided as monthly totals in a NetCDF file.

## Meteorology
| Variable | Units | Description |
| --- | --- | --- |
| precipitation | m | Rain plus snow fall (downscaled) |
| temperature | degrees celsius | Air temperature (downscaled) |
| referencePotET | m | Reference potential evapotranspiration (downscaled) |

## Storage
| Variable | Units | Description |
| --- | --- | --- |
| interceptStor | m | Canopy intercepted water storage |
| snowCoverSWE | m | Snow pack snow water equivalent storage |
| snowFreeWater | m | Snow pack free water storage |
| topWaterLayer | m | Top (ponding) water storage |
| storUppTotal | m | Upper (first) soil layer water storage |
| storLowTotal | m | Lower (second) soil layer water storage |
| storGroundwater | m | Non-fossil groundwater storage |
| storGroundwaterFossil | m | Fossil groundwater storage |
| surfaceWaterStorage | m | Surface (rivers, lakes and reservoirs) water storage |
| totalActiveStorageThickness | m | Total water storage (all of the above except for fossile groundwater storage) |
| totalWaterStorageThickness | m | Total water storage (all of the above) |
| satDegUpp | m_water.m_capacity-1 | Upper (first) soil layer saturation degree |
| satDegLow | m_water.m_capacity-1 | Lower (second) soil layer saturation degree |
| satDegTotal | m_water.m_capacity-1 | Soil saturation degree (upper and lower layers) |

## Evapotranspiration
| Variable | Units | Description |
| --- | --- | --- |
| interceptEvap | m | Canopy intercepted water evaporation |
| actSnowFreeWaterEvap | m | Snow pack free water evaporation |
| topWaterLayerEvap | m | Top (ponding) water evaporation |
| actBareSoilEvap | m | Bare upper (first) soil layer water evaporation |
| actTranspiUppTotal | m | Upper (first) soil layer water transpiration |
| actTranspiLowTotal | m | Lower (second) soil layer water transpiration |
| actTranspiTotal | m | Soil water transpiration (upper and lower layers) |
| waterBodyActEvaporation | m | Water body (rivers, lakes and reservoirs) water evaporation |
| actualET | m | Land-surface evapotranspiration (all of the above except for water body evaporation) |
| totalEvaporation | m | Total evapotranspiration (all of the above) |

## Runoff
| Variable | Units | Description |
| --- | --- | --- |
| directRunoff | m | Direct surface runoff |
| interflowTotal | m | Interflow runoff from the soil to the surface |
| baseflow | m | Baseflow runoff from the groundwater to the surface |
| local_water_body_flux | m | Runoff from local water bodies |
| runoff | m | Land-surface runoff (all of the above except for runoff from local water bodies) |
| totalRunoff | m | Total runoff (all of the above) |
| discharge | m3.s-1 | Discharge (routed upstream accumulated runoff) |

## Water use
| Variable | Units | Description |
| --- | --- | --- |
| irrGrossDemand | m | Irrigation water demand |
| nonIrrGrossDemand | m | Non-irrigation (domestic, industry and livestock) water demand |
| totalGrossDemand | m | Total water demand (irrigation and non-irrigation) |
| surfaceWaterAbstraction | m | Surface water (rivers, lakes and reservoirs) water abstraction |
| nonFossilGroundwaterAbstraction | m | Non-fossil groundwater abstraction |
| fossilGroundwaterAbstraction | m | Fossil groundwater abstraction |
| totalGroundwaterAbstraction | m | Groundwater abstraction (non-fossil and fossil) |
| desalinationAbstraction | m | Desalination water abstraction |
| totalAbstraction | m | Total abstraction (all of the above) |
| irrPaddyWaterWithdrawal | m | Paddy irrigation water withdrawal |
| irrNonPaddyWaterWithdrawal | m | Non-paddy irrigation water withdrawal |
| irrigationWaterWithdrawal | m | Irrigation water withdrawal (paddy and non-paddy) |
| domesticWaterWithdrawal | m | Domestic water withdrawal |
| industryWaterWithdrawal | m | Industry water withdrawal |
| livestockWaterWithdrawal | m | Livestock water withdrawal |
| nonIrrReturnFlow | m | Return flow from non-irrigation withdrawals |
