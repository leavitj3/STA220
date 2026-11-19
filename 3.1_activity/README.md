# STA 220, Activity 3.1: Mercer Fact Check

The R behind Activity 3.1 (Principles of Visualization): the eight social-media graphics on the
team cards, the benchmarks printed on the cards and the handout, and a starter script for Part B.
Data for Everyday Life, The College of New Jersey, fall 2026.

## For students

Clone this repository (clone, not fork), open `sta220-activity_3.1.Rproj`, and use `scripts/`:

1. `01_import.R` reads the User Friendly Budget Database and writes the two tables in `data/`.
   They are already there, so you only need to run it if you want to see how they were made.
2. `02_cards.R` draws the eight card figures and prints every benchmark. Open it after class to
   see how each card was built, honest or not.
3. `03_your_town.R` is for Part B. Change the two lines at the top to your town and run it; it
   prints the rows, the rates, and the real-dollar change the handout asks for.

## What is here

```
sta220-activity_3.1/
  sta220-activity_3.1.Rproj   # double-click to open the project
  README.md                   # this file
  data/
    UFB.xlsm                  # the raw User Friendly Budget Database. Never edit by hand!
    ufb_activity_2016_2025.csv  # made by 01_import.R: every NJ town in every year, 2016 to 2025
    cpi_u_2016_2025.csv       # CPI-U annual averages (BLS), for inflation adjustment
  scripts/
    01_import.R               # reads the database, writes data/*.csv
    02_cards.R                # the eight card figures and the benchmarks
    03_your_town.R            # Part B starter: your town's rows, rates, and real-dollar change
  output/
    figures/                  # the eight card figures; rerun 02_cards.R, don't edit
```

The scripts need only the tidyverse; `01_import.R` also needs `readxl`, which comes with it.

## The data

`data/UFB.xlsm` is the User Friendly Budget Database published by the New Jersey Department of
Community Affairs, Division of Local Government Services: one sheet per budget year, one row per
municipality that filed a User Friendly Budget, with Minimum, Maximum, Median, and Average rows at
the bottom. The figures are self-reported by the municipalities and have not been independently
audited. Downloaded in fall 2026.

`data/ufb_activity_2016_2025.csv` has one row per town per year. What each column is, and where it
comes from in a Summary sheet (subgroup header, then column name):

| column | sheet column |
|---|---|
| `pop` | Population and Density: Population (the Census estimate for that sheet's year) |
| `land_sqmi` | Population and Density: Land Area (sq. mi) |
| `budget` | General Budget Appropriations: Total General Budget |
| `local_tax` | Total Anticipated Revenues: Local Tax for Municipal Purposes (the municipal-purpose levy) |
| `state_aid` | Total Anticipated Revenues: State Aid (without offsetting appropriation) |
| `surplus` | Total Anticipated Revenues: Surplus ("surplus used as revenue in the budget") |
| `rev_total` | Total Anticipated Revenues: Total |
| `ft_employees` | Total Personnel Costs: Total Full-Time Employees |
| `police_ft`, `police_cost` | Police Officer Personnel Costs: # of Full-Time Employees, Total Personnel Cost |
| `app_pub_safety`, `app_pub_works`, `app_parks`, `app_debt` | General Budget Appropriations: Public Safety, Public Works, Parks and Recreation, Debt |
| `net_debt_pc` | Net Debt: Per Capita Net Debt |
| `gov_body_cost` | Governing Body Personnel Costs: Total Personnel Cost |

"No data" and "--" become `NA`. Dollar columns are in dollars, not millions.

`data/cpi_u_2016_2025.csv` is the CPI-U, all items, U.S. city average, annual average
(1982-84 = 100), from the Bureau of Labor Statistics. An amount from year y in 2025 dollars is
`amount * cpi_u[2025] / cpi_u[y]`. Check the table against bls.gov before reusing it.

## Drive times and police headquarters (OpenStreetMap)

`data/police_hq_addresses.xlsx` has one row per police department: every department in Mercer
County, plus departments in Middlesex, Burlington, Hunterdon, and Monmouth counties whose town
centers are within about 30 minutes of TCNJ (19 in all). Each row gives the web page its address
came from (`address_source`), checked October 22, 2026. The `codebook` sheet explains every column.

`data/police_hq_drive_times.csv` is made by `05_drive_times.R` and read by `06_police_salary.R`. It
has the drive time and distance from TCNJ to each headquarters, with no traffic. Two free public
services supplied it:

- The U.S. Census Bureau's geocoder turned 17 of the 19 addresses into latitude and longitude.
- OpenStreetMap supplied the rest. The coordinates for the two addresses the Census could not match
  (South Brunswick and Mansfield) were looked up in OpenStreetMap's search, Nominatim. Every drive
  time and distance was calculated by OSRM, a routing program that runs on OpenStreetMap's road
  map (router.project-osrm.org, queried October 22, 2026).

Data from OpenStreetMap is © OpenStreetMap contributors and is available under the Open Database
License (ODbL): https://www.openstreetmap.org/copyright. If you reuse the drive times, the
distances, or those two coordinates, keep this credit with them.

Nominatim and the OSRM server are shared servers with usage policies: at most one request per
second, and no bulk or heavy use. Read them before you send either one a request:
https://operations.osmfoundation.org/policies/nominatim/ and
https://routing.openstreetmap.de/about.html. You do not need to run `05_drive_times.R`, because its
output is already in `data/`.

## Manually Curating Data for Import into R

ufb_2026_police.csv was manually created following the steps below:

Sheet: I used only the “2026 Summary” tab of UFB.xlsm. I copied the cell values
and pasted as values, ignoring the data table format and removing formulas. For muni_code, data was pasted exact so it did not convert to numeric (i.e., kept the lead 0)

Dropped Rows: I dropped rows 1–5 because they were headers and rows 570-574 because they were averages. 

Keep Only Columns Related to Police Officer Personal Cost
C: Municipality
D: County
F: No UFB Available
G: Sig. Data Missing
HE–HL: the eight “Police Officer Personnel Costs” columns

Rename Columns: I renamed every column to lower-snake case and removed special characters. 

Delete "No Data": I used find-and-replace to delete all "No Data".

Format as CSV: I saved it as a .CSV. 


