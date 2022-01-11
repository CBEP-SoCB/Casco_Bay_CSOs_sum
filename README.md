# Analysis of CSO discharges in the Casco Bay Region 

<img
    src="https://www.cascobayestuary.org/wp-content/uploads/2014/04/logo_sm.jpg"
    style="position:absolute;top:10px;right:50px;" />

Data analysis archive examining more than twenty years of annual data on
combined sewer overflow discharges from the Casco Bay region, and five years of
storm by storm and site by site data From Portland, Maine.

# Statement of Purpose
CBEP is committed to the ideal of open science.  Our State of the Bay data
archives ensure the science underlying the 2020/2021 State of Casco Bay report
is documented and reproducible by others. The purpose of these archives is to
release  data and data analysis code whenever possible to allow others to
review, critique, learn from, and build upon CBEP science.

# Archive Structure
CBEP 2020/2021 State of the Bay data analysis summaries contain a selection of 
data,  data analysis code, and visualization code as used to produce 
results shared via our most recent State of Casco Bay report. Usually, these
archives are organized into two or three folders, including the following:

- Data  folder.  Contains data in simplified or derived form as used in our
data  analysis.  Associated metadata is contained in related Markdown documents,
usually `DATA_SOURCES.md` and `DATA_NOTES.md`.

- Analysis.  Contains one or more R Notebooks proceeding through the principal
data analysis steps that underpin SoCB reporting. To simplify the archives,
much preliminary analysis, and many analysis "dead ends" have been omitted. 

- Graphics.  Contains R Notebooks stepping through development of graphics, and
also copies of resulting graphics, usually in \*.png and \*.pdf formats.  These
graphics may differ from graphics as they appear in final State of the Bay
graphical layouts. Again, most draft versions of graphics have been omitted for 
clarity.

# Summary of Data Sources
The term "Combined Sewer Overflow" is used to refer to direct discharges
of combined human sewage and stormwater to waters like rivers, lakes, or the
ocean.

As sewers were developed in many cities, including Portland, Maine, only a
single set of underground pipes was commonly constructed, which carried both
rainwater and sewage (direct and untreated) from the city to nearby waters.

When the (U.S.) "Federal Water Pollution Control Act"" (better known as the
Clean Water Act) was passed in 1972, it funded construction of wastewater
treatment facilities in American cities.  However, replacing all the underground
plumbing of a city was and is a massive, expensive undertaking, so flows from
these existing "combined" sewers were diverted to the treatment plants.

Under ordinary (dry) conditions, this can be a good thing, as both sewage and
polluted runoff receive treatment.  However after heavy rains, the hydraulic
capacity of the sewer system or the wastewater treatment plants can be exceeded, 
leading to direct discharges of an untreated or only partially treated mixture
of sewage and runoff.

##  Data Contents
This data archive contains data from Maine's Department of Environmental (DEP)
Protection and the Portland Water District (PWD) pertaining to combined sewer
discharges in the Casco Bay Region.  The DEP data extends back to the late
1980s, but because of poor data quality prior to about 1997, we only review data
since the late 1990s.

The DEP data was derived from annual DEP statewide Combined Sewer OVerflow
reports. Present-day reports do not expose all of the historic data.  CBEP has
archived older reports that include the older data.

The PWD data was received in a series of e-mails from staff at the Portland
Water District over a period of years. CBEP began archiving year-end data
spreadsheets in 2015. The data was shared with CBEP (and other individuals
and organizations in the region) with an interest in water quality, but the data
may not have always been deemed "final" by PWD.

It is worth noting that annual total discharges reported in the PWD data and the 
annual total discharges for Portland reported in the DEP data do not match. The
values are close, but not identical.

## "Active" CSOs 
DEP has changed the way they depict CSO locations on available GIS data.  Today, 
the DEP data includes "Active" CSOs. Our GIS maps follow the DEP  convention,
and include the same collection of sites. Active CSOs correspond to CSOs with
active licenses, not necessarily CSOs that have discharged in the recent past. 

In fact, four CSOs in Portland listed as "active" in 2019 had no measurable
discharges from 2015 through 2019.  All of these CSOs are located on the north
side of Back Cove.  A major CSO remediation project, completed in 2013 was
designed, in part, to close those CSOs.  We are not certain why they continue to
be listed as "active".

Discharges from sites in other CSO communities (South Portland and Westbrook)
have also been sharply reduced, to the point that these CSOs have probably been
effectively retired, but we avoid trying to make that determination
without an official source.
