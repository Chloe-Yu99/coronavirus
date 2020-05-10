
<!-- README.md is generated from README.Rmd. Please edit that file -->

# coronavirus <a href='https://RamiKrispin.github.io/coronavirus/'><img src='man/figures/coronavirus.png' align="right"  /></a>

<!-- badges: start --->

[![build](https://github.com/RamiKrispin/coronavirus/workflows/build/badge.svg?branch=master)](https://github.com/RamiKrispin/coronavirus/actions?query=workflow%3Abuild)
[![CRAN\_Status\_Badge](https://www.r-pkg.org/badges/version/coronavirus)](https://cran.r-project.org/package=coronavirus)
[![lifecycle](https://img.shields.io/badge/lifecycle-experimental-orange.svg)](https://www.tidyverse.org/lifecycle/#experimental)
[![License:
MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)
[![GitHub
commit](https://img.shields.io/github/last-commit/RamiKrispin/coronavirus)](https://github.com/RamiKrispin/coronavirus/commit/master)
[![Downloads](https://cranlogs.r-pkg.org/badges/grand-total/coronavirus)](https://cran.r-project.org/package=coronavirus)

<!-- badges: end -->

The coronavirus package provides a tidy format dataset of the 2019 Novel
Coronavirus COVID-19 (2019-nCoV) epidemic. The raw data pulled from the
Johns Hopkins University Center for Systems Science and Engineering (JHU
CCSE) Coronavirus
[repository](https://github.com/CSSEGISandData/COVID-19).

More details available
[here](https://RamiKrispin.github.io/coronavirus/), and a `csv` format
of the package dataset available
[here](https://github.com/RamiKrispin/coronavirus-csv)

A summary dashboard is available
[here](https://ramikrispin.github.io/coronavirus_dashboard/)

<img src="man/figures/2019-nCoV-CDC-23312_without_background.png" width="65%" align="center"/></a>

<figcaption>

Source: Centers for Disease Control and Prevention’s Public Health Image
Library

</figcaption>

## Important Note

As this an ongoing situation, frequent changes in the data format may
occur, please visit the package news to get updates about those changes

## Installation

Install the CRAN version:

``` r
install.packages("coronavirus") 
```

Install the Github version (refreshed on a daily bases):

``` r
# install.packages("devtools")
devtools::install_github("RamiKrispin/coronavirus")
```

## Data refresh

While the **coronavirus** [CRAN
version](https://cran.r-project.org/package=coronavirus) is updated
every month or two, the [Github (Dev)
version](https://github.com/RamiKrispin/coronavirus) is updated on a
daily bases. The `update_dataset` function enables to overcome this gap
and keep the installed version with the most recent data available on
the Github version:

``` r
library(coronavirus)
update_dataset()
```

**Note:** must restart the R session to have the updates available

## Usage

``` r
data("coronavirus")
```

This `coronavirus` dataset has the following fields:

  - `date` - The date of the summary
  - `province` - The province or state, when applicable
  - `country` - The country or region name
  - `Lat` - Latitude point
  - `Long` - Longitude point
  - `type` - the type of case (i.e., confirmed, death)
  - `cases` - the number of daily cases (corresponding to the case type)

<!-- end list -->

``` r
head(coronavirus) 
#>         date province     country lat long      type cases
#> 1 2020-01-22          Afghanistan  33   65 confirmed     0
#> 2 2020-01-23          Afghanistan  33   65 confirmed     0
#> 3 2020-01-24          Afghanistan  33   65 confirmed     0
#> 4 2020-01-25          Afghanistan  33   65 confirmed     0
#> 5 2020-01-26          Afghanistan  33   65 confirmed     0
#> 6 2020-01-27          Afghanistan  33   65 confirmed     0
```

Summary of the total confrimed cases by country (top 20):

``` r
library(dplyr)

summary_df <- coronavirus %>% 
  filter(type == "confirmed") %>%
  group_by(country) %>%
  summarise(total_cases = sum(cases)) %>%
  arrange(-total_cases)

summary_df %>% head(20) 
#> # A tibble: 20 x 2
#>    country        total_cases
#>    <chr>                <int>
#>  1 US                 1309550
#>  2 Spain               223578
#>  3 Italy               218268
#>  4 United Kingdom      216525
#>  5 Russia              198676
#>  6 France              176782
#>  7 Germany             171324
#>  8 Brazil              156061
#>  9 Turkey              137115
#> 10 Iran                106220
#> 11 China                83990
#> 12 Canada               68918
#> 13 Peru                 65015
#> 14 India                62808
#> 15 Belgium              52596
#> 16 Netherlands          42581
#> 17 Saudi Arabia         37136
#> 18 Mexico               33460
#> 19 Switzerland          30251
#> 20 Ecuador              29071
```

Summary of new cases during the past 24 hours by country and type (as of
2020-05-09):

``` r
library(tidyr)

coronavirus %>% 
  filter(date == max(date)) %>%
  select(country, type, cases) %>%
  group_by(country, type) %>%
  summarise(total_cases = sum(cases)) %>%
  pivot_wider(names_from = type,
              values_from = total_cases) %>%
  arrange(-confirmed)
#> # A tibble: 187 x 4
#> # Groups:   country [187]
#>    country              confirmed death recovered
#>    <chr>                    <int> <int>     <int>
#>  1 US                       25621  1615     13541
#>  2 Russia                   10817   104      5308
#>  3 Brazil                    9167   639      2388
#>  4 United Kingdom            3896   346         4
#>  5 Peru                      3168   100      1234
#>  6 India                     3113   116      1414
#>  7 Pakistan                  2301    37       279
#>  8 Mexico                    1938   193      1510
#>  9 Saudi Arabia              1704    10      1024
#> 10 Turkey                    1546    50      3084
#> 11 Iran                      1529    48      1227
#> 12 Chile                     1247    10       507
#> 13 Canada                    1244   126      1023
#> 14 Qatar                     1130     1        79
#> 15 Italy                     1083   194      4008
#> 16 Belarus                    951     5       566
#> 17 Singapore                  753     0       256
#> 18 Germany                    736    39      1600
#> 19 Spain                      721   179      2804
#> 20 Sweden                     656    45         0
#> 21 Bangladesh                 636     8       313
#> 22 United Arab Emirates       624    11       458
#> 23 Belgium                    585    60       210
#> 24 France                     580    80       256
#> 25 Indonesia                  533    16       113
#> 26 South Africa               525     8       830
#> 27 Ukraine                    515    15       203
#> 28 Dominican Republic         506     5       298
#> 29 Egypt                      488    11        57
#> 30 Colombia                   444    17       145
#> 31 Kuwait                     415     2       156
#> 32 Bahrain                    330     0        27
#> 33 Romania                    320    16       489
#> 34 Netherlands                289    64         2
#> 35 Poland                     285     9       253
#> 36 Afghanistan                255     6        30
#> 37 Ecuador                    253    13         0
#> 38 Ghana                      251     4        55
#> 39 Nigeria                    239    11        66
#> 40 Ireland                    219    17         0
#> # … with 147 more rows
```

Plotting the total cases by type worldwide:

``` r
library(plotly)

coronavirus %>% 
  group_by(type, date) %>%
  summarise(total_cases = sum(cases)) %>%
  pivot_wider(names_from = type, values_from = total_cases) %>%
  arrange(date) %>%
  mutate(active = confirmed - death - recovered) %>%
  mutate(active_total = cumsum(active),
                recovered_total = cumsum(recovered),
                death_total = cumsum(death)) %>%
  plot_ly(x = ~ date,
                  y = ~ active_total,
                  name = 'Active', 
                  fillcolor = '#1f77b4',
                  type = 'scatter',
                  mode = 'none', 
                  stackgroup = 'one') %>%
  add_trace(y = ~ death_total, 
             name = "Death",
             fillcolor = '#E41317') %>%
  add_trace(y = ~recovered_total, 
            name = 'Recovered', 
            fillcolor = 'forestgreen') %>%
  layout(title = "Distribution of Covid19 Cases Worldwide",
         legend = list(x = 0.1, y = 0.9),
         yaxis = list(title = "Number of Cases"),
         xaxis = list(title = "Source: Johns Hopkins University Center for Systems Science and Engineering"))
```

<img src="man/figures/total_cases.png" width="100%" />

Plot the confirmed cases distribution by counrty with treemap plot:

``` r
conf_df <- coronavirus %>% 
  filter(type == "confirmed") %>%
  group_by(country) %>%
  summarise(total_cases = sum(cases)) %>%
  arrange(-total_cases) %>%
  mutate(parents = "Confirmed") %>%
  ungroup() 
  
  plot_ly(data = conf_df,
          type= "treemap",
          values = ~total_cases,
          labels= ~ country,
          parents=  ~parents,
          domain = list(column=0),
          name = "Confirmed",
          textinfo="label+value+percent parent")
```

<img src="man/figures/treemap_conf.png" width="100%" />

## Data Sources

The raw data pulled and arranged by the Johns Hopkins University Center
for Systems Science and Engineering (JHU CCSE) from the following
resources:

  - World Health Organization (WHO): <https://www.who.int/> <br>
  - DXY.cn. Pneumonia. 2020. <http://3g.dxy.cn/newh5/view/pneumonia>.
    <br>
  - BNO News:
    <https://bnonews.com/index.php/2020/02/the-latest-coronavirus-cases/>
    <br>
  - National Health Commission of the People’s Republic of China (NHC):
    <br> <http://www.nhc.gov.cn/xcs/yqtb/list_gzbd.shtml> <br>
  - China CDC (CCDC):
    http:://weekly.chinacdc.cn/news/TrackingtheEpidemic.htm <br>
  - Hong Kong Department of Health:
    <https://www.chp.gov.hk/en/features/102465.html> <br>
  - Macau Government: <https://www.ssm.gov.mo/portal/> <br>
  - Taiwan CDC:
    <https://sites.google.com/cdc.gov.tw/2019ncov/taiwan?authuser=0>
    <br>
  - US CDC: <https://www.cdc.gov/coronavirus/2019-ncov/index.html> <br>
  - Government of Canada:
    <https://www.canada.ca/en/public-health/services/diseases/coronavirus.html>
    <br>
  - Australia Government Department of Health:
    <https://www.health.gov.au/news/coronavirus-update-at-a-glance> <br>
  - European Centre for Disease Prevention and Control (ECDC):
    <https://www.ecdc.europa.eu/en/geographical-distribution-2019-ncov-cases>
  - Ministry of Health Singapore (MOH):
    <https://www.moh.gov.sg/covid-19>
  - Italy Ministry of Health:
    <http://www.salute.gov.it/nuovocoronavirus>
  - 1Point3Arces: <https://coronavirus.1point3acres.com/en>
  - WorldoMeters: <https://www.worldometers.info/coronavirus/>
  - COVID Tracking Project: <https://covidtracking.com/data>. (US
    Testing and Hospitalization Data. We use the maximum reported value
    from “Currently” and “Cumulative” Hospitalized for our
    hospitalization number reported for each state.)
  - French Government: <https://dashboard.covid19.data.gouv.fr/>
  - COVID Live (Australia): <https://www.covidlive.com.au/>
  - Washington State Department of Health:
    <https://www.doh.wa.gov/emergencies/coronavirus>
  - Maryland Department of Health: <https://coronavirus.maryland.gov/>
  - New York State Department of Health:
    <https://health.data.ny.gov/Health/New-York-State-Statewide-COVID-19-Testing/xdss-u53e/data>
  - NYC Department of Health and Mental Hygiene:
    <https://www1.nyc.gov/site/doh/covid/covid-19-data.page> and
    <https://github.com/nychealth/coronavirus-data>
  - Florida Department of Health Dashboard:
    <https://services1.arcgis.com/CY1LXxl9zlJeBuRZ/arcgis/rest/services/Florida_COVID19_Cases/FeatureServer/0>
    and
    <https://fdoh.maps.arcgis.com/apps/opsdashboard/index.html#/8d0de33f260d444c852a615dc7837c86>
  - Palestine (West Bank and Gaza): <https://corona.ps/details>
  - Israel:
    <https://govextra.gov.il/ministry-of-health/corona/corona-virus/>
  - Colorado: <https://covid19.colorado.gov/covid-19-data>
