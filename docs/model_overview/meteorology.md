# Diagram

## Meteorology

``` mermaid
graph TD

  subgraph Input
    prec["precipitation input (+constant and factor)"]
    temp["temperature input (+constant and factor)"]
    pet("potential evapotranspiration input (+constant and factor)")
  end
  subgraph Meteo Module
    meteo_prec[precipitation]
    meteo_temp[temperature]
    meteo_pet[potential evapotranspiration]
    meteo_down_prec[precipitation]
    meteo_down_temp[temperature]
    meteo_down_pet[potential evapotranspiration]
    meteo_smooth_prec[precipitation]
    meteo_smooth_temp[temperature]
    meteo_smooth_pet[potential evapotranspiration]
  end

  prec -- "read_forcing()" -->meteo_prec[precipitation]
  temp -- "read_forcing()" --> meteo_temp
  temp -. "HamonPotET()" .-> meteo_pet
  pet -. "read_forcing()" .-> meteo_pet
  meteo_prec -- "downscalePrecipitation()" --> meteo_down_prec
  meteo_temp -- "downscaleTemperature()" --> meteo_down_temp
  meteo_pet -- "downscaleReferenceETPot()" --> meteo_down_pet
  meteo_down_prec -- "windowaverage()" --> meteo_smooth_prec
  meteo_down_temp -- "windowaverage()" --> meteo_smooth_temp
  meteo_down_pet -- "windowaverage()" --> meteo_smooth_pet
```

## Other
