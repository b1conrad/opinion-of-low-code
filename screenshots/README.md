# Screenshots

Some screenshots from a Mendix application are in this folder.
Here is the crux of the decision being made, in a Mendix microflow:

![microflow](https://b1conrad.github.io/opinion-of-low-code/screenshots/SubMicroflow.png)

Compare all of that with this code:
```
  df = day{"values"} // daily forecast
  weather_type = df{"SnowAccumulationAvg"} > 3  => "Snowing"       |
                 df{"RainAccumulationAbg"} > 3  => "Rainy"         |
                 df{"CloudBaseAvg"} > 50        => "Cloudy"        |
                 df{"CloudBaseAvg"} > 5         => "PartialCloudy" |
                                                   "Sunny"
```
