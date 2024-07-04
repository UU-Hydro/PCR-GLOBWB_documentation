# PCR-GLOBWB 2.0: Water Demand, Withdrawals and Use 

## Sectoral water demand 

PCR-GLOBWB 2.0 includes the water demand for four sectors that has to be met from the available resources. The included sectors are: *domestic, industrial, livestock and irrigation*. 

Water demand is provided as a water slice over the cell area per time step (typically monthly). For domestic, industrial and livestock, these demands are forcings and are prescribed in terms of the gross demand and the net demand. The gross demand is the amount of water that is ideally withdrawn in full to satisfy the needs, the net demand is what is actually consumed, i.e., what is not returning in liquid form to the hydrological system in the model. The difference between the gross and net demand is returned and constitutes the return flows. 

The irrigation water demand is computed in the model as it is the result of the current hydrological situation and the soil and meteorological conditions. So, per day, the irrigation water demand is computed for paddy and non-paddy irrigation. Return flows are not specified a priori but arise due to the amount of irrigation water application that returns to the system by means of deep percolation to the groundwater or via direct runoff, which emerge during the simulations. 

 

## Withdrawals 

Water withdrawal is the actual volume of water taken from the available resources to meet the demands. In PCR-GLOBWB, water withdrawal is a total per resource per time step that together would meet the sum of all demands within the model time step. 

Water withdrawals are taken from the surface water and from the groundwater storage. When water is taken out of storage, the corresponding states (e.g., channel storage, groundwater storage) and fluxes (e.g., river discharge, base flow) are modified. In the case of groundwater storage, a subdivision is made into withdrawals from renewable and non-renewable groundwater resources. Renewable groundwater resources concern groundwater present above the drainage base in the cell that is actively replenished by the groundwater recharge from the soil and that contributes to the base flow. Groundwater withdrawals can continue as long as water is present in the store but in that case the base flow ends and the corresponding groundwater withdrawal is reported as *fossil* as it is not actively replenished. As groundwater withdrawals rely heavily on infrastructure in terms of wells and pumps, the maximum groundwater pumping capacity can be specified that would limit the amount of groundwater that can be withdrawn. In case not all of the required groundwater can be withdrawn, e.g., because of limited pumping capacity or because the groundwater reservoir is exhausted, a water gap will open because the withdrawals are insufficient to meet the demand. 

In addition to surface water and groundwater withdrawals, water can also be supplied by desalinating ocean water. This ‘withdrawal’ from desalinated water is also included and prescribed from existing databases on the volume of desalinated water produced. Because desalination is expensive, the desalinated water withdrawal is used preferentially to meet the domestic water demand and is subtracted from the demand before the other withdrawals are determined. 

Water withdrawals in PCR-GLOBWB reflect the long-term expectations on the available water resources. So, PCR-GLOBWB starts by partitioning the outstanding water demand once the supply of desalinated water is removed by estimating the volumes that have to be withdrawn from the surface water or the groundwater. These estimates can be updated dynamically to reflect the long-term changes in demand and availability using the allocation scheme by De Graaf et al. (2014) or being prescribed using fixed ratios from global datasets (e.g., Siebert et al., 2010; McDonald et al., 2011). Actual water availability within the time step interferes with the projected withdrawals. First, the projected surface water withdrawal is compared with the availability of surface water. Any withdrawal that cannot be met, is passed to the groundwater under the assumption that this provides an unlimited fall-back option if the groundwater pumping capacity is not limiting. The consequence of this approach is that in periods of falling surface water availability the groundwater system is increasingly taxed and that this may evolve towards a situation where water is withdrawn non-sustainably from the fossil groundwater store. 

PCR-GLOBWB 2.0 first considers water demands and availability locally, *i.e.*, per cell. However, water demands and availability may be mismatched locally; for example, rivers may run through a cell that does not directly include the city that constitutes most of the domestic water demand in the vicinity. By pooling demand and availability over larger regions, they can be more realistically matched and more water withdrawn to meet the demand. These larger zones are identified as allocation zones and determine how demand is met by the withdrawals and how the withdrawals are assigned to the respective sectors; all the water withdrawals are summed and proportionally distributed over the sectoral demands and reported as the allocation per sector. The implication of the allocation zones used is that the sectoral water demand is met by water that is withdrawn elsewhere and transported to the cell under consideration. 

 

## Water use and return flows 

The water allocated per sector and assigned to each cell is used there and in part consumed, meaning that the consumed part is removed from the representation of the hydrological system in PCR-GLOBWB; it may become incorporated into goods or produce or it can be evaporated or transporated to the atmosphere. This amount of water consumption is proportional to the ratio of the net demand over the gross demand per sector. The remainder of the withdrawals is returned to the hydrological system as return flows. With the exception of irrigation, all return flows end up within the time step into the surface water. Water allocated to irrigation is actually applied to the model and in part can become part of the hydrological system again, and end up into the groundwater storage as a result of deep percolation or in the surface water as a result of runoff. 

It is important to realize that water withdrawal and use may divert water in space, as water withdrawals can be allocated elsewhere to meet the demand, and that it may shift water from one resource to another, particularly moving groundwater with long residence time to the surface water system with much shorter residence times. While the volumes involved may be relatively small, they may become appreciable if the turnovers are small, such as regions that are water scarce and heavily rely on fossil groundwater or desalinated water or during periods of prolonged drought. 
