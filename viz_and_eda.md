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

``` r
weather_df |> # producing summary information about missingness is very important
  filter(is.na(tmax))
```

    ## # A tibble: 17 × 6
    ##    name         id          date        prcp  tmax  tmin
    ##    <chr>        <chr>       <date>     <dbl> <dbl> <dbl>
    ##  1 Molokai_HI   USW00022534 2022-05-31    NA    NA    NA
    ##  2 Waterhole_WA USS0023B17S 2021-03-09    NA    NA    NA
    ##  3 Waterhole_WA USS0023B17S 2021-12-07    51    NA    NA
    ##  4 Waterhole_WA USS0023B17S 2021-12-31     0    NA    NA
    ##  5 Waterhole_WA USS0023B17S 2022-02-03     0    NA    NA
    ##  6 Waterhole_WA USS0023B17S 2022-08-09    NA    NA    NA
    ##  7 Waterhole_WA USS0023B17S 2022-08-10    NA    NA    NA
    ##  8 Waterhole_WA USS0023B17S 2022-08-11    NA    NA    NA
    ##  9 Waterhole_WA USS0023B17S 2022-08-12    NA    NA    NA
    ## 10 Waterhole_WA USS0023B17S 2022-08-13    NA    NA    NA
    ## 11 Waterhole_WA USS0023B17S 2022-08-14    NA    NA    NA
    ## 12 Waterhole_WA USS0023B17S 2022-08-15    NA    NA    NA
    ## 13 Waterhole_WA USS0023B17S 2022-08-16    NA    NA    NA
    ## 14 Waterhole_WA USS0023B17S 2022-08-17    NA    NA    NA
    ## 15 Waterhole_WA USS0023B17S 2022-08-18    NA    NA    NA
    ## 16 Waterhole_WA USS0023B17S 2022-08-19    NA    NA    NA
    ## 17 Waterhole_WA USS0023B17S 2022-12-31    76    NA    NA

``` r
#nanier:: (package - missingess across all columns)
```

## Fancier scatterplots!

``` r
weather_df |> 
  ggplot(aes(x = tmin, y = tmax, color = name)) + # color = name, assigns color to each name
  geom_point(alpha = 0.3) + # alpha aes, not in aes b/c it is not mapped, 1 = solid, 0 = see through
  geom_smooth(se = FALSE) # adds smooth line, se = FALSE removes error band around lines, not reliable so remove
```

    ## `geom_smooth()` using method = 'loess' and formula = 'y ~ x'

    ## Warning: Removed 17 rows containing non-finite outside the scale range
    ## (`stat_smooth()`).

    ## Warning: Removed 17 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](viz_and_eda_files/figure-gfm/unnamed-chunk-6-1.png)<!-- -->

Where you define aesthetics can matter

``` r
weather_df |> 
  ggplot(aes(x = tmin, y = tmax)) + 
  geom_point(aes(color = name, size = prcp), alpha = 0.3, size = 0.3) + # if you do this, mapping is not inherited, size = 0.3 size of points, size = prcp variable name, will size points based on variable 
  geom_smooth(se = FALSE)
```

    ## `geom_smooth()` using method = 'gam' and formula = 'y ~ s(x, bs = "cs")'

    ## Warning: Removed 17 rows containing non-finite outside the scale range
    ## (`stat_smooth()`).

    ## Warning: Removed 17 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](viz_and_eda_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->

using faceting real quick

``` r
weather_df |> 
  ggplot(aes(x = tmin, y = tmax)) + 
  geom_point(aes(color = name, size = prcp), alpha = 0.3, size = 0.3) + 
  geom_smooth(se = FALSE) +
  facet_grid(. ~ name) # `. ~ name` splits into three columns - cols equal one subgroup/value, `name ~ .` = rows equal one subgroup/value
```

    ## `geom_smooth()` using method = 'loess' and formula = 'y ~ x'

    ## Warning: Removed 17 rows containing non-finite outside the scale range
    ## (`stat_smooth()`).

    ## Warning: Removed 17 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](viz_and_eda_files/figure-gfm/unnamed-chunk-8-1.png)<!-- -->

More interesting scatterplot

``` r
weather_df |> 
  ggplot(aes(x = tmin, y = tmax, color = name, shape = name)) + # shape = name, maps shape to name, gives different share for each category
  geom_point(aes(size = prcp), alpha = 0.3) +
  geom_smooth(se = FALSE) +
  facet_grid(. ~ name)
```

    ## `geom_smooth()` using method = 'loess' and formula = 'y ~ x'

    ## Warning: Removed 17 rows containing non-finite outside the scale range
    ## (`stat_smooth()`).

    ## Warning: Removed 19 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](viz_and_eda_files/figure-gfm/unnamed-chunk-9-1.png)<!-- -->

# Learning Assessment: Write a code chain that starts with weather_df; focuses only on Central Park, converts temperatures to Fahrenheit, makes a scatterplot of min vs. max temperature, and overlays a linear regression line (using options in geom_smooth())

LA plot:

``` r
weather_df |> 
  filter(name == "CentralPark_NY") |> 
  mutate(
    tmax_fahr = tmax * (9/5) + 32, 
    tmin_fahr = tmin * (9/5) + 32
  ) |> 
  ggplot(aes(x = tmin_fahr, y = tmax_fahr)) +
  geom_point() +
  geom_smooth(se = FALSE, method = "lm") # method = "lm", fits linear regression
```

    ## `geom_smooth()` using formula = 'y ~ x'

![](viz_and_eda_files/figure-gfm/unnamed-chunk-10-1.png)<!-- -->

## Small things

``` r
weather_df |> 
  ggplot(aes(x = tmin, y = tmax, color = name, shape = name)) + 
  #geom_point(alpha = 0.3) + 
  geom_smooth(se = FALSE)
```

    ## `geom_smooth()` using method = 'loess' and formula = 'y ~ x'

    ## Warning: Removed 17 rows containing non-finite outside the scale range
    ## (`stat_smooth()`).

![](viz_and_eda_files/figure-gfm/unnamed-chunk-11-1.png)<!-- -->

``` r
weather_df |> 
  ggplot(aes(x = tmin, y = tmax, color = name, shape = name)) + 
  geom_smooth(se = FALSE) +
  geom_point() # added in layers so adding points after smooth makes smooth difficult to see, 
```

    ## `geom_smooth()` using method = 'loess' and formula = 'y ~ x'

    ## Warning: Removed 17 rows containing non-finite outside the scale range
    ## (`stat_smooth()`).

    ## Warning: Removed 17 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](viz_and_eda_files/figure-gfm/unnamed-chunk-11-2.png)<!-- -->

``` r
weather_df |> 
  ggplot(aes(x = tmin, y = tmax)) +
  geom_hex() # separatess x y plane into hexes and shows density of data 
```

    ## Warning: Removed 17 rows containing non-finite outside the scale range
    ## (`stat_binhex()`).

![](viz_and_eda_files/figure-gfm/unnamed-chunk-12-1.png)<!-- -->

``` r
weather_df |> 
  ggplot(aes(x = tmin, y = tmax)) +
  geom_point(color = "purple") # adding color outside of aes to geom point, can also use any color expressable as html hashcode 
```

    ## Warning: Removed 17 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](viz_and_eda_files/figure-gfm/unnamed-chunk-13-1.png)<!-- -->

## Univariate plots

``` r
weather_df |> 
  ggplot(aes(x = tmin)) +
  geom_histogram(color = "white", fill = "red") # color is outside bounds, fill is inside 
```

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

    ## Warning: Removed 17 rows containing non-finite outside the scale range
    ## (`stat_bin()`).

![](viz_and_eda_files/figure-gfm/unnamed-chunk-14-1.png)<!-- -->

``` r
weather_df |> 
  ggplot(aes(x = tmin, color = name)) +
  geom_histogram()
```

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

    ## Warning: Removed 17 rows containing non-finite outside the scale range
    ## (`stat_bin()`).

![](viz_and_eda_files/figure-gfm/unnamed-chunk-15-1.png)<!-- -->

``` r
weather_df |> 
  ggplot(aes(x = tmin, fill = name)) +
  geom_histogram() #gives overlapping histogram
```

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

    ## Warning: Removed 17 rows containing non-finite outside the scale range
    ## (`stat_bin()`).

![](viz_and_eda_files/figure-gfm/unnamed-chunk-15-2.png)<!-- -->

``` r
# add facet grid
```

how would I fix? facet

``` r
weather_df |> 
  ggplot(aes(x = tmin, fill = name)) +
  geom_histogram() +
  facet_grid(name ~ .)
```

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

    ## Warning: Removed 17 rows containing non-finite outside the scale range
    ## (`stat_bin()`).

![](viz_and_eda_files/figure-gfm/unnamed-chunk-16-1.png)<!-- -->

density plot

``` r
weather_df |> 
  ggplot(aes(x = tmin, fill = name)) +
  geom_density(alpha = 0.2)
```

    ## Warning: Removed 17 rows containing non-finite outside the scale range
    ## (`stat_density()`).

![](viz_and_eda_files/figure-gfm/unnamed-chunk-17-1.png)<!-- -->

boxplot

``` r
weather_df |> 
  ggplot(aes(x = name, y = tmin)) +
  geom_boxplot(aes(fill = name))
```

    ## Warning: Removed 17 rows containing non-finite outside the scale range
    ## (`stat_boxplot()`).

![](viz_and_eda_files/figure-gfm/unnamed-chunk-18-1.png)<!-- -->

violin plots

``` r
weather_df |> 
  ggplot(aes(x = name, y = tmin, fill = name)) +
  geom_violin()
```

    ## Warning: Removed 17 rows containing non-finite outside the scale range
    ## (`stat_ydensity()`).

![](viz_and_eda_files/figure-gfm/unnamed-chunk-19-1.png)<!-- -->

ridge plot

``` r
weather_df |> 
  ggplot(aes(x = tmin, y = name)) +
  geom_density_ridges()
```

    ## Picking joint bandwidth of 1.41

    ## Warning: Removed 17 rows containing non-finite outside the scale range
    ## (`stat_density_ridges()`).

![](viz_and_eda_files/figure-gfm/unnamed-chunk-20-1.png)<!-- -->

LA univariate plots: Make plots that compare precipitation across
locations. Try a histogram, a density plot, a boxplot, a violin plot,
and a ridgeplot; use aesthetic mappings to make your figure readable

``` r
# density plot
weather_df |> 
  ggplot(aes(x = prcp, fill = name)) +
  geom_density(alpha = 0.2)
```

    ## Warning: Removed 15 rows containing non-finite outside the scale range
    ## (`stat_density()`).

![](viz_and_eda_files/figure-gfm/unnamed-chunk-21-1.png)<!-- -->

``` r
# histogram
weather_df |> 
  ggplot(aes(x = prcp, fill = name)) +
  geom_histogram() +
  facet_grid(name ~ .)
```

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

    ## Warning: Removed 15 rows containing non-finite outside the scale range
    ## (`stat_bin()`).

![](viz_and_eda_files/figure-gfm/unnamed-chunk-21-2.png)<!-- -->

``` r
weather_df |> 
  filter(prcp > 5, prcp < 1000) |> # zooms in on data by filtering out observations, want to be sure to explain this is zoomed in and does not represent the entrie dataset 
  ggplot(aes(x = prcp, fill = name)) +
  geom_density(alpha = 0.2) 
```

![](viz_and_eda_files/figure-gfm/unnamed-chunk-21-3.png)<!-- -->

## Saving and embedding

saving plots

``` r
ggp_weather_violin = 
  weather_df |> 
  ggplot(aes(x = name, y = tmin, fill = name)) +
  geom_violin()

ggp_weather_violin
```

    ## Warning: Removed 17 rows containing non-finite outside the scale range
    ## (`stat_ydensity()`).

![](viz_and_eda_files/figure-gfm/unnamed-chunk-22-1.png)<!-- -->

``` r
ggsave("plots/violin_plot.pdf", ggp_weather_violin, # if plot is not saved in environment, will just pull last things you sent to viewer
       width = 8, height = 6) # width and heigh in inches
```

    ## Warning: Removed 17 rows containing non-finite outside the scale range
    ## (`stat_ydensity()`).

embedding plots

``` r
ggp_weather_violin
```

    ## Warning: Removed 17 rows containing non-finite outside the scale range
    ## (`stat_ydensity()`).

![](viz_and_eda_files/figure-gfm/unnamed-chunk-23-1.png)<!-- -->
