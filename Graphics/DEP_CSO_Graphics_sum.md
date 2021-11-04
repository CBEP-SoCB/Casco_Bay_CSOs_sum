Casco Bay CSO Revised Graphics
================
Curtis C. Bohlen, Casco Bay Estuary Partnership
11/04/2021

-   [Load DEP Data](#load-dep-data)
    -   [Establish Folder References](#establish-folder-references)
-   [Load Weather Data](#load-weather-data)
    -   [Access data](#access-data)
-   [Casco Bay Towns Data](#casco-bay-towns-data)
-   [Merge Two Data Sets](#merge-two-data-sets)
-   [Totals Data](#totals-data)
-   [Merge Two Other Data Sets](#merge-two-other-data-sets)
-   [Graphics](#graphics)
    -   [CSO Volumes Per Inch of Rain](#cso-volumes-per-inch-of-rain)
        -   [Linear Model to Extract
            Slope](#linear-model-to-extract-slope)
    -   [Regional CSO Volumes (by Town)](#regional-cso-volumes-by-town)
        -   [Linear Regression to Extract
            Slope](#linear-regression-to-extract-slope)
        -   [Graphic with Cape Elizabeth in
            Red](#graphic-with-cape-elizabeth-in-red)
    -   [Total Outfalls](#total-outfalls)
        -   [Regression to Extract Slope](#regression-to-extract-slope)
-   [Summary Table 2019](#summary-table-2019)
-   [Percent Reduction, Two Ways](#percent-reduction-two-ways)
    -   [Simple Year Over Year Change](#simple-year-over-year-change)
    -   [Linear Regression Predictions](#linear-regression-predictions)

<img
    src="https://www.cascobayestuary.org/wp-content/uploads/2014/04/logo_sm.jpg"
    style="position:absolute;top:10px;right:50px;" />

\#Load Libraries

``` r
library(tidyverse)
#> Warning: package 'tidyverse' was built under R version 4.0.5
#> -- Attaching packages --------------------------------------- tidyverse 1.3.1 --
#> v ggplot2 3.3.5     v purrr   0.3.4
#> v tibble  3.1.4     v dplyr   1.0.7
#> v tidyr   1.1.3     v stringr 1.4.0
#> v readr   2.0.1     v forcats 0.5.1
#> Warning: package 'ggplot2' was built under R version 4.0.5
#> Warning: package 'tibble' was built under R version 4.0.5
#> Warning: package 'tidyr' was built under R version 4.0.5
#> Warning: package 'readr' was built under R version 4.0.5
#> Warning: package 'dplyr' was built under R version 4.0.5
#> Warning: package 'forcats' was built under R version 4.0.5
#> -- Conflicts ------------------------------------------ tidyverse_conflicts() --
#> x dplyr::filter() masks stats::filter()
#> x dplyr::lag()    masks stats::lag()
library(CBEPgraphics)
load_cbep_fonts()
theme_set(theme_cbep())

#library(corrplot)
```

# Load DEP Data

## Establish Folder References

``` r
sibfldnm <- 'Data'
parent   <- dirname(getwd())
sibling  <- file.path(parent,sibfldnm)
niecefldnm <- 'DEP Data'
niece <- file.path(sibling, niecefldnm)
```

``` r
fn <-'DEP_Annual_Totals.csv'
fpath <- file.path(niece, fn)
the_data <- read_csv(fpath, col_types = 
                       c(Community = col_character(),
                         Year = col_integer(),
                         Volume = col_double(),
                         Events = col_double(),
                         Outfalls = col_double()))
```

# Load Weather Data

## Access data

We extract annual Precipitation Totals (in mm), and Annual Days with
more than one tenth of an inch (2.5mm), and one inch (25.4mm) of rain
from the annual weather summaries from NOAA.

``` r
fn <-'Annual_Weather_PWD.csv'
fpath <- file.path(sibling, fn)
rain_data <- read_csv(fpath, col_types =
                       cols(date = col_datetime(format = ""),
                            datatype = col_character(),
                            value = col_double(),
                            attributes = col_character(),
                            station = col_skip())) %>%
  mutate(Year = as.integer(format(date, format = '%Y'))) %>%
  filter (datatype %in% c('PRCP', 'DP10', 'DP1X')) %>%
  select(Year, datatype, value) %>%
  pivot_wider(names_from = datatype, values_from = value) %>%
  rename(Precip_mm = PRCP, GT0.1 = DP10, GT1.0 = DP1X) %>%
  mutate(Precip_in = Precip_mm / 25.4) %>%
  filter(Year > 1996)
```

# Casco Bay Towns Data

We remove Yarmouth from the data because their contribution ended twenty
years ago, and is so small that it vanishes in the graphic display.

Although the number of events and outfalls were reported in the 2008 CSO
report for years prior to 1996, they show no variation, suggesting these
data are not meaningful, so we strip them out here.

We report results on a millions of gallons basis.

``` r
cb_towns_data_long <- the_data %>%
  filter(Community != 'Yarmouth') %>%
  mutate(Volume = Volume / (10^6)) %>%
  filter(Year > 1996)
```

``` r
cb_towns_data_wide <- cb_towns_data_long %>%
  pivot_wider(Year, names_from = Community,
              values_from = Volume) %>%
  rowwise() %>%
  mutate(Total = sum(c(`Cape Elizabeth`,
                       `Portland & PWD`,
                       `South Portland`,
                       Westbrook), na.rm = TRUE),
         PctPortland = `Portland & PWD`/Total) %>%
  ungroup()
```

# Merge Two Data Sets

We produce an annual data set that combines Portland Jetport weather
data, discharge data for the Casco Bay towns, as well as the Casco Bay
and Statewide totals.

``` r
data_combined <- rain_data %>%
  left_join(cb_towns_data_wide, by = 'Year') %>%
  mutate(CSO_MG_per_inch = Total / Precip_in)
```

# Totals Data

This data set includes statewide and Casco Bay annual totals of the DEP
data. Volumes are available both in total discharge volumes each year,
both in Gallons and in Millions of Gallons.

Again, total number of EVENTS in a year is not meaningful, because of
the risk of double counting, so we omit do not sum events.

``` r
annual_data <- the_data %>%
  group_by(Year) %>%
  summarize(TotVol      = sum(Volume,   na.rm = TRUE),
            TotVolMG    = TotVol / (10^6),
            TotOutfalls = sum(Outfalls, na.rm = TRUE),
            
            CBTotVol      = sum(Volume, na.rm = TRUE),
            CBVolMG    = CBTotVol / (10^6),
            CBTotOutfalls = sum(Outfalls, na.rm = TRUE),
            
            CBPctVol       = round(CBTotVol / TotVol, 4) * 100,
            CBPctOutfalls  = round(CBTotOutfalls / TotOutfalls, 4) * 100,
            .groups = 'drop') %>%
  filter(Year > 1996)
```

# Merge Two Other Data Sets

We produce an annual data set that combines Portland Jetport weather
data, and the Casco Bay totals.

``` r
annual_data_all <- rain_data %>%
  left_join(annual_data, by = 'Year') %>%
  mutate(CSO_MG_per_inch = CBVolMG / Precip_in)
```

``` r
rm(cb_towns_data_wide, rain_data, the_data, annual_data)
```

# Graphics

## CSO Volumes Per Inch of Rain

### Linear Model to Extract Slope

We need the slope here and in later graphics to automatically annotate
the figure with the annual decline in CSO volumes. Unfrtunately, we
cannot as readily automate placement of annotations, so that requires
hand coding for each figure.

``` r
cb_cso_lm <- lm(CSO_MG_per_inch ~ Year, data = annual_data_all)
#summary(cb_cso_lm)
slope = round(coef(cb_cso_lm)[2],1)
theannot <- paste( slope, 'MG per inch per year')
```

``` r
plt <- annual_data_all %>%
  filter (Year > 1995) %>%
  ggplot(aes(x = Year, y = CSO_MG_per_inch)) + 
  geom_area(fill = cbep_colors()[1]) +
  geom_smooth( method = 'lm', se=FALSE,
               color = cbep_colors()[3],
               lwd = 0.5,
               lty = 2) +
  geom_text(aes(x=2011, y=15, label = theannot),
            family = 'Montserrat',
            size = 4,
            hjust = 0) +
  ylab('Regional CSO Volume\n(millions of gallons per inch of rain)') +
  theme(axis.title= element_text(size = 14)) +
  scale_fill_manual(values = cbep_colors2()) +
  xlim(c(1997,2021)) 
plt
#> `geom_smooth()` using formula 'y ~ x'
```

<img src="DEP_CSO_Graphics_sum_files/figure-gfm/volume_per_inch_plot-1.png" style="display: block; margin: auto;" />

``` r
ggsave('figures/CSO_MG_per_inch.pdf', device = cairo_pdf, width = 7, height = 5)
#> `geom_smooth()` using formula 'y ~ x'
```

``` r
plt +
  geom_line(aes(y = Precip_in/2), color = cbep_colors()[5]) +
  #geom_smooth( aes(y=Precip_in/2), method = 'lm', se=FALSE, color = cbep_colors()[3]) +
  scale_y_continuous(labels = scales::number,sec.axis = sec_axis(~ . * 2,
                                         name = "Annual Rainfall (in)"))
#> `geom_smooth()` using formula 'y ~ x'
```

<img src="DEP_CSO_Graphics_sum_files/figure-gfm/add_rainfall-1.png" style="display: block; margin: auto;" />

``` r
#ggsave('figures/CSO_MG_per_inch_w_rain.pdf',
#        device = cairo_pdf, width = 7, height = 5)
```

## Regional CSO Volumes (by Town)

### Linear Regression to Extract Slope

``` r
cb_towns_lm <- lm(Total ~ Year, data = data_combined)
#summary(cb_towns_lm)
slope = round(coef(cb_towns_lm)[2],1)
theannot <- paste( slope, 'MG per year')
```

### Graphic with Cape Elizabeth in Red

``` r
plt <- cb_towns_data_long %>%
  ggplot(aes(x = Year, y = Volume)) + 
  geom_area(aes(fill = Community)) +
  ylab('CSO Volume\n(millions of gallons)') +
  scale_fill_manual(values = c('red4', cbep_colors()[c(5,4,1)]), name = '') +
  theme(legend.position=c(.75,.75))
plt
```

<img src="DEP_CSO_Graphics_sum_files/figure-gfm/town_graphics_2-1.png" style="display: block; margin: auto;" />

``` r
plt +
  geom_line(mapping = aes(y = Precip_in * 25), 
            data = data_combined,
            color = cbep_colors()[3],
            lwd = 0.5) +
  geom_smooth(mapping = aes(y=Total),
              data = data_combined,          
              method = 'lm', se=FALSE,
              color = cbep_colors()[3],
              lwd = 0.5,
              lty = 2) + 
  geom_text(aes(x=2013, y=600, label = theannot),
            family = 'Montserrat',
            fontface = 'plain',
            size = 4,
            hjust = 0) +
  geom_text(aes(x=2017, y=1400, label = 'Rainfall'),
            family = 'Montserrat',
            fontface = 'plain',
            size = 3,
            hjust = 0) +
  scale_y_continuous(sec.axis = sec_axis(~ . / 25,
                                         name = "Annual Rainfall (in)")) +
  theme(legend.text = element_text(size = 8))
#> `geom_smooth()` using formula 'y ~ x'
```

<img src="DEP_CSO_Graphics_sum_files/figure-gfm/Towns_add_annotations_1-1.png" style="display: block; margin: auto;" />

``` r
ggsave('figures/CSO_town_area_REVISED_red.pdf',
       device = cairo_pdf, width = 7, height = 5)
#> `geom_smooth()` using formula 'y ~ x'
```

``` r
plt +
  geom_line(mapping = aes(y = Precip_in * 25), 
            data = data_combined,
            color = cbep_colors()[3],
            lwd = 0.5) +
  geom_smooth(mapping = aes(y=Total),
              data = data_combined,          
              method = 'lm', se=FALSE,
              color = cbep_colors()[3],
              lwd = 0.5,
              lty = 2) + 
  geom_text(aes(x=2013, y=600, label = theannot),
            family = 'Montserrat',
            fontface = 'plain',
            size = 4,
            hjust = 0) +
  geom_text(aes(x=2017, y=1400, label = 'Rainfall'),
            family = 'Montserrat',
            fontface = 'plain',
            size = 3,
            hjust = 0) +
  scale_y_continuous(sec.axis = sec_axis(~ . / 25,
                                         name = "Annual Rainfall (in)")) +
  theme(legend.position = 'bottom',
        legend.text = element_text(size = 8))
#> `geom_smooth()` using formula 'y ~ x'
```

<img src="DEP_CSO_Graphics_sum_files/figure-gfm/Towns_add_annotations_2-1.png" style="display: block; margin: auto;" />

``` r
ggsave('figures/CSO_town_area_REVISED_red_bottom.pdf',
       device = cairo_pdf, width = 7, height = 5)
#> `geom_smooth()` using formula 'y ~ x'
```

## Total Outfalls

### Regression to Extract Slope

``` r
cb_outfalls_lm <- lm(CBTotOutfalls ~ Year,
                     data = annual_data_all,
                     subset = Year>2001)
#summary(cb_outfalls_lm)
slope = coef(cb_outfalls_lm)[2]
theannot <- paste('Eliminating an outfall every', -round(1/slope,1), 'years')
```

``` r
plt <-annual_data_all %>%
  filter(Year > 2001) %>%  # Outfalls data not available earlier
  ggplot(aes(x = Year, y = CBTotOutfalls)) + 
  geom_point() +
  geom_smooth(se = FALSE, method = 'lm') +
  ylab('CSO Outfalls') +
  geom_text(aes(x=2010, y=47, label = theannot),
            family = 'Montserrat',
            fontface = 'plain',
            size = 4,
            hjust = 0) +
  xlim(c(2000, 2020)) +
  ylim(c(0,60))
plt
#> `geom_smooth()` using formula 'y ~ x'
```

<img src="DEP_CSO_Graphics_sum_files/figure-gfm/total_outfalls_line-1.png" style="display: block; margin: auto;" />

``` r
#ggsave('figures/CSO_outfalls_points.pdf',
#       device = cairo_pdf, width = 7, height = 5)
```

# Summary Table 2019

Note that it’s really not appropriate to add up events, as events in
different towns may have occurred on the same days, making them the same
events. We do so here for programming simplicity. The total Events value
should be removed before releasing the table.

``` r
cb_towns_data_long %>% 
  filter (Year == 2019) %>%
  select(Community, Events, Outfalls, Volume) %>%
  filter(! is.na(Volume)) %>%
  bind_rows(summarise_all(., funs(if(is.numeric(.)) sum(.) else "Total"))) %>%
  write_csv('Community_table.csv') %>%
  knitr::kable(digits = c(0,0,0,2))
#> Warning: `funs()` was deprecated in dplyr 0.8.0.
#> Please use a list of either functions or lambdas: 
#> 
#>   # Simple named list: 
#>   list(mean = mean, median = median)
#> 
#>   # Auto named with `tibble::lst()`: 
#>   tibble::lst(mean, median)
#> 
#>   # Using lambdas
#>   list(~ mean(., trim = .2), ~ median(., na.rm = TRUE))
#> This warning is displayed once every 8 hours.
#> Call `lifecycle::last_warnings()` to see where this warning was generated.
```

| Community      | Events | Outfalls | Volume |
|:---------------|-------:|---------:|-------:|
| Cape Elizabeth |      2 |        1 |   0.43 |
| Portland & PWD |     46 |       30 | 184.45 |
| South Portland |      3 |        4 |   8.65 |
| Westbrook      |      4 |        5 |   9.82 |
| Total          |     55 |       40 | 203.35 |

# Percent Reduction, Two Ways

## Simple Year Over Year Change

Comparing first year to last year in the record. This is not the best
way to go, since it does not address year to year variability.

``` r
data_combined %>%
  select(Year, Total) %>%
  summarize(pct_chng= (last(Total) - first(Total)) / first(Total))
#> # A tibble: 1 x 1
#>   pct_chng
#>      <dbl>
#> 1   -0.618
```

## Linear Regression Predictions

``` r
df <- tibble(Year = c(1997, 2019))
res <- predict(cb_towns_lm , newdata = df)
(res[2]-res[1])/res[1]
#>          2 
#> -0.8017867
```

So overall, discharges have declined about 80% since 1997.
