# Screenshots

Some screenshots from a Mendix application

Compare all of that with this code:
```
  df = day{"values"} // daily forecast
  weather_type = df{"SnowAccumulationAvg"} > 3  => "Snowing"       |
                 df{"RainAccumulationAbg"} > 3  => "Rainy"         |
                 df{"CloudBaseAvg"} > 50        => "Cloudy"        |
                 df{"CloudBaseAvg"} > 5         => "PartialCloudy" |
                                                   "Sunny"
```
