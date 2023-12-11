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

### Getting Id and Names of all the stations
##### Created a dictionary stations which takes all station id's as key and names as value
```
import csv
n=0
stations={}
with open('NY.csv','r') as f:
    data= csv.reader(f)
    for i in data:
        if len(i)>1:
            if i[6] not in stations:
                stations[i[0]]=i[6]39
        n+=1
stations
```
##### We have 39 Stations in the US as follow

| STATION    |  NAME
-----------|--------------------------------------------|
|72055399999 |  PORT AUTH DOWNTN MANHATTAN WALL ST HEL, NY US
|72498894704 | DANSVILLE MUNICIPAL AIRPORT, NY US
72501454780 | MONTAUK AIRPORT, NY US
72501504789 | MONTGOMERY ORANGE CO AIRPORT, NY US
72501654790 | SHIRLEY BROOKHAVEN AIRPORT, NY US
72503014732 | LAGUARDIA AIRPORT, NY US
72503614757 | POUGHKEEPSIE AIRPORT, NY US
72503794745 | WESTCHESTER CO AIRPORT, NY US
72503814714 | STEWART FIELD, NY US
72505004781 | ISLIP LI MACARTHUR AIRPORT, NY US
72505394728 | NY CITY CENTRAL PARK, NY US
72514554746 | MONTICELLO SULLIVAN, NY US
72514654773 | FULTON OSWEGO CO AIRPORT, NY US
72515004725 | BINGHAMTON GREATER AP, NY US
72515594761 | ITHACA TOMPKINS CNTY, NY US
72515614748 | ELMIRA CORNING REGIONAL AIRPORT, NY US
72515754757 | WELLSVILLE MUNICIPAL AIRPORT, NY US
72518014735 | ALBANY INTERNATIONAL AIRPORT, NY US
72518699999 | OGDENSBURG INTERNATIONAL, NY US
72519014771 | SYRACUSE HANCOCK INTERNATIONAL AIRPORT, NY US
72519454778 | PENN YAN AIRPORT, NY US
72519664775 | ROME GRIFFISS AIRFIELD, NY US
72522014750 | GLENS FALLS AIRPORT, NY US
72523504720  |JAMESTOWN, NY US
72528014733  |BUFFALO NIAGARA INTERNATIONAL, NY US
72528300465 | CATTARAUGUS CO OLEAN AIRPORT, NY US
72528704724 | NIAGARA FALLS INTERNATIONAL AIRPORT, NY US
72529014768 | ROCHESTER GREATER INTERNATIONAL, NY US
72622394725 | MASSENA INTERNATIONAL AIRPORT, NY US
72622564776 | PLATTSBURGH INTERNATIONAL AIRPORT, NY US
72622794790 | WATERTOWN INTERNATIONAL AIRPORT, NY US
72622894740 | SARANAC LAKE ADIRONDACK REGIONAL AIRPORT, NY US
74370014715 | WHEELER SACK FIELD ARMY AIR FIELD, NY US
74486094789 | JFK INTERNATIONAL AIRPORT, NY US
74486454787 | FARMINGDALE REPUBLIC AIRPORT, NY US
74486514719 | WESTHAMPTON GABRESKI AIRPORT, NY US
74498914747 | DUNKIRK CHAUTAUQUA CO AIRPORT, NY US
74499404741 | SCHENECTADY, NY US
------------

#### Checked weather location of each station is same or not
##### This code returns a dictionary LatLong which contains Id's of stations whose location is changed. After execution this code returns an empty dictionary which states that no station changed its location in 2022. 
#### This gives us an insight about the mobility of each station.

```
import csv
LatLong={}
Location={}
with open('Vermont.csv','r') as f:
    n=0
    data= csv.reader(f)
    for i in data:
        if len(i)>1:
            if i[0] not in LatLong:
                LatLong[i[0]]=[i[3],i[4]]
            elif LatLong[i[0]]!=[i[3],i[4]]:
                Location[i[0]]=[LatLong[i[0]],[i[3],i[4]]]
            n+=1
Location
```
#### Created a function temperature for better aesthetics and flexibility of the code. This function takes a file as an atttribute and returns a dictionary with station ID's and average tempereature of 2022.
##### The Code:
```
def temperature(file):
    with open(file) as f:
        temp={}
        n=0
        data= csv.reader(f)
        for i in data:
            if len(i)>1 and 'TMP' not in i[13][:4] and -30<int(i[13][:4])<50:
                if i[0] not in temp:
                    n=1
                    temp[i[0]]=int(i[13][:4])
                    
                else:
                    n+=1
                    temp[i[0]]=((temp[i[0]]*(n-1))+int(i[13][:4]))/n

    return temp
```
- This function takes the file and reads it with CSV module
- Then it applies algebric algorithm to get avg temp of 2022
- returns a dictionary temp
```
temp=temperature('NY.csv')
```
|Staions  | Average Temperature in degree celsius 2022 |
|------------|-------------------------
| PORT AUTH DOWNTN MANHATTAN WALL ST HEL, NY US |   14.0869
| DANSVILLE MUNICIPAL AIRPORT, NY US        |        9.00484
| MONTAUK AIRPORT, NY US                     |      12.0452
| MONTGOMERY ORANGE CO AIRPORT, NY US         |      9.41202
| SHIRLEY BROOKHAVEN AIRPORT, NY US            |    12.0923
| LAGUARDIA AIRPORT, NY US                      |   13.2535
| POUGHKEEPSIE AIRPORT, NY US                    |  10.6125
| WESTCHESTER CO AIRPORT, NY US     |               11.6227
| STEWART FIELD, NY US              |               11.0868
| ISLIP LI MACARTHUR AIRPORT, NY US  |              12.1106
| NY CITY CENTRAL PARK, NY US         |             12.8792
| MONTICELLO SULLIVAN, NY US           |             8.4857
| FULTON OSWEGO CO AIRPORT, NY US       |            7.59787
|BINGHAMTON GREATER AP, NY US            |          7.25353
| ITHACA TOMPKINS CNTY, NY US             |          7.85078
| ELMIRA CORNING REGIONAL AIRPORT, NY US   |         8.49377
|WELLSVILLE MUNICIPAL AIRPORT, NY US        |       7.31835
|ALBANY INTERNATIONAL AIRPORT, NY US         |      9.84477
|OGDENSBURG INTERNATIONAL, NY US              |     7.83195
|SYRACUSE HANCOCK INTERNATIONAL AIRPORT, NY US |    8.90541
|PENN YAN AIRPORT, NY US                        |   8.02926
|ROME GRIFFISS AIRFIELD, NY US                   |  7.7383
|GLENS FALLS AIRPORT, NY US                       | 8.1868
|JAMESTOWN, NY US                           |       6.28938
|BUFFALO NIAGARA INTERNATIONAL, NY US        |      8.3834
|CATTARAUGUS CO OLEAN AIRPORT, NY US          |     6.98198
|NIAGARA FALLS INTERNATIONAL AIRPORT, NY US    |    8.36426
|ROCHESTER GREATER INTERNATIONAL, NY US         |   8.14241
|MASSENA INTERNATIONAL AIRPORT, NY US            |  6.53808
|PLATTSBURGH INTERNATIONAL AIRPORT, NY US         | 7.40396
|WATERTOWN INTERNATIONAL AIRPORT, NY US       |     7.44666
|SARANAC LAKE ADIRONDACK REGIONAL AIRPORT, NY US |  4.62823
|WHEELER SACK FIELD ARMY AIR FIELD, NY US         | 6.15612
|JFK INTERNATIONAL AIRPORT, NY US                | 12.5583
|FARMINGDALE REPUBLIC AIRPORT, NY US          |    12.5503
|WESTHAMPTON GABRESKI AIRPORT, NY US           |   11.4724
|DUNKIRK CHAUTAUQUA CO AIRPORT, NY US           |   8.63795
|SCHENECTADY, NY US                              | 11.4279
-----------------------------------------------  --------

#### Visualized the temperature range of all the stations.
#### This Bar chart shows temperature range in New York State.

![Bar](https://github.com/varadpawar1/Big-Data-Final-Project/assets/102013045/7c15b435-267d-468d-8c67-91f39a681706)

#### Highest Avg Temperatre
| PORT AUTH DOWNTN MANHATTAN WALL ST HEL, NY US |   14.0869
|------------------|--------------
#### Lowest Avg Temperature
|SARANAC LAKE ADIRONDACK REGIONAL AIRPORT, NY US |  4.62823
|-------|-------
