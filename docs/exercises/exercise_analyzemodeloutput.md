# Analyze PCR-GLOBWB model output
The aim of this excersise is to get familiar with different ways to analyse the PCR-GLOBWB model output. This excersise will provide some guidance on how to do some simple analyses with both CDO and Python, and provide a basis to be expended upon for more complicated analyses.

## Requirements
1. A Python environment with the numpy, NetCDF4, CDO and ncview packages installed. You can use the default PCR-GLOBWB environment (see the [install section](../../user_guide/install)), that already contains PCRaster.

    `conda activate pcrglobwb_python3`

2. An interactive Python editor. We recommend to install Visual Studio Code. Follow the Visual Studio Code install instructions given [here](https://code.visualstudio.com/docs/setup/windows). A user guide and short reference on the Visual Studio Code for Python can be found [here](https://code.visualstudio.com/docs/languages/python).

3. PCR-GLOBWB model output with your variables of interest. Several output variables are available in the default PCR-GLOBWB output (see the [configure section](../../user_guide/configure)).

    `wget https://opendap.4tu.nl/thredds/fileServer/data2/pcrglobwb/version_2019_11_beta/example_output/global_05min_gmd_paper_output/discharge_monthAvg_output_1958-01-31_to_2015-12-31_zip.nc`

    Note that the default outputs are given for the global extent and at a 5 arc-minute resolution. As such, outputs can be relatively large (~ 5GB).

## Calculate yearly and multi-year monthly climatologies with CDO
Visualize the monthly discharge output using the [ncview](http://meteora.ucsd.edu/~pierce/ncview_home_page.html) visualization software. Ncview allows us to visualize (multiple) variables for each timestep in a NetCDF file. You should be able to see a visualization of the monthly discharge as shown below.

`ncview discharge_monthAvg_output_1958-01-31_to_2015-12-31_zip.nc`

![monthly discharge](../img/)

Next we will calculate the yearly average, multi-year monthly average and multi-year average discharge using the CDO [yearmean](http://www.idris.fr/media/ada/cdo.pdf), [ymonmean](http://www.idris.fr/media/ada/cdo.pdf) and [timmean](http://www.idris.fr/media/ada/cdo.pdf) functions. You should be able to see a visualization of these different climatologies as shown below. Note the change in the timesteps.

`cdo yearmean discharge_monthAvg_output_1958-01-31_to_2015-12-31_zip.nc discharge_yearAvg_output_1958-01-31_to_2015-12-31_zip.nc`

`ncview discharge_yearAvg_output_1958-01-31_to_2015-12-31_zip.nc`

![yearly discharge](../img/)

`cdo ymonmean discharge_monthAvg_output_1958-01-31_to_2015-12-31_zip.nc discharge_myMonthAvg_output_1958-01-31_to_2015-12-31_zip.nc`

`ncview discharge_myMonthAvg_output_1958-01-31_to_2015-12-31_zip.nc`

![multi-year monthly discharge](../img/)

`cdo timmean discharge_monthAvg_output_1958-01-31_to_2015-12-31_zip.nc discharge_myAvg_output_1958-01-31_to_2015-12-31_zip.nc`

`ncview discharge_myAvg_output_1958-01-31_to_2015-12-31_zip.nc`

![multi-year discharge](../img/)

Lastly, plot and save the discharge timeseries for the multi-year monthly discharge at one of the pixels. Ncview can do this by clicking on (several) cell, thereby automatically creating a simple plot. The values in the plot can be stored using the "dump" menu. You should be able to see a visualization of these different timeseries as shown below.

![multi-year monthly timeseries plot](../img/)

![multi-year monthly timeseries dump](../img/)

## Importing temporal and spatial values in Python
Open your interactive Python editor.
