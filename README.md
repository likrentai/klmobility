# Malaysia Klang Valley Rapid Rail Activity Map

An interactive web map of how people use the Rapid Rail network (LRT, MRT and Monorail) across the Klang Valley, Malaysia, built entirely from open government data.

**Live map:** https://likrentai.github.io/klmobility/

Each station is drawn by its **level of activity** (average passenger entries per day) and coloured by its **weekend ratio**: blue stations are busy on working days and quiet at weekends, which marks weekday commuting at either the home end or the work end of the journey; red stations stay as busy or get busier at weekends, typical of shopping and leisure areas. Clicking a station draws lines to its top 5 destinations and lists what lies within 500 m of it (shops, food and drink, offices, education, healthcare, tourism and leisure, and the main land use) and who uses it (residents within 500 m, entries per resident, station type and district household income). A switch recolours the stations by **station type** instead of weekend ratio, and optional layers show land use around every station and median household income by district.

## Key findings (June to August 2026)

| Finding | Station | Figure |
|---|---|---|
| Most active station | Bukit Bintang | about 35,000 entries a day |
| Next most active | KL Sentral, KLCC | about 26,400 and 21,700 entries a day |
| Most commuter-like | Kerinchi (Bangsar South) | weekend ratio 0.27 |
| Other commuter hubs | Semantan, Raja Chulan | weekend ratios 0.32 and 0.34 |
| Most weekend-heavy | Pasar Seni (Petaling Street) | weekend ratio 1.27 |
| Other weekend draws | Imbi, Hang Tuah (Bukit Bintang area) | weekend ratios above 1.1 |

Travel is widely spread: on average, a station's top 5 destinations carry only about 43% of its trips.

## What surrounds each station (Version 2)

Places and land use within 500 m (about a 6 to 7 minute walk) of each station were counted from OpenStreetMap. The places with the most around them are the historic and shopping core of Kuala Lumpur:

| Station | Shops | Food and drink | Tourism and leisure | All places within 500 m |
|---|---|---|---|---|
| Chow Kit | 458 | 148 | 54 | 740 |
| Bukit Bintang | 283 | 337 | 88 | 735 |
| Masjid Jamek | 364 | 167 | 57 | 639 |
| Imbi | 237 | 263 | 83 | 609 |
| Pasar Seni | 254 | 195 | 51 | 538 |

### Does the surrounding area explain the weekend ratio?

The hypothesis was that weekday-heavy (blue) stations would sit among offices and weekend-heavy (red) stations among shops and leisure. Two experiments tested it, using rank correlations between each measure and the weekend ratio. The answer is: **only moderately**.

| Finding | Evidence |
|---|---|
| Gaps in OpenStreetMap weaken every link | Among the 44 stations whose 500 m circle is at least 60% mapped, the correlations roughly double |
| Retail and commercial activity lean towards weekends | Well-mapped stations: shops 0.30, commercial land 0.23, food and drink 0.21; green space -0.22 |
| Office space leans towards weekdays, but too little is mapped to confirm it | Office buildings only: -0.14; just 171 office buildings are mapped, and only 7% of buildings have their number of storeys recorded |
| Malaysian shophouses distort building-based measures | Counting `commercial` buildings as workplaces flipped the result to +0.26, because shophouses and malls carry that tag (5,821 of them against 171 offices) |
| Home-end and work-end stations cannot be told apart | Both ends of a commute are busy on weekdays, and residential buildings show no link (0.08). Hourly ridership would be needed, and it is not published for Rapid Rail |

The weekend ratio is therefore best read as **when** a station is used, with its surroundings as supporting context rather than proof.

## Who uses each station (Version 3)

Residents within 500 m of each station were estimated from a gridded population map (Kontur, 400 m hexagons). Comparing them with ridership gives **entries per resident**: a low value means a station mainly serves the people living around it; a high value means most users come from elsewhere, to work, shop or visit. Combined with the weekend ratio, this sorts every station into one of four types:

| | Weekday-heavy (weekend ratio below 0.61) | Weekend-leaning (0.61 or above) |
|---|---|---|
| **Serves its residents** (below 0.47 entries per resident) | **Home-end commuter** (38 stations), e.g. Kinrara, Kelana Jaya, Sri Petaling, Puchong Prima | **Local neighbourhood** (38), e.g. Ampang, Cempaka, Pandan Jaya, Kampung Baru |
| **Draws visitors** (0.47 or above) | **Work-end commuter** (39), e.g. Kerinchi, Raja Chulan, Ampang Park, Abdullah Hukum | **Leisure destination** (39), e.g. Bukit Bintang, KL Sentral, KLCC, Pasar Seni |

The dividing values are the network medians, so each side holds about half the stations.

**This separates the two ends of commuting, which Version 2 could not.** Kerinchi (Bangsar South) and Raja Chulan (Golden Triangle) come out as work-end stations, while Sri Petaling comes out as home-end, even though all three are weekday-heavy. The busiest places are clear destinations: Bukit Bintang has about 35,000 entries a day from roughly 7,300 residents (4.8 entries per resident), and KLCC and Pasar Seni about 3.6 each.

| Check | Result |
|---|---|
| Robustness to the radius | Doubling the radius to 1,000 m changed the type of only 1 of 154 stations; every reference station kept its type |
| Residents vs ridership | Weak link (rank correlation +0.14): the number of people within walking distance only loosely predicts how many use a station |
| Known misfits | Residential stations with park-and-ride or feeder buses (e.g. Putra Heights, Setiawangsa) appear as work-end stations, because their users live beyond walking distance; widening the radius to 1 km did not change them |

Median monthly household income by district (DOSM) ranges from RM 8,837 in Klang to RM 11,404 in Ulu Langat (2024); Kuala Lumpur is RM 10,234 and Putrajaya RM 10,056 (2022, the latest year DOSM publishes for the federal territories). It is shown as broad context only, since Kuala Lumpur is a single district.

## How it was built

| Step | Notebook | What it does |
|---|---|---|
| 1 | `01datapreparation.ipynb` | Downloads ridership and station data, cleans and joins them, and saves small ready-to-map files in `data/processed` |
| 2 | `03context1.ipynb` | Counts places and measures land use within 500 m of each station from OpenStreetMap |
| 3 | `03context2.ipynb` | Experiments: well-mapped stations only, and building floor area as a measure of workplaces and homes |
| 4 | `04population1.ipynb` | Estimates residents within 500 m of each station, computes entries per resident, assigns station types, and adds district household income |
| 5 | `02buildmap.ipynb` | Reads the prepared files and draws the interactive map, saved as `docs/index.html` |

Main methods:

- **Separating totals from flows.** The source file mixes daily station totals with station-to-station trips; mixing them doubles every count, so they are split before any analysis.
- **Station matching.** Ridership station codes (e.g. `KG18: Bukit Bintang`) are matched to GTFS coordinates by cleaned name and line code, handling interchanges, sponsor names (e.g. KL Sentral - REDONE) and differently padded codes. All 154 places are matched.
- **New stations averaged over their days in service.** A station that opened during the period is averaged only over the days it has data, not counted as zero before it opened. The Shah Alam Line (LRT3) opened on 29 June 2026 with free rides until 31 July, so its 20 stations have taps only from 1 August; without this correction their activity would be understated about threefold. Each line code is averaged separately and summed per place, so Bandar Utama (Kajang Line and LRT3) counts both platforms correctly.
- **Working days vs weekends.** Public holidays in Kuala Lumpur are grouped with weekends, because travel on those days behaves like a weekend. The period has 61 working days and 31 weekend and holiday days.
- **Spatial joins.** Each station is tagged with its district and parliamentary constituency using DOSM boundary files.
- **Areal interpolation.** Where a population hexagon is only partly inside a station's circle, it contributes the same share of its population as the share of its area inside.
- **Walking-distance circles.** 500 m circles are drawn in a metre-based map projection (UTM zone 47N), and OpenStreetMap features are counted by their centre point, so a mall counts once.

**Tools:** Python, pandas, GeoPandas, OSMnx, Folium (Leaflet), holidays, Jupyter.

## Data sources

| Data | Source | Licence |
|---|---|---|
| Daily origin-destination ridership, Rapid Rail (Klang Valley) | [data.gov.my](https://data.gov.my/data-catalogue/ridership_od_rapidrail_daily) (Prasarana) | CC BY 4.0 |
| Station locations (GTFS static feed) | [data.gov.my Open API](https://developer.data.gov.my/realtime-api/gtfs-static) (Prasarana) | CC BY 4.0 |
| District and constituency boundaries | [Department of Statistics Malaysia (DOSM)](https://github.com/dosm-malaysia/data-open) | Open data |
| Places, land use and buildings around stations | [OpenStreetMap](https://www.openstreetmap.org/copyright) contributors, via OSMnx | ODbL |
| Population, 400 m hexagons (1 November 2023) | [Kontur Population: Malaysia](https://data.humdata.org/dataset/kontur-population-malaysia) via HDX | CC BY 4.0 |
| Household income by district | [DOSM Household Income and Expenditure Survey](https://open.dosm.gov.my/data-catalogue/hh_income_district) | Open data |
| Base map | [OpenStreetMap](https://www.openstreetmap.org/copyright) contributors | ODbL |

## Limitations

- **Putrajaya Line exits are missing in the source data.** The 32 stations from Damansara Damai (PYL05) to Putrajaya Sentral (PYL41) record entries, but no trip is ever recorded as ending there. Network totals still balance, so those exits are credited to other stations and cannot be recovered. For this reason the map measures activity by entries only, and no destination line ends at those stations.
- **Daily data only.** Morning and evening peaks cannot be separated; the working-day and weekend comparison stands in for that.
- **Stations, not journeys.** Tap-in and tap-out data shows which stations people used, not where their journeys actually began or ended.
- **Rapid Rail only.** KTM Komuter and the airport rail link are not included.
- **The weekend ratio is a clue, not proof** of land use. Blue stations include both residential (home end) and office (work end) stations, and a station can be quiet at weekends for other reasons, such as a nearby university.
- **New Shah Alam Line (LRT3) stations have a short record.** Their figures cover August 2026 only, their first month of paid service, when curiosity trips may still lift weekend use. The map marks them as new.
- **Station types assume people walk to the station.** Stations with park-and-ride car parks or feeder buses draw users from further away and can be classed as destinations.
- **Population is a modelled estimate.** Kontur distributes census and satellite-derived population across hexagons; it is reliable at neighbourhood scale but not a count.
- **Income is coarse.** It is published by district only, and for Kuala Lumpur and Putrajaya the latest year is 2022.
- **OpenStreetMap is mapped by volunteers.** Counts of places show what has been mapped, not an official census; central Kuala Lumpur is well covered, some outer areas less so.

## Running it yourself

Requires Python 3.11 or later.

1. Install the libraries: `pip install pandas pyarrow geopandas osmnx folium requests holidays jupyter`
2. Run `01datapreparation.ipynb`. The first run downloads about 5 MB of ridership data and caches it in `data/raw`.
3. Run `03context1.ipynb`. The first run fetches OpenStreetMap data, which can take several minutes, and caches it.
4. Optionally run `03context2.ipynb` to repeat the experiments.
5. Run `04population1.ipynb`. The first run downloads the population (about 11 MB) and income data and caches them.
6. Run `02buildmap.ipynb`. All visual settings (marker shape, colours, sizes, text, land-use, station-type and income colours) are in its first code cell.

To refresh with newer data, change the two dates at the top of notebook 1 and follow the steps in its final section.

## Roadmap

- [x] **Version 1:** station activity, weekend ratio and top destinations
- [x] **Version 2:** what surrounds each station (shops, offices and land use from OpenStreetMap)
- [x] **Version 3:** who lives there (people within 500 m of each station from a gridded population map, station types, and household income by district)
- [ ] **Version 4:** how activity changes over time (data since 2023)

## Author
Lik Ren Tai
