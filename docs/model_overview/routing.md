# PCR-GLOBWB 2.0: Routing 

## Surface water 

Surface water constitutes a separate land cover class for which the process descriptions differ from the land surface proper. Thus, a fraction of the cell area in each cell is reserved for surface water, which can be subdivided into a network of river stretches on which water bodies can be present. River stretches consist of connected cells and are characterized by a channel length, width and depth. Water bodies represent lakes and reservoirs and they can cover multiple, adjacent cells that are fed by upstream rivers and potentially connect to the downstream river network by means of a single outlet. 

At every cell, the surface water is replenished locally by the total runoff from the land area and by the channel precipitation; also, water is lost due to open water evaporation, surface water withdrawals and riverbed infiltration. This defines the local changes in the surface water storage, which corresponds to the channel volume for rivers and the water body storage for lakes and reservoirs. Laterally, the surface water storage changes because of the in- and outflow. For river cells, this results in the simulated river discharge. For water bodies, the local water changes, the inflow from upstream rivers and the discharge at the outlet are totalized in order to describe the total storage over time for the entire water body. 

For river cells, the flow equation used is Manning's equation; for lakes, the flow is based on the discharge equation of a broad-crested weir whereas for reservoirs it is a function of the reservoir operations (see below). 

 

### River discharge 

River discharge can be simulated by different routing options in PCR-GLOBWB. In all instances, the main output consists of the changing channel storage and the corresponding discharge. The choice depends on the required output and consideration of the computation times. Three options can be identified: 

1. Accutraveltime 

The simplest and fastest routing scheme is the *accutraveltime* option. In this simulation, the characteristic travel time of each cell is computed on the basis of the bankfull discharge and combined with the downstream travel distance to identify where the routed channel volume will appear within one day (= one time step). Discharge is then the volumetric rate of all the water passing a cell within the time step and the channel volume is composed of all the water reaching the current cell in the current time step. Water bodies are accommodated by releasing a discharge volume that is dependent on the reservoir operations. If water bodies are present, all flow is intercepted by the water bodies and stored there before the flow continues. The assumption here is that the residence time of water in water bodies that can be represented on the river network of PCR-GLOBWB is much larger than a single day. 

The accutraveltime option in PCR-GLOBWB is mass conservative but uses an approximation of the flow equation. 

 

2. Kinematic wave, constant river width 

Discharge can also calculated from the kinematic wave approximation of the Saint-Venant Equation [Chow et al., 1988]. In this case, conservation of mass is considered but also the flow equation is better approximated, using the channel characteristics as basis. In this case, water actually flows dependent as a function of the water depth and the local slope. This makes this routing method more computationally demanding than the accutraveltime function and in certain cases sub-timestepping is required to accurately simulate the discharge and the resulting water levels. 

In this case, water travels from cell to cell and this gives a more continuous simulation of the discharge and the connection to the water bodies. As water is always contained in the channel, water heights can exceed the channel depth and this may lead to flow velocities and discharge rates that are faster than what could be expected realistically during high water events. 

 

3. Kinematic wave with variable inundation 

The kinematic wave routing scheme can be coupled with an inundation scheme that expands the inundated area onto the floodplain if the water level surpasses the channel depth. The inundated area is computed on the basis of the elevation distribution in the cell and with the larger inundated area, the resistance increases and the propagation of the flood wave slows. This routine gives a better representation of the river regime for rivers that are not or only partially embanked. Also, the inundated area and flood depth provides relevant information for hazard assessments or environmental modelling. This routing method, however, requires more computational time and is relatively slow; this options should only be preferred when the temporal variation of flooded areas is of importance. 

 

### Waterbodies: lakes and reservoirs 

Waterbodies are contiguous units of connected cells with a total water volume. Lakes do not possess an actual depth, the storage only being described in terms of the active water depth above elevation of the outlet cell, the discharge being caclulated using the equation for a broad-crested weir. 

Reservoirs do have a total storage and the outflow is dependent on the storage as outlined by the reservoir scheme. At the moment, PCR-GLOBWB 2.0 uses a simple reservoir scheme in which the outflow approaches the average discharge if the reservoir is filled to a preset capacity. This capacity is less than the total storage capacity, to create a freeboard and dampen the outflow if the inflow approaches critical outflow levels (taken to be equal to the bankfull discharge). On the lower end, the outflow of the reservoir is curtailed if the storage falls below a minimum capacity, representing the dead pool volume of the reservoir. This reservoir scheme is representative of a hydropower reservoir if no varying demand is specified as the aim then would be to maintain the maximum head difference in the reservoir. 
