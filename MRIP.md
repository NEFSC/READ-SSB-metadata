# Overview
Tables: 

* MRIP_MA_SITE_LIST  : Assign Massachusetts MRIP sites to either North (GOM) or South (GB) for purposes of the GOM and GB cod stock assessments.
* MRIP_COD_ALL_SITE_LIST : Assign *all* MRIP sites to one of the four Cod stocks (WGOM, EGOM, GB, SNE)
Location: NEFSCDB1

Schema: RECDBS.  Send an email to nmfs.nec.stockeff@noaa.gov to gain access


## MRIP_MA_SITE_LIST
Recreational fishing trips that occur in Massachusetts sometimes must be assigned to the Gulf of Maine or Georges Bank.  Sites are classified as North or South.

North: 

1. Sites that are North of Cape Cod.
2. Sites on Cape Cod that face North into the Gulf of Maine.

South:

1. Sites that are South or West of Cape Cod.
2. Sites on Cape Cod that face South, West, or East into the Vineyard Sound, Buzzards Bay, or Georges Bank.

MRIP_MA_SITE_LIST contains:
site_id
site_name
city
county
county_code
state_code
lat_dec
lon_dec
stock_region_calc
stock_explanation


## MRIP_COD_ALL_SITE_LIST

MRIP_COD_ALL_SITE_LIST contains:
intsite - MRIP intercept site
nmfs_stat_area - three digit statistical area
nmfs_stock_area: stock area for cod
site_name
site_address
site_city
site_zip
site_zip_addon +4 code
county
county_code
state_code
site_lat
site_lon

It also contains lots of other MRIP site fields.




Scott assigned many/most of the statistical areas for the SNE sites.  

Brian Linton  finished assigning stat areas (NMFS_STAT_AREA) for the SNE intercept sites that Scott wasn't able to get to before he left.  There were a handful of unidentified sites that did not have any location information associated with them.  In those cases, Brian assigned stat areas based on the stat areas of sites with adjacent site ID numbers (INTSITE) and known locations.  This is the approach that Scott recommended, when we discussed the issue.  In general, those unidentifed sites only had one or two angler interviews each, and those interviews were from the 80s and 90s.


# Current Collection Methods
These sites were compiled by Scott Steinback.

# Changes to Collections Methods

# Tips and Tricks

# General Caveats

# Sample Code
Here is some sample SQL to get the data:  
```
select SITE_ID, STOCK_REGION_CALC from recdbs.mrip_ma_site_list;
```



Starting with MRIP data trip data, I 
```
merge m:1 site_id using "${data_raw}/ma_site_allocation.dta", keepusing(stock_region_calc) keep(1 3)
rename  site_id intsite
drop _merge

/*classify into GOM or GBS */
gen str3 area_s="AAA"

replace area_s="GOM" if st==23 | st==33
replace area_s="GOM" if st==25 & strmatch(stock_region_calc,"NORTH")
replace area_s="GBS" if st==25 & strmatch(stock_region_calc,"SOUTH")

```
To classify trips into either the Gulf of Maine or Georges Bank.




To get the data from MRIP_COD_ALL_SITE_LIST
```
select * from RECDBS.MRIP_COD_ALL_SITE_LIST;
```



# Sample Projects
* Min-Yang uses this to classify trips that target cod/haddock into GOM or GB.

# Update Frequency and Completeness

Infrequent

# Other Metadata sources


# Related Tables




## Support Tables

