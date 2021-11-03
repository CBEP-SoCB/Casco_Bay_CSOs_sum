<img
    src="https://www.cascobayestuary.org/wp-content/uploads/2014/04/logo_sm.jpg"
    style="position:absolute;top:10px;right:50px;" />
    
# Metadata for DEP CSO Data

## Contents of `DEP_Annual_Totals.csv`
165 rows, containing data on annual CSO Totals (Volume, Events, and Outfalls) 
beginning in 1987 for five Casco Bay towns, including:  

*  Cape Elizabeth
*  Portland & PWD
*  South Portland
*  Westbrook
*  Yarmouth

Data prior to 1997 is incomplete, and often appears to have been estimated, not 
based on actual monitoring.  It should be used with extreme caution.  Cape
Elizabeth data prior to 2002 is also incomplete.

Column Name | Contents                | Units / Values
------------|-------------------------|--------------
Community	  | Name of the Community   | Text
Year	      | Year                    | Integer
Volume      | Total annual discharge  | Gallons
Events      |	Number of CSO "Events"  | Integer
Outfalls    | Number of "Active" outfalls | Integer
------------|-------------------------|--------------

Here, a CSO "Event" consists of the combination of a storm and discharge 
from a CSO, so more than one "event" can occur during a storm if
multiple CSO locations discharge.
