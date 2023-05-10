# Make a custom clone and landmask map
The aim of this excersise is to create a custom clone/landmask map for a single catchment. This (smaller) clone/landmask map can be use to significantly reduce the time needed to run PCR-GLOBWB and the storage needed to store the PCR-GLOBWB model outputs, as less cells are actually simulated.

## Requirements
1. A python environment with the PCRaster package installed. You can use the default PCR-GLOBWB environment (see the [install section](../../user_guide/install)), that already contains PCRaster.

    `conda activate pcrglobwb_python3`

2. A (global) PCRaster flow direction (LDD) map which contains your domain of interest. Several global flow direction maps are available in the default PCR-GLOBWB input (see the [configure section](../../user_guide/configure)).

    `wget https://opendap.4tu.nl/thredds/fileServer/data2/pcrglobwb/version_2019_11_beta/pcrglobwb2_input/global_05min/routing/ldd_and_cell_area/lddsound_05min.nc`

    Note that the default flow direction maps are in the netCDF format and will need to be converted to the PCRaster format and subsequently validated. Please check the description for the PCRaster [ldd](https://pcraster.geo.uu.nl/pcraster/4.4.0/documentation/pcraster_manual/sphinx/op_ldd.html) and [lddrepair](https://pcraster.geo.uu.nl/pcraster/4.3.3/documentation/pcraster_manual/sphinx/op_lddrepair.html) functions.

    `gdal_translate -of PCRaster lddsound_05min.nc lddsound_05min.nc.map`

    `pcrcalc lddsound_05min.map = "lddrepair(ldd(lddsound_05min.nc.map))"`

## Select an appropriate outflow point
Visualize the flow direction map using the PCRaster [aguila](https://pcraster.geo.uu.nl/misc/developments/aguila/) visualization software. Note that flow directions are internally defined as numbers that represent the flow direction (see the description of the PCRaster [ldd](https://pcraster.geo.uu.nl/pcraster/4.4.0/documentation/pcraster_manual/sphinx/secdatbase.html#formldd) data type). You should be able to see a visualization of the flow directions as shown below.

`aguila lddsound_05min.map`

![flow directions](../img/)

Next we will calculate the [Strahler number](https://en.wikipedia.org/wiki/Strahler_number) (or stream order) of the river network on top of the flow directions. The Strahler numbers will help us to identify the tributaries of our river network. Note that you can visualize the flow direction and Strahler number at the same time. You should be able to see a visualization of the river network as shown below.

`pcrcalc stream_order.map = "streamorder(lddsound_05min.map)"`

`aguila stream_order.map + lddsound_05min.map`

![flow directions and stream orders](../img/)

By zooming into the river network visualization you can identify the most downstream cell in your study area. In the “View” menu, activate “Show Cursor and Values” and click the most downstream cell. Write the coordinates of the downstream cell on a piece of paper. For our example, the coordinates that are identified are x: 112.881; y: -7.54043.

## Derive the upstream catchment of the outflow point
Make a text file named point.txt, using a text editor (e.g. notepad, geany), consisting of 1 line and 3 columns with the coordinates we selected above.

1 *<your_x_longitude_coordinate\>* *<your_y_latitude_coordinate\>*

![outflow point](../img/)

Next, we rasterize our point text file into a PCRaster point map using the PCRaster [col2map](https://pcraster.geo.uu.nl/pcraster/4.4.0/documentation/pcraster_manual/sphinx/app_col2map.html) function. You should be able to see a visualization of the outflow point as shown below.

`col2map --clone lddsound_05min.map -B -m mv -x 2 -y 3 -v 1 point.txt point.map`

`aguila stream_order.map + point.map + lddsound_05min.map`

![flow directions, stream orders and outflow point](../img/)

Lastly we use the point map to derive the upstream catchment using the PCRaster [catchment](https://pcraster.geo.uu.nl/pcraster/4.4.0/documentation/pcraster_manual/sphinx/op_catchment.html) function. You should be able to see a visualization of the catchment as shown below.

`pcrcalc catchment.map = "catchment(lddsound_05min.map, point.map)"`

`pcrcalc catchment.map = "if(catchment.map eq 1, boolean(1))"`

`aguila catchment.map + lddsound_05min.map`

![flow directions and catchment](../img/)

## Create a clone map based on the upstream catchment
We will try to identify a rectangular box that can bounds the upstream catchment area we derived above. In aquila you will have to identify the coordinates of four corners of your bounding box. In the “View” menu, activate “Show Cursor and Values” and click the bounding corners of the catchment area. Write the coordinates of the corners on a piece of paper. Note that the corner coordinates (from all four corners) must be nicely-rounded (integer) values (without decimals). For our example, the lower left corner coordinate from the bounding box shown in the screenshot below is 111.0, -9.0 (rounded from 111.544, -8.4746).

| name | recorded value | actual value |
| --- | --- | --- |
| upper left corner | 111.0 ; -7.0 | 111.544 ; xx |
| upper right corner | 114.0 ; -7.0 | 111.544 ; xx |
| lower left corner | 111.0 ; -9.0 | xx ; -8.4746 |
| lower right corner | 114.0 ; -9.0 | xx ; -8.4746 |

Then, given the model resolution (5 arc minutes = 5/60 arc degrees = 0.08333333333 degrees), you can calculate the number of rows and columns.

| name | recorded value | calculation |
| --- | --- | --- |
| number of rows | 24 | (-9.0 – -7.0) / (5 / 60) |
| number of columns | 36 | (114.0 – 111.0) / (5 / 60) |

Next we can create a clone map using the PCRaster [mapattr](https://pcraster.geo.uu.nl/pcraster/4.4.0/documentation/pcraster_manual/sphinx/app_mapattr.html) function which will invoke an interactive window/menu. Note that for PCR-GLOBWB clone map, please keep the default values for cell representation (“small integer”), projection (“y increases from bottom to top”), angle (“0.0”). You should be able to see an interactive window as shown below. After you finish with filling in or editing the information about map properties, press “q” (to quit) and then “y” (to confirm to create the map). You should be able to see a visualization of the clone map as shown below.

`mapattr clone_my_area.map`

![mapattr interactive](../img/)

`aguila clone_my_area.map + lddsound_05min.map`

![flow directions and clone map](../img/)

## Create a landmask map based on the clone map
Crop the catchment.map, that still has a global extent, to the extent of your clone map using the PCRaster [resample](http://pcraster.geo.uu.nl/pcraster/4.1.0/doc/manual/app_resample.html) function. This catchment map will be used as our landmask.

`resample --clone clone_my_area.map catchment.map landmask_my_area.map`

These clone and landmask files can now be used in the globalOptions of your PCR-GLOBWB configuration file, as described in the [configure section](../../user_guide/configure).



