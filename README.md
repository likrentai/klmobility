# Malaysia Klang Valley Rapid Rail Activity Map

An interactive web map of how people use the Rapid Rail network (LRT, MRT and Monorail) across the Klang Valley, Malaysia, built entirely from open government data.

**Live map:** https://likrentai.github.io/klmobility/

Each station is drawn by its **level of activity** (average passenger entries per day) and coloured by its **weekend ratio**: pale stations are busy on working days and quiet at weekends, which marks weekday commuting at either the home end or the work end of the journey; dark blue stations stay as busy or get busier at weekends, typical of shopping and leisure areas. Clicking a station draws lines to its top 5 destinations and lists what lies within 500 m of it (shops, food and drink, offices, education, healthcare, tourism and leisure, and the main land use) and who uses it (residents within 500 m, entries per resident, station type and district household income). A switch recolours the stations by **station type** or by **change since 2023**, and optional layers show land use around every station and median household income by district. A **time slider** resizes every station to its average entries in any month from January 2023 to September 2026, and the station panel includes a small chart of that station's monthly entries.

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

The hypothesis was that weekday-heavy stations would sit among offices and weekend-heavy stations among shops and leisure. Two experiments tested it, using rank correlations between each measure and the weekend ratio. The answer is: **only moderately**.

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

## How use has changed since 2023 (Version 4)

Average daily entries were calculated for every station and every calendar month from January 2023 to September 2026. To compare like with like, each station's June to August 2026 average is set against its June to August 2023 average, so school holidays and festive seasons fall in the same place in both years.

**The network grew by about a third.** Across the 135 stations that can be compared, entries rose from about 459,000 to 604,000 a day (+31.5%); the typical station grew by 29%, and only 2 stations fell. The network was at its lowest in April 2023 (about 398,000 a day, the Hari Raya month) and at its highest in August 2026 (about 638,000). Growth is slowing:

| June to August | Network entries a day | Change on the year before |
|---|---|---|
| 2023 | 459,000 | |
| 2024 | 548,000 | +19% |
| 2025 | 600,000 | +10% |
| 2026 | 614,000 | +2% (including the new Shah Alam Line from August) |

**The fastest growth came from new development and newer stations:**

| Station | June to August 2023 | June to August 2026 | Change | Likely reason |
|---|---|---|---|---|
| Tun Razak Exchange | 5,116 | 15,566 | +204% | [The Exchange TRX](https://en.wikipedia.org/wiki/The_Exchange_TRX) mall opened in November 2023; the largest gain of any station (+10,450 a day) |
| Pusat Bandar Damansara | 1,456 | 4,994 | +243% | [Pavilion Damansara Heights](https://en.wikipedia.org/wiki/Pavilion_Damansara_Heights) opened in stages from 2023 |
| Kwasa Damansara | 578 | 1,760 | +204% | Growing new township, and the interchange between the Kajang and Putrajaya Lines |
| CGC Glenmarie | 3,341 | 8,111 | +143% | Glenmarie business area; possibly also passengers transferring to the new Shah Alam Line in August 2026 |
| Putra Permai, Kuchai, Conlay | under 1,200 | 1,000 to 2,900 | +148% to +190% | Newer or suburban stations building up their ridership |

**The biggest hubs barely grew.** Masjid Jamek (+0.7%), KL Sentral (+3.4%) and KLCC (+6.3%) stayed almost level, while Damai (-7.8%) and Chow Kit (-2.0%) fell slightly. The extra trips went elsewhere, and not only to the suburbs: Bukit Bintang (+22.5%, about 6,400 more entries a day), Maluri and Ampang Park (about 5,000 more each) gained the most after Tun Razak Exchange. Growth was broad rather than concentrated: the median station grew by 35% in Kuala Lumpur and by 27% in Petaling.

| Check | Result |
|---|---|
| Stations too new to compare | The 19 Shah Alam Line (LRT3) stations opened in 2026 and are coloured separately on the map. Bandar Utama is compared on its Kajang Line platform only (+68.6%), so its new LRT3 platform does not count as growth |
| Putrajaya Line phase 2 | Opened on 16 March 2023; its stations are compared from June to August 2023, their first full summer, so part of their growth is early ramp-up |
| Small starting numbers | Large percentages at quiet stations (e.g. Sultan Ismail, +229% from about 530 a day) mean little in absolute terms; the map shows size and change together for this reason |

## How it was built

| Step | Notebook | What it does |
|---|---|---|
| 1 | `01datapreparation.ipynb` | Downloads ridership and station data, cleans and joins them, and saves small ready-to-map files in `data/processed` |
| 2 | `03context1.ipynb` | Counts places and measures land use within 500 m of each station from OpenStreetMap |
| 3 | `03context2.ipynb` | Experiments: well-mapped stations only, and building floor area as a measure of workplaces and homes |
| 4 | `04population1.ipynb` | Estimates residents within 500 m of each station, computes entries per resident, assigns station types, and adds district household income |
| 5 | `05timeline1.ipynb` | Downloads the yearly ridership files from 2023, computes average daily entries by station and month, and compares June to August 2023 with June to August 2026 |
| 6 | `02buildmap.ipynb` | Reads the prepared files and draws the interactive map, saved as `docs/index.html` |

Main methods:

- **Separating totals from flows.** The source file mixes daily station totals with station-to-station trips; mixing them doubles every count, so they are split before any analysis.
- **Station matching.** Ridership station codes (e.g. `KG18: Bukit Bintang`) are matched to GTFS coordinates by cleaned name and line code, handling interchanges, sponsor names (e.g. KL Sentral - REDONE) and differently padded codes. All 154 places are matched.
- **New stations averaged over their days in service.** A station that opened during the period is averaged only over the days it has data, not counted as zero before it opened. The Shah Alam Line (LRT3) opened on 29 June 2026 with free rides until 31 July, so its 20 stations have taps only from 1 August; without this correction their activity would be understated about threefold. Each line code is averaged separately and summed per place, so Bandar Utama (Kajang Line and LRT3) counts both platforms correctly.
- **Fair comparisons over time.** Monthly figures are averages over the days each line code had data, so a station that opened mid-month is not understated, and a month with fewer than 7 days of data is left out. The change since 2023 compares the same three months in both years, and only line codes with at least 30 days of data in both.
- **Working days vs weekends.** Public holidays in Kuala Lumpur are grouped with weekends, because travel on those days behaves like a weekend. The period has 61 working days and 31 weekend and holiday days.
- **Spatial joins.** Each station is tagged with its district and parliamentary constituency using DOSM boundary files.
- **Areal interpolation.** Where a population hexagon is only partly inside a station's circle, it contributes the same share of its population as the share of its area inside.
- **Walking-distance circles.** 500 m circles are drawn in a metre-based map projection (UTM zone 47N), and OpenStreetMap features are counted by their centre point, so a mall counts once.

**Tools:** Python, pandas, GeoPandas, OSMnx, Folium (Leaflet), holidays, Jupyter.

## Data sources

| Data | Source | Licence |
|---|---|---|
| Daily origin-destination ridership, Rapid Rail (Klang Valley), 2023 to 2026 | [data.gov.my](https://data.gov.my/data-catalogue/ridership_od_rapidrail_daily) (Prasarana) | CC BY 4.0 |
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
- **The weekend ratio is a clue, not proof** of land use. Weekday-heavy stations include both residential (home end) and office (work end) stations, and a station can be quiet at weekends for other reasons, such as a nearby university.
- **New Shah Alam Line (LRT3) stations have a short record.** Their figures cover August 2026 only, their first month of paid service, when curiosity trips may still lift weekend use. The map marks them as new.
- **Change over time compares two summers.** June to August 2023 and 2026 are like-for-like, but one period cannot show every shift; the monthly chart for each station shows the full pattern. The last month shown may be incomplete.
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
6. Run `05timeline1.ipynb`. The first run downloads the yearly ridership files from 2023 and caches them.
7. Run `02buildmap.ipynb`. All visual settings (marker shape, colours, sizes, text, land-use, station-type, income and change colours, and the time slider) are in its first code cell.

To refresh with newer data, change the two dates at the top of notebook 1 and follow the steps in its final section, then rerun `05timeline1.ipynb` (it downloads the current year again) and `02buildmap.ipynb`.

## Roadmap

- [x] **Version 1:** station activity, weekend ratio and top destinations
- [x] **Version 2:** what surrounds each station (shops, offices and land use from OpenStreetMap)
- [x] **Version 3:** who lives there (people within 500 m of each station from a gridded population map, station types, and household income by district)
- [x] **Version 4:** how activity changes over time (monthly time slider, change since 2023, and a monthly chart for each station)
- [ ] **Next:** seasonal and festive patterns (Hari Raya, Chinese New Year, school holidays)

## Author
Lik Ren Tai
