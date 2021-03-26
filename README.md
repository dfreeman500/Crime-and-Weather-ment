# Crime-and-Weather-ment
## The Question: Do crime levels and weather in Louisville, KY have any relationship?
#


## The Data
#
* ### Louisville Crime data is retrieved within multiple .csv files from  https://data.louisvilleky.gov/dataset/crime-reports
    * Included are crimes occuring within 2003 until portions of 2021
    * 'Crime_Data_2019.csv' has reports from 2019-2021. Crimes occuring in 2021 are left out of the daily analysis.
    * Crime analysis is performed on the occurence date.
    * There are close to 1.4 million crimes and 1.2 million incidents listed in the dataset from 2004 til portions of 2021.
    * General Data issues/Concerns:
        * The differences betwen DATE_REPORTED and DATE_OCCURED can be years apart. This results in an unequal distribution of crimes particularly on the first of the month or first of the year. 
        * In some cases the crime is reported prior to the date of occurence (These are removed). 
        * 'Crime_Data_2003.csv' only has ~19,000 crimes reported when years 2004-2020 range from ~71,000 to ~91,000 crime reports/year. So, 2003 was left out of the anaylsis




* ### Louisville Daily weather data ('2451549.csv') is retrieved from https://www.ncdc.noaa.gov/cdo-web/datasets/GHCND/stations/GHCND:USC00154958/detail
    * This file provides daily weather information from 1997 until portions of 2021
    * In particular, the most important information used in this analysis is Precipitation in inches (PRCP), SNOW in inches, Max Temperature (TMAX), and  Minimum Temperature(TMIN).
    * General Data issues/Concerns:
        * Given that this weather information only provides the highest, lowest, or cumulative values in a day, it cannot give us information about the weather at the time of the crime. 





* ### Louisville 'Hourly' weather data was scraped from https://www.wunderground.com/history/daily/KSDF/date/ using the following program: https://github.com/dfreeman500/Scrape-Wunderground
    * 'Wunderground_KSDF_weather_2004-2020.csv' provides weather data from 2004 until portions of 2021
    * In general, this provides at least 24 observations (including: temp, humidity, precipitation, etc) per day. One day has 0 information and one day has 84 observations. 
    * General Data issues/Concerns:
        * Days with less than 24 observations (particularly only 1-2) generally appear unreliable. For example '0' degrees is a common temp. The highest temps (ex: >130) or windspeeds (ex: >100) appear on days with only 1 or 2 observations.
        * In general, days with 24 or more observations appear to be reasonable (i.e. temps gradually change with each observation). One caveat to this is that occasionally, observations **appear** to be out of order. For example: an observation at 11:56 PM may appear as the first observation on the webpage for Day Z (within the AM section) but given the trend of temperature change, it may belong within Day Y's observation. However, because scraping is done per day/page, the 11:56 PM entry is tagged as Day Z when it likely should be Day Y. 




## The Model/Conclusion:
#
* Crime and weather per day:
    * Pearson Correlation Coefficient shows only weak correlation for the following relationships:
        * Number of Crimes on a day and TMAX
        * Number of Crimes on a day and TMIN
        * Number of Assaults on a day and TMAX
        * Number of Assaults on a day and TMIN 
        * Number of Vandalisms on a day and TMAX
    * Linear regression is shown to model TMIN, TMAX, PRCP, and SNOW  and Number of crimes on a day

There is only a weak relationship between total number of crimes on a day (and certain crime types on a day) and max and minumum temperatures on that day. 

#

## Instructions:
#
* Clone the repo
* pip install -r requirements.txt
* Run crime-weather.ipynb 
* (Program written in Python 3.7.2)



For an easy way to simply view this notebook, use the following link:
https://nbviewer.jupyter.org/github/dfreeman500/Crime-and-Weather-ment/blob/main/crime-weather.ipynb