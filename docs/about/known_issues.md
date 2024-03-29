# Known Issues

**7 July 2014 (reported by Edwin H. Sutanudjaja)**

If "allocation segments" are used in allocating surface water abstractions and satisfying water demands, the yearly accumulated values of "actSurfaceWaterAbstract" and "allocSurfaceWaterAbstract" (reported in the log report/file) will not be the same. They will have small differences. These are (most likely) due to numerical errors in pcraster implementation in float32 (see: <http://sourceforge.net/p/pcraster/bugs-and-feature-requests/591/>).

**28 July 2014 (documented/reported by Edwin H. Sutanudjaja)**

The corner coordinates (from all four corners) of the clone map must be nicely-rounded (integer) values (without decimals).
    
**17 September 2014 (reported by Edwin H. Sutanudjaja)**

Variable names used in the forcing netcdf files (precipitation, temperature and reference potential evapotranspiration) must be as follows:

- "precipitation" in an input precipitation netcdf file
- "temperature" in an input temperature netcdf file, and
- either "evapotranspiration" and "referencePotET" in an input reference potential evapotranspiration file.

