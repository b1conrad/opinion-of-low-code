# Screenshots

Some screenshots from a Mendix application are in this folder.

## The application -- weather

We use an API (provided by tomorrow.io) to get the weather for the next five days. The whole business of mapping bits and pieces of the JSON it provides is done in Mendix with a point-and-shoot mechanism that is out of our scope here.

### Today's weather -- in a word

There is a microflow that determines an icon to represent the general weather of a day (not great for places like Alberta, Canada, where any given day can experience all four seasons):

![microflow](https://b1conrad.github.io/opinion-of-low-code/screenshots/Microflow.png)

Here is the crux of the decision being made, in the Mendix microflow named `SUB_GenerateWeatherType`:

![submicroflow](https://b1conrad.github.io/opinion-of-low-code/screenshots/SubMicroflow.png)

It is pretty clear what is going on: given a daily forecast, we are to classify the day into a single word, "sunny" if nothing else.

The first decision point seems wrong somehow! Is there snow? If not, it is snowing. Wait, what?

![snowdecision](https://b1conrad.github.io/opinion-of-low-code/screenshots/SnowDecision.png)

Examining the actual logic (as opposed to the label that fits in the losange), we see that, oh, the question really is "Can we rule out snow?" abbreviated to the last word of the question!

Then everything flows along, until "Partial?" which on inspection would mean "Can we rule out partial cloudiness?" If we cannot (the `false` branch) then it is "Rainy". No, that can't be right! This appears to be ~~a typo~~ actually a missed selection from a popup list of the enumeration (because in alphabetic order, PartialCloudy and Rainy are next-door neighbors (as they are in real life, to be fair)).

Here is the enumeration:

![enumeration](https://b1conrad.github.io/opinion-of-low-code/screenshots/Enumeration.png)

For completeness here is the expression for the decision about partially cloudy:

![partialdecision](https://b1conrad.github.io/opinion-of-low-code/screenshots/PartialDecision.png)

## Analysis of the low code approach (as exemplified by Mendix)

As the sub-microflow shows, it is easy to get a high-level overview.

However, the actual logic is hidden. Double-clicking on a decision losange will pop up a modal window showing the actual expression being tested.
Notice that the expression is code -- hence Mendix is a "low code" rather than "no code" system.

Sadly this means that you can only look at one decision at a time. This puts a great deal of stress on the programmer's short term memory!

Another down side is that the logic is spread over two microflows, an enumeration definition, and pop-up windows for each item in each microflow.
There is no way to see it all at once.

## High code comparison

Compare all of that with this function:
```
    weather_type = function(day){
      df = day{"values"} // daily forecast
      df{"SnowAccumulationAvg"} > 3  => "Snowing"       |
      df{"RainAccumulationAbg"} > 3  => "Rainy"         |
      df{"CloudBaseAvg"} > 50        => "Cloudy"        |
      df{"CloudBaseAvg"} > 5         => "PartialCloudy" |
                                        "Sunny"
    }
```

You can still see at a glance what is going on! Furthermore, the details are also fully available.

I have a strong preference for code. Even a seasoned Mendix veteran has been overheard to say, "To be honest, sometimes I would just like to see the code!"

