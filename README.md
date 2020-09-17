# DATA601-HW1

## Overview

The city of Washington, DC posts a series of [open data](https://opendata.dc.gov) which report operating metrics for public services and list information regarding public schools, libraries and parks.  The [311 City Service Requests in 2019](https://opendata.dc.gov/datasets/311-city-service-requests-in-2019) data represents non-emergency requests to the city.  These 365,005 requests were made during the calendar year 2019 and includes information about the issue, when and where it was made and the progress made to resolve it.

## Getting Started

The data file for this effort is large - over 100MB - which is too large too host in GitHub.  Run the script below to retrieve it:

```shell script
mkdir data
cd data
curl https://opendata.arcgis.com/datasets/98b7406def094fa59838f14beb1b8c81_10.csv -o 311_City_Service_Requests_in_2019.csv
cd ../
```

Or, create a directory `data` and download the 'Spreadsheet' from the [OpenData DC website](https://opendata.dc.gov/datasets/311-city-service-requests-in-2019) and place into `data`.
 
## Hypothesis

- The volume of requests will correspond to the population of the wards, ie the most populous wards will have the most requests.
- The city resolved 70% of requests are resolved in the established service window.
- Visualize where the requests are routed, ie what departments are responsible for the work?

## Data Cleaning

The primary tasks for data cleaning for this project revolved around casting data types - floats to ints and strings to datetime.  

## Results

### Volume of Requests by Ward

Requests by ward from the data set.  Ward 0 represents requests where the ward was not supplied.

| Ward | Requests |
|---|---|
| 6 | 58853 |
| 2 | 53724 |
| 5 | 46616 |
| 4 | 42809 |
| 7 | 40001 |
| 1 | 37371 |
| 8 | 33112 |
| 3 | 30615 |
| 0 | 2413 |

Washington, DC's wards are fairly similar in size according to the [2010 Census](https://dcist.com/story/11/03/24/detailed-dc-census-numbers-revealed/). 

| Ward | Population |
|---|---|
| 1 | 76,197 |
| 2 | 79,915 |
| 3 | 77,152 |
| 4 | 75,773 |
| 5 | 74,308 |
| 6 | 76,598 |
| 7 | 71,068 |
| 8 | 70,712 |

##### Conclusion

There is no connection between the population of of a ward and the volume of 311 requests.  
There may be another dimension that correlates the frequency of 311 service requests. 

#### On-Time Resolution

```python
# calculate if resolution was before due date and store in a new column
data_set['RESOLVED_ON_TIME'] = data_set['RESOLUTIONDATE'] < data_set['SERVICEDUEDATE']
data_set[data_set['RESOLVED_ON_TIME'] == True].count()['RESOLVED_ON_TIME'] / data_set.count()['RESOLVED_ON_TIME']
0.8365623390079707
```

<img src="https://github.com/burgwyn/DATA601-HW1/blob/master/images/RESOLVED_ON_TIME.png" alt="Pie chart of on-time resolution" />

##### Conclusion

The public service departments of Washington, DC have completed 84% of service requests in the established service window.

#### Departments Executing Service Requests

<img src="https://github.com/burgwyn/DATA601-HW1/blob/master/images/ORGANIZATION.png" alt="Pie chart of organizations" />

##### Conclusion

The Department of Public Works (DPT) and Department of Transportation (DOT) are responsible for the majority of the service requests.

## Conclusion and Future Analysis

This data set, along with previous years' data, could influence a number of decisions in how Washington, DC interacts with it's citizens.  In the year 2019 the city fulfilled 84% of citizen's requests in a timely manner.
When faith in government is waning, success like this if worth touting.  Displaying a dashboard or a service report card would provide transparency and accountability to departments.

Going forward, this data set could influence how the city chooses to budget services and prioritize the execution of work.
- Departments could be stationed closer to the wards that need work
- Frequency of events like 'Trash Collection - Missed' and where they happen
- Using past data to inform level of effort estimates, ie the time to resolve 'Graffiti Removal'

Lastly, there is a wealth of geographic information in the data set that could be visualized on a map to help show where service requests are coming from.

## Project Info

Contributors: [Nat Burgwyn](https://github.com/burgwyn)

Languages: Python

Tools: PyCharm, Jupyter Notebook

Libraries: pandas
