Viz I
================

Import weather data

``` r
data("weather_df")
```

First plot

``` r
#ggplot(data = weather_df, mapping = aes(x = tmin, y = tmax)) + #aes - asthetic, what variables from data you want mapped onto canvas, don't normally see this, see below
  #geom_point()

ggplot(weather_df, aes(x = tmin, y = tmax)) + # more typical code
  geom_point()
```

    ## Warning: Removed 17 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](viz_and_eda_files/figure-gfm/unnamed-chunk-2-1.png)<!-- -->

``` r
weather_df |> 
  ggplot(aes(x = tmin, y = tmax)) +
  geom_point() #same as code in  chunk above, stylistic choice
```

    ## Warning: Removed 17 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](viz_and_eda_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->

``` r
ggp_weather_scatterplot = #when saved in environment, still ggplot object, can add layers
  weather_df |> 
  ggplot(aes(x = tmin, y = tmax)) +
  geom_point() 

ggp_weather_scatterplot 
```

    ## Warning: Removed 17 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](viz_and_eda_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

Check that some rows are missing
