
<img src="https://www.cascobayestuary.org/wp-content/uploads/2014/04/logo_sm.jpg"
    style="position:absolute;top:10px;right:50px;" />

# Data Sources
## Portland CSO Discharge Data
Data on Portland CSOs was received in e-mails from Dennis Welch, of
Portland Water District, sent to Curtis C. Bohlen.

Dennis sends monthly summaries of CSO activity to a list of water quality
stakeholders, including CBEP.

December reports each year contain data for the previous 12 months, providing a
convenient annual summary.
 
In some cases, (e.g., 2015 data) the results we received had not been certified 
as "FINAL", but they are thought to be correct.

Portland CSO Data came from from e-mails from Dennis Welch, of PWD, sent

*  1/11/2016,  
*  3/2/2017,   
*  1/26/2018,   
*  1/30/2019,
*  10/01/2020.

2016, 2017, and 2018 data were in CBEP e-mail archives. The 2015 e-mail is no 
longer accessible via GMail. The 2015 data was retrieved from archives of data
analysis related to preparing the Casco Bay Nutrient Council report several 
years ago.

A request for the complete 2019 data was sent to Denis Welch on September 30, 
2020. He responded October 1, 2020 with the 2019 files, in two separate e-mails.

E-mails contained both CSO volume and rainfall data, in separate excel 
spreadsheets.  Rainfall data includes both daily rainfall totals and the highest 
hourly rainfall of the day. We did not use these rainfall data in SoCB
reporting, so the data is not presented here in this summary repository.

The data from 2015 uses a slightly different data format.  It is
structured with a row for each DAY within events.  All other data sheets have a
row for each EVENT, with a start date and an end date. We transformed the data
into a consistent format by events.

## Statewide Annual CSO Discharge Totals by Town
Maine Department of Environmental Protection provides annual CSO monitoring
reports, which include large data tables showing measured or estimated CSO
VOlumes going back to the late 1980s.  The reports are available through the 
Maine DEP website [here](https://www.maine.gov/dep/water/cso/).

The 2016, 2017, and 2018 reports were downloaded from that web site by Tyler
Walsh on December 17th, 2019.  The 2019 Report was downloaded by Curtis C. 
Bohlen on  June 15, 2020.

Each report contains data set from recent years only.  The 2019 report shows
data from 1987 through 2019, but it omits 1989 through 2004 "for legibility."

CBEP retained a copy of DEP's 2008 report in our archives, which includes
the older data.  However, much of the data collected before the mid 1990s was
estimated, and not measured, so it is not especially helpful in this context.

### Citations:
> Maine Department of Environmental Protection. 2020.  Maine Combined Sewer
  Overflow 2019 Status Report.  Document No.: DEPLQ0972L-2020. Contact: Michael
  S. Riley, P.E., CSO Abatement Coordinator, Bureau of Water Quality, Maine DEP,
  Augusta, Maine.  Available at: https://www.maine.gov/dep/water/cso/. Accessed
  June 15, 2020.
  
> Maine Department of Environmental Protection. 2017.  Maine Combined Sewer
  Overflow 2016 Status Report.  Document No.: DEPLQ0972I-2017. Contact: Michael
  S. Riley, P.E., CSO Abatement Coordinator, Bureau of Water Quality, Maine DEP,
  Augusta, Maine.  Available at: https://www.maine.gov/dep/water/cso/. Accessed
  December 17th, 2019.
  
> Maine Department of Environmental Protection. 2009.  Maine Combined Sewer
  Overflow 2008 Status Report.  Document No.: DEPLW0972-2009. Prepared By:
  John N. True, P.E., CSO  Coordinator, Division of Water Quality Management,
  Bureau of Land and Water Quality Control, Department of Environmental
  Protection. 
  
## GIS Data
The original geospatial data was downloaded on September 12, 2019 by
Curtis C. Bohlen, from:
https://geolibrary-maine.opendata.arcgis.com/datasets/mainedep-cso

The equivalent data has since moved to:
https://hub.arcgis.com/datasets/maine::mainedep-cso

The source file contains a statewide CSO data layer (in shapefile format),
derived from permit data. The source file includes both "Active" and "Inactive"
CSOs Our derived data and analyses focus on the "Active" CSOs only, and 
only CSOs located within the Casco Bay watershed.

We received an e-mail from Fred Dillon of the City of South Portland, 
identifying two CSO locations that are no longer Active, but that continue to be 
listed as Active on the  DEP geospatial data. Those locations have been
removed from the geospatial data reported here.

The DEP data provides an attribute  "OUTFALL_ID", a three digit string, which is
unique only within towns.  The data City of Portland data uses identifiers of
the form "CSO ###", where ### is the three digit string. The crosswalk works as
expected once the State CSO geospatial data is restricted only to Portland CSOs.
CSOs in adjacent towns (especially South Portland) often have the same three 
digit identifier.  We checked the success of the crosswalk by looking at the
location of each CSO in GIS and comparing it to the narrative description 
included in the Portland CSO files.

## Weather and Precipitation Data
The Annual Portland Jetport weather data was downloaded from NOAA's
National Centers for Environmental Information APIs using a small Python 
program, written by CBEP staff. 

Data was downloaded to the file "Annual_Weather_PWD.csv" in "Long" format,
based on the "GSOY", or Global Summary of the Year data format.  Annual
total precipitation, the only value used here, is "PRCP".

Data documentation can be found here:
https://www.ncdc.noaa.gov/cdo-web/datasets


