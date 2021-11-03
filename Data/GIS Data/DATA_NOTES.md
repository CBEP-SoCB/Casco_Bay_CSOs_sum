<img
    src="https://www.cascobayestuary.org/wp-content/uploads/2014/04/logo_sm.jpg"
    style="position:absolute;top:10px;right:50px;" />
    
# Metadata for DEP GIS Data
Both the `Portland_CSOs` and the `Regional_CSOs` shapefiles are derived
directly from the downloaded DEP data, which contained no metadata.  Most
fields principally relate to identifying the CSO, the associated facility or 
town.  Out interpretation of the attributes are as follows.  These may not be 
correct.

Column Name | Contents                          | Units / Values
------------|-----------------------------------|--------------
OBJECTID    | Arbitrary ID assigned by GIS      | Integer
FACILITY_N  | Organization holding the permit   | Text
OUTFALL_NA  | Text name of outfall              | Text
NEPDES_FAC  | NPDES Permit Number?              | Text
OUTFALL_ID  | Outfall id, NOT  unique.          | Text 
MASTERID    | Combination of Permit number and outfall ID? | Text
MAINE_LICE  | Maine License number?
LATITUDE    | Latitude;  Apparently WGS 1984    | Float
LONGITUDE   | Longitude; apparently WGS 1948    | Float
TOWN        | Town NAme                         | Test
------------|-----------------------------------|--------------

To facilitate alignment with the Portland CSO discharge data, we joined with
the `portland_cso_summary.csv` file and imported the following
attributes:

Column Name | Contents                                      | Units / Values
------------|-----------------------------------------------|--------------
CSO	        | Portland CSO ID Code in the form  "CSO_###"   | ### <= 043
Location	  | Description of CSO Location                   | Text
Events	    | Number of CSO "Events" 2015 through 2019      | Integer
Volume	    | CSO discharge volume 2015 through 2019        | Gallons
Events2019	| Number of CSO "Events" 2019                   | Integer
Volume2019  | CSO discharge volume 2019                     | Gallons
------------|-----------------------------------------------|--------------

Volume2019
