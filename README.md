# Project: Analysing the US weather data of 2022 from world data.

### Description: This project processes world's weather data of 2022 to get the US data and then analysing US data to get information about weather stations particularly of Texas, New York and Vermont state. The project involves analysing of these weather stations to give insights about temperature range throughout the year. We have 13476 world stations in the form of CSVs and after data processing, we get 2900 CSVs of 2900 stations. In this project, CSV reader module is used to process the big data as this module reads the data line by line which makes this process time and memory efficient.

### Technologies Used
- Python
- CSV Reader/Writer
- Matplotlib
- Folium
### Data Sources
- World Data:
### Data Used
- US Data
- Texas Data
- New York Data

### Project Deliverables
- Python code for data cleaning, merging, and analysis
- Visualizations and graphs to show insights and patterns
- Interactive map highlighting for temperature visualization
- Final report summarizing insights of New York, Texas and Vermont stations.

### Meta Data 
| Columns  | Description | 
| ------------- |------------- |
| Station ID  | Station ID |
| Date  | Timestamp date of each data |
| Source  | Quality Code |
| Latitude  | Latitude of station |
| Longitude  | Longitude of station |
| Station Name  | Station Name |
| Elevation  | Elevation of station |
| Report Type | Report Type  |
| Call Sign  | Call Sign |
| Quality  | Quality Code |
| Wind  | Wind information of station |
| CIG  | Ceiling |
| VIS  | Visibility |
| Temperature  | Temperature | 
| Dew  | Moisture formed near station|
| SLP  | Sea Level Pressure |


### Getting New York state data from the US data
```
import csv
with open('US_Data.csv') as d:
    data = csv.reader(d)
    n=0
    for i in data:
        if len(i)>1:
            if 'NY US' in i[6]:
                with open('NY.csv','a') as f:
                    w = csv.writer(f)
                    w.writerow(i)
        n+=1
f.close()
```

- Import CSV Library
- Read US_Data line by line
- Search for New York/Vermont/Texas name
- Open another file on writer mode and then write each line in NY.csv
- close writer

