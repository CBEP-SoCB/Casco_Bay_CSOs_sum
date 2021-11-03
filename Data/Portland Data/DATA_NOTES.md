<img
    src="https://www.cascobayestuary.org/wp-content/uploads/2014/04/logo_sm.jpg"
    style="position:absolute;top:10px;right:50px;" />
    
# Metadata for Portland CSO Data

## Contents of `Portland_CSO_Data_2015-2019.csv`
206 rows, containing data on individual CSO "events", defined as the duration
of a single storm during which one or more Portland CSOs discharged.

Column Name | Contents                                      | Units / Values
------------|-----------------------------------------------|--------------
event	      | Event ID (Unique only within a given year)	  | Integer
firstdate   | Starting Date of the Rain Event / CSO Event   | "%m/%d/%Y"
lastdate	  | Ending Date of the Rain Event / CSO Event	    | "%m/%d/%Y"
days	      | Number of days included in event              | Integer
totalprecip	| Total reported precipitation during the event | Inches
maxprecip	  | Maximum Hourly Precipitation during event     | Inches
CSO_002 through CSO_043 | CSO discharge volume by CSO	      | Gallons
eventtotal	| Total event discharge (Sum of all CSOs)	      | Gallons
Year	      |Calendar Year	                                | Integer
------------|-----------------------------------------------|--------------

# Contents of `Portland_Locations.csv`
Crosswalk table from Portland CSO ID code to location name

Column Name | Contents                                      | Units / Values
------------|-----------------------------------------------|--------------
CSO	        | Portland CSO ID Code in the form  "CSO_###"   | ### <= 043
Location    | Description of CSO Location                   | Text
------------|-----------------------------------------------|--------------

# Contents of `portland_rainfall_cso_summary.csv`
Summary table containing CSO Totals (events and volume) for the five year
period of record (2015 through 2019) and for the most recent year (2019).

Column Name | Contents                                      | Units / Values
------------|-----------------------------------------------|--------------
CSO	        | Portland CSO ID Code in the form  "CSO_###"   | ### <= 043
Location	  | Description of CSO Location                   | Text
Events	    | Number of CSO "Events" 2015 through 2019      | Integer
Volume	    | CSO discharge volume 2015 through 2019        | Gallons
Events2019	| Number of CSO "Events" 2019                   | Integer
Volume2019  | CSO discharge volume 2019                     | Gallons
------------|-----------------------------------------------|--------------


