#PCR-GLOBWB Land Surface

##Concepts and rationale

PCR-GLOBWB represents the terrestrial land surface by means of a regular grid to describe the interaction with the hydrological cycle. Each cell of this grid captures the average conditions of this landscape but also considers sub-grid variability to include the grain of the landscape at scales finer than the cell size.

In principle, the natural flow of liquid water between cells of the regular grid occurs by means of the drainage network. This is represented by the topology of converging flow directions or *Local Drainage Direction map* linked to an eight-point pour algorithm. This topology represents the river network that consists of river stretches dominated by open channel flow and water bodies such as lakes and man-made reservoirs for which the flow is dependent on a storage-outflow relation (see Routing). For the natural flow of liquid water between cells, a notable exception to this primacy of surface water flow is when PCR-GLOBWB is coupled to MODFLOW to better capture the groundwater dynamics, including the lateral flow of groundwater between cells.
In addition to the natural flow of liquid water between cells, also the human water demands may lead to the withdrawal from water in nearby cells if the local water availability is too low to meet the demand. In that case, the regionally withdrawn water is allocated to the receiving cell and locally used and any water that is non-consumed released as return flow. This human-induced flow of water between cells can alter the local water balance (see Water Demand and Use for details).

The regular grid in PCR-GLOBWB is often projected geographically (i.e., discretized in latitude and longitude) and has a grid resolution in units of decimal degrees (e.g., 0.5 arc degrees or 5 arc minutes). To represent the actual dimensions of the projected planar surface, some inputs have spatial dimensions. The most notable of these is the cell area [m<sup>2</sup>] that is provided for the land surface as the distance between the meridians tapers off towards the poles. It is important too to note that the local drainage direction map also accommodates this distortion in the represented topology of the river network.

Within the cell, the land surface is subdivided into different land cover types representing units with different hydrological behaviour that emerges as a result of land use related processes or the corresponding parameterization. Each of these land cover types is represented by a unit area for which the hydrological response arises from the vertical exchange of water in its different forms from the atmosphere, through the canopy to the soil and groundwater, and vice-versa within a single, daily timestep. The resulting states and fluxes are averaged over the cell according to the relative abundance of the land cover types. The generic processes and their parameterization are described below. For a detailed description of the underlying equations etc. of PCR-GLOBWB see Van Beek and Bierkens (2009) and Sutanudadja et al. (2018) for their implementation and modification in PCR-GLOBWB. In terms of the lateral exchange within the cell, the total runoff from the land surface comprising the direct runoff, subsurface stormflow and base flow is added as a total to the river drainage network in that cell within the same daily timestep.

##Land cover types

A broad distinction can be made between those land cover types that only receive precipitation and those that receive an additional liquid water input to their soil surface, which is currently only the case if the land surface is irrigated. This input, precipitation, possibly increased by irrigation, is the main input to the daily water balance that is solved for each land cover type. The processes that are considered in solving the water balance for each land cover type are the same and are described by instances of the model class `landcover.py`, one instance per land cover type. Differences in the hydrological response between the land cover types primarily arise as a result of differences in their parameterization, as well as in the boundary conditions and initial conditions. The parameterization thus differs between the instances that represent the land cover types in a cell, but also within the instance of a land cover type as the parameterization vary spatially and temporally to capture essential differences influencing the hydrological response, such as the soil properties or the moments of leaf set and leaf fall of its canopy in the case of deciduous forests.

As mentioned before, all land cover types can be present in a cells and they are distinguished on the basis of their suspected influence on the hydrological response. However, since the solution of the daily water balance in PCR-GLOBWB requires that the model iterates over all the land cover types, the number of land cover types should be limited in order to keep computation times manageable. Typically, the following land cover types are used:

*Tall natural vegetation*

This concerns forests, woody vegetation of considerable height. This increases the turbulent exchange of latent heat with the atmosphere. The canopy is often dense, and can be evergreen or deciduous, and hence the transpiration rate is generally large. Because of the canopy, the amount of bare soil evaporation is relatively smaller, also because this land cover type roots deep where possible and can extract moisture for transpiration at greater depth. Forests can be managed and used but this is of minor influence on the diversity of the vegetation  and does not affect their density or height.

*Short natural vegetation*

This land cover type is similar to *tall natural vegetation* but is generally shorter and/or more sparse and roots less deep. As a consequence, the evapotranspiration will tend to be lower and bare soil evaporation tends to become relatively more prominent.

*Pasture and rangeland*

This is an intermediate category between the natural land cover types and cultivated land. It can comprise pastures and meadows that are akin to *short natural vegetation* and rangeland with semi-natural vegetation from herbs to shrubs and trees that are more alike *tall natural vegetation*. This category is particularly included to incorporate the effect of decreasing vegetation height and density compared to the natural vegetation types.

*Rainfed crops*

Rainfed crops represent cultivated land. Typically, this can be characterized as a single crop on a field during predefined growing season, intersected by fallow periods. As such, canopy density will vary strongly over time and periods of increased transpiration occur in between periods of increased direct runoff and bare soil evaporation. Vegetation properties such as height and root depth may vary strongly between crops and have to be accounted for.

*Irrigated non-paddy crops*

As *rainfed crops* but now irrigation water is applied in order to obtain more favourable crop growth and yields. The nature of the irrigation is described in detail in Water Demand and Use. With irrigation, soil moisture will increase and so will the evapotranspiration and deep drainage to the groundwater.

*Irrigated paddy crops*

Irrigated paddy crops differ from *irrigated non-paddy crops* that the main purpose of irrigation in the case of wet rice is directed towards pest control by permanently ponding the surface and not so much to overcome water constraints on the assimilation process during crop growth. As such the amount of irrigation water required and the scheduling are different (see Water Demand and Use for details). In addition to its effect on the water demand, paddy irrigation creates permanently ponded water surfaces that increase the open water evaporation while the infiltration of the ponded water into the soil leads to increased soil moisture contents that persist once irrigation ceases.

*Urban areas*

Urban conditions span from sub-urban areas to metropolitan areas that are very different in appearance but are all characterized by an increased are of impervious, built-up areas that will not accommodate water but rather discharge this quickly as direct runoff. This land cover type takes this in account by including the impervious area in addition to other vegetation types that can be present that may represent parks, undeveloped land etc.

In a cell, each land cover type is represented by a fraction of the land surface in that cell that is complementary to the area of open freshwater (river stretch, water body). The parameterization for a land cover type in a cell is specific and may summarize many different vegetation types present. This summary is obtained by averaging the properties of a specific vegetation type belonging to a land cover type on the basis of the corresponding area. So, multiple types of forests (deciduous, evergreen) or mixed forests (savannah) can be averaged to a representative property. This averaging can consider temporal variations as well, as would arise because of differences in phenology, e.g., deciduous forest, or because of cropping calendars and growth, for example when different crop types are present in a cell or multi-cropping is used. The underlying assumption for this linear proportionality with area is based on the FAO guidelines for crop water requirements[@allen98] and is explained in the next section.

##Atmosphere-canopy interactions

Precipitation can fall as rain or snow (see Meteo) and will be intercepted by the canopy. The amount of intercepted precipitation is dependent on the canopy cover and its interception capacity. Both are related to the leaf area index and the values differ over time as a function of vegetation development. All intercepted precipitation is stored on the canopy where it is liable to evaporation. The throughfall is passed to the soil.

For each land cover type, a temporally variable crop factor is defined[@allen98] that depends on vegetation density, rooting depth, albedo and roughness as well as on the local meteorological conditions (wind speed, relative humity). This crop factor links the reference potential evaporation to a potential rate over the land cover type considered. The crop factor is dimensionless and equal to one if the land cover specific potential evaporation is equal to the reference potential evaporation, above it when it is larger. The assumed linearity in crop specific potential evaporation is the justification to obtain equivalent parameters per crop type by averaging the properties of different vegetation types by area.
The crop-specific potential evaporation is the rate at which open water can evaporate over the surface. This is first subtracted from any water stored on the canopy, then from any liquid water present in the snow cover that can be present. What is left is passed to the soil surface where it is subdivided into bare soil evaporation and transpiration.

##Soil surface – Snow

