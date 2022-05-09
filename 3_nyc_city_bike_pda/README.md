
# Data Analysis on NYC City Bikes Performance (2018)

The __Data Analysis on NYC City Bikes Performance 2018__ (the report)is based on the research on the usage of 10 bikes in 2018 by [NYC Citi Bike](https://citibikenyc.com/homepage)  There are a total of 4268 observations, covering customers' gender, age, gender, bike usage type, as well as trips details including start and end times and locations. 

## Data Ethic Guidline

The data used in this report is a public source stored in the Github `tidyverse package`. There is not direct consent authorized by NYC Citi Bikes or NYC Citi Bikes's users. This report is only used for data analysis case study. 

On behalf of the data collected by NYC Citi Bikes, there is not evidence show that it disclosure users' private data: such as names, personal ID, or address etc. 

## Objective

This report explores the overall bike hiring service performance in 2018. The analysis focuses on the sectors of:

- The relations between gender and customer type;
- The relations between travel duration and age;
- Performance by month;
- Performance by weekday;
- An overview of popularity of different stations

## Built with

`R Studio`:
`tsibbledata`
`tsibble`
`tidyverse`
`janitor`
`lubridate`
`leaflet`

## Data Cleaning and Wrangling

The main purpose of this project is practicing the use of `lubridate`, that clean the `POSIXct` format as multiple columns of date format. 

```r
nyc_bikes_df_day_fixed <- nyc_bikes_df %>% 
  mutate(date = day(start_time), .after = 2) %>% 
  mutate(month = month(start_time, label = TRUE, abbr = FALSE), .after = 3) %>% 
  mutate(year = year(start_time), .after = 4)
```

Below we aim to find out the traveled time and age of each users:

```r
nyc_bikes_df_day_fixed <- nyc_bikes_df_day_fixed %>% 
  mutate(travelled_time = stop_time - start_time, .after = 6) %>% 
  mutate( age = year - birth_year, .after = 14)
```

Then, mutate a `weekday` column that helps to figure out the renting service performance on weekdays

```r

nyc_bikes_mass_weekday <- nyc_bikes_mass %>% 
  mutate(weekday = wday(start_time), .after = 5) %>% 
  mutate(weekday = case_when(
    weekday == "1" ~ "Sunday",
    weekday == "2" ~ "Monday",
    weekday == "3" ~ "Tuesday",
    weekday == "4" ~ "Wednesday",
    weekday == "5" ~ "Thursday",
    weekday == "6" ~ "Friday",
    weekday == "7" ~ "Saturday"
  ))

```

## Geospatial 

The report includes a geospatial indicating the deatails of each stations, which the markers show the top 3 most popular stations; and the blue warning signs show the least popular stations.



