# Crime-and-Weather-ment


For an easy way to simply view this notebook, use the following link:
https://nbviewer.jupyter.org/github/dfreeman500/Crime-and-Weather-ment/blob/main/crime-weather.ipynb

#
## Instructions:

* Clone the repo
* pip install -r requirements.txt
* Run crime-weather.ipynb 
* (Program written in Python 3.7.2)

#
#


## 1. Research Background and Question:
* Louisville, KY publishes crime reports which generally include DATE_REPORTED, DATE_OCCURED, and CRIME_TYPE.
* NOAA publishes historical weather data for Louisville, KY which generally include daily Maximum temperature (TMAX), Minimum temperature (TMIN), Precipitation in inches (PRCP), and Snow in inches (SNOW).

## Question: Will certain weather conditions be statistically related to crime levels in Louisville, Kentucky?

#
## 2. Importing the Data

### Crime Data:
* Multiple .csv files were retrieved from  https://data.louisvilleky.gov/dataset/crime-reports which includes crimes reported within dates 2003 up until portions of 2021.
* 'Crime_Data_2003.csv' only has ~19,000 crimes reported and only begins to consistently report crimes each day in April. Years 2004-2020 range from ~71,000 to ~91,000 crime reports/year and these were concatenated into concat_crime_df. Given that 2003 appeared incomplete, it was left out of the anaylsis
* 'Crime_Data_2019.csv' has reports from 2019-2021. Crimes occuring in 2021 are left out of the anaylsis.


### Weather Data:
* The weather data .csv was retrieved from https://www.ncdc.noaa.gov/cdo-web/datasets/GHCND/stations/GHCND:USC00154958/detail and includes a daily statement for each day starting in 1997 up until portions of 2021.
* Each date generally includes daily Maximum temperature (TMAX), Minimum temperature (TMIN), Precipitation in inches (PRCP), and Snow in inches (SNOW)

#
## 3. Cleaning the Data
* Cleaning concat_crime_df involves calculating several new columns, removing entries without certain timestamps, including only crimes occuring between 2004-2020, and removing crimes that were reported prior to the stated occurence (ex: incident number: 80-10-047137 has DATE_REPORTED: 2006-06-23, DATE_OCCURED: 2010-06-23 - a 1,461 day difference).

* concat_crime_df_seven_day is created which only includes crimes reported in 7 or fewer days from occurence.

* weather_crime_count_df is created to combine the weather for each particular day with a count of crimes types on that particular day. Multiple columns are added and multi-day precipitation is split between days.

* weather_crime_count_df_seven_day is created to combine the weather for each particular day with a count of crimes types that are reported 7 or fewer days from occurence.

#

## 4. Data Visualization
A big concern is the distribution of crimes accross days and months. Because someone can report a crime that happened up to years in the past, there is a tendency for those crimes to be reported for example on the first of the year or the first of the month. 

* The 3 boxplots below show the relative equal distribution of average crimes for days of the week but unequal loading of crimes on the first day of the month and first day of the year. 

* After the boxplots, there are 4 3d bar graphs that show the percent distribution of crimes across days of the month (or year) depending on the time between report and occurence. With few or 0 days between report and crime occurence, the crimes are relatively equally distrubuted. The more days between report and occurence, the more likely the crime is said to have occured on the 1st.  The 2nd and 4th 3d bar graph utilizes >= to make calculations which smoothes out the graph and allows crimes with greater than 29 days between report and occurence to be represented. The 2nd graph can be read as follows: If there are 29 or MORE days between the report and occurence, then ~25% of crimes will be reported as occuring on the 1st (expected occurence would be ~1/31 (3.2%) )

    * Given this unequal distribution, a 7 day cutoff is used for analysis (ex: weather_crime_count_df_seven_day, concat_crime_df_seven_day). Seven days is somewhat arbitrary but it allows to keep the majority of crimes, but yet increase the confidence that when a crime is said to have occured on day X, it indeed occured on that day and weather can be matched to that day with confidence.

* A Distrubution of crime types is provided in a horizontal bar graph

* A scatter plot matrix is shown between TMIN, TMAX, PRCP, SNOW, and NUMBER_OF_CRIMES

* Pearson Correlation Coefficients are checked between NUMBER_OF_CRIMES, each of the crime types, and (TMIN, TMAX, PRCP, and SNOW). Coefficients with abs(.20) or more are printed in red and a scatter plot is also displayed. If the test showed a p value of <.05 then the value is printed in blue. 

#

## 5. Model

* Linear regression is performed on NUMBER_OF_CRIMES and TMIN, TMAX, PRCP, and SNOW. Linear regression allows for continuous prediction of 2 related continuous values. For example in the case of TMIN, if a day has a minimum temperature of 80 degrees, the number of crimes in a day might be forecasted to be roughly around 225 (y = 0.73059131 * 80 + 166.700) given the current data set.



#

## 6. Conclusion
### Will certain weather conditions be statistically related to crime levels in Louisville, Kentucky?
    Yes, but weakly related. TMIN and TMAX were highly correlated (.92). Below, the correlations with greater than .2 (or less than -.2) are shown and TMIN and TMAX often occur in pairs. SNOW and PRCP were not correlated at or above the weak threshold. 
    
    The following crime categories (or total number of crimes) were at least weakly correlated to TMIN or TMAX: 

* NUMBER_OF_CRIMES and TMIN : Correlation coeffient:  0.3635  . p value:  2.861187670807485e-193 
* NUMBER_OF_CRIMES and TMAX : Correlation coeffient:  0.3836  . p value:  7.94787066240026e-217 

* THEFT/LARCENY and TMIN : Correlation coeffient:  0.2699  . p value:  3.9749555602900573e-104 
* THEFT/LARCENY and TMAX : Correlation coeffient:  0.266  . p value:  4.7496989949773195e-101

* BURGLARY and TMIN : Correlation coeffient:  0.2029  . p value:  1.0093019115182034e-58 

* ASSAULT and TMIN : Correlation coeffient:  0.2612  . p value:  2.0093839056860717e-97 
* ASSAULT and TMAX : Correlation coeffient:  0.2689  . p value:  2.359337965350694e-103

* VANDALISM and TMIN : Correlation coeffient:  0.2353  . p value:  7.045336569505856e-79 
* VANDALISM and TMAX : Correlation coeffient:  0.2445  . p value:  3.1566094195376774e-85 

* VEHICLE BREAK-IN/THEFT and TMIN : Correlation coeffient:  0.2659  . p value:  5.408061543648149e-101 
* VEHICLE BREAK-IN/THEFT and TMAX : Correlation coeffient:  0.2647  . p value:  4.38592789976704e-100 
##
    A linear regression was able to give some insight into forcasting using the least squares method.


Some concerns are that the TMAX, TMIN, PRCP, and SNOW are only cumulative or maximum for the day. They don't tell us what the weather was at the time of the crime. 

#

<!-- * ### Louisville 'Hourly' weather data was scraped from https://www.wunderground.com/history/daily/KSDF/date/ using the following program: https://github.com/dfreeman500/Scrape-Wunderground
    * 'Wunderground_KSDF_weather_2004-2020.csv' provides weather data from 2004 until portions of 2021
    * In general, this provides at least 24 observations (including: temp, humidity, precipitation, etc) per day. One day has 0 information and one day has 84 observations. 
    * General Data issues/Concerns:
        * Days with less than 24 observations (particularly only 1-2) generally appear unreliable. For example '0' degrees is a common temp. The highest temps (ex: >130) or windspeeds (ex: >100) appear on days with only 1 or 2 observations.
        * In general, days with 24 or more observations appear to be reasonable (i.e. temps gradually change with each observation). One caveat to this is that occasionally, observations **appear** to be out of order. For example: an observation at 11:56 PM may appear as the first observation on the webpage for Day Z (within the AM section) but given the trend of temperature change, it may belong within Day Y's observation. However, because scraping is done per day/page, the 11:56 PM entry is tagged as Day Z when it likely should be Day Y.  -->

