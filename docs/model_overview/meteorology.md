# PCR-GLOBWB Meteorology

## Rationale

PCR-GLOBWB requires meteorological input on the amount of precipitation of atmospheric water at the earth's surface,  the nature of this precipitation as either rain or snow, and the amount of water lost as vapour to the atmosphere as a result of evapotranspiration. This input has to be provided as regularly gridded spatio-temporal fields with the same temporal resolution of the main timestep of PCR-GLOBWB, i.e., daily. The spatial resolution of the input grids can vary dependent on the source, but typically meteorological data are taken from meteorological reconstructions such as the CRU time series (REF) or GPCP interpolated rain gauge data (REF) or from the output of General Circulation Models or Regional Climate Models (GCMs, RCMs) with resolutions equal or coarser than the spatial resolution of PCR-GLOBWB (half arc degree or 5 arc minutes). In this case, the meteorological data are disaggregated to the spatial resolution of PCR-GLOBWB, usually using a nearest-neighbour approach.

## Variables

### Precipitation rate

The precipitation rate in PCR-GLOBWB is prescribed in terms of water slice, the depth of rainfall falling over a unit area in one timestep. The native unit used is [m3/m2/day]. This amount falls uniformly over the cell area where it interacts with the canopy and land surface conditions of the different land covers present (see the land surface description). Internally, the precipitation is subdivided into rain and snow (liquid and solid precipitation respectively). This subdivision is based on the air temperature.

### Air temperature

The air temperature in PCR-GLOBWB is prescribed as that measured in or representative for the air mass directly over the earth surface (e.g., at 2 m height). The native unit used is [degC] and the value applies uniformly for all land covers in the cell. The air temperature is used to split the precipitation amount into snowfall and rainfall. The respective fractions are dependent on the melt temperature of snow and a transition zone in which precipitation can be either rain or snow. However, in the default setup of PCR-GLOBWB, this transition zone is absent and all precipitation falls as either snow or rain, depending on whether the air temperature is less than the melt temperature of snow or greater than or equal to the melt temperature.

### Reference potential evaporation

The reference potential evaporation represents the potential rate at which a standard, uniform grass surface, disease-free and not limited by the availability of water or nutrients, can exchange water vapour by means of evaporation and transpiration to the atmosphere.

PCR-GLOBWB has two modes to obtain the reference potential evaporation that is turned into a specific potential evaporation for the land cover types within the cell. The more parsimonious option is to use the option to compute the reference potential evaporation with the Hamon formula on the basis of the specified air temperature and the day length, which depends on the latitude of the cell and the julian day number within the year. In this case, no specific input has to be specified.
The alternative is to prescribe the reference potential evaporation as input to PCR-GLOBWB. This option requires additional input but has the advantage that the reference potential evaporation can be a closer representation of the turbulent exchange of latent heat with the atmosphere in the form of atmosphere due to the incoming and outgoing short- and longwave radiation, wind, relative humidity etc. Standard practice is to use the Penman-Montheith formula in agreement with FAO practices to compute crop water requirements [@allen98 @beek08].

Reference potential evaporation is also a rate and its native unit  is  [m<sup>2 </sup>/m<sup>3 </sup>/day].

## Data requirements

Meteorological input should be provided in the shape of regular grids with a daily resolution stored in a netCDF in which each variable has as dimensions time, latitude, longitude. The timesteps of the netCDF should cover all the days in the simulation period of PCR-GLOBWB and the spatial resolution should be equal or coarser than the spatial resolution of the model’s map attributes, in which case the input data are disaggregated to the model resolution if needed. To ensure full spatial coverage, the meteorological data should cover at least the extent of the area of interest or clone.

Precipitation rate and air temperature should always be provided, the reference potential evaporation can be provided or the option set to use Hamon instead.
Typical input datasets over historical periods are based on combinations of climatic reconstructions and reanalysis products such as the combination of CRU and ERA-40 / ERA-Interim used initially in PCR-GLOBWB (Van Beek and Bierkens, 2008) and the WATCH and W5E5 datasets (REFS).

## Further options

The requirement to specify the meteorological input with a daily resolution as regular grids make the data volume of this input often large. Moreover, often the output from GCMs has different variable names and units than the ones expected by PCR-GLOBWB. To minimise data handling, PCR-GLOBWB can automatically subsample and disaggregate the meteorological input provided and conversion options are included to translate variable names and scale the input to meet the PCR-GLOBWB requirements.

Meteorological variables often vary over much shorter distances than the spatial resolution of the available meteorological datasets due to altitudinal gradients. This small-scale variance is lost by disaggregation with nearest-neighbour downscaling. To keep some of this small-scale variance that particularly may be important in mountainous terrain where more precipitation may fall due to orographic effects and this precipitation may accumulate as snow due to the colder circumstances with height, PCR-GLOBWB offers the option to specify a uniform lapse rate for the precipitation and the air temperature at the original resolution of the meteorological dataset. This lapse rate describes an additive change in air temperature per descend in unit elevation, a relative change for precipitation. These lapse rates are then applied on the basis of the elevation difference between the Digital Elevation Model at the model resolution of PCR-GLOBWB relative to the uniform, average elevation that is representative for the meteorological input dataset. This results in a spatially more varied and detailed meteorological input although the edge effects along the boundaries of the original data resolution cannot be avoided.
