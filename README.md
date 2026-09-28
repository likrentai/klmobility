# Malaysia Klang Valley Rapid Rail Activity Map

An interactive web map of how people use the Rapid Rail network (LRT, MRT and Monorail) across the Klang Valley, Malaysia, built entirely from open government data.

**Live map:** https://likrentai.github.io/klmobility/

Each station is drawn by its **level of activity** (average passenger entries per day) and coloured by its **weekend ratio**, which hints at what kind of place it serves: blue stations empty out at weekends, like office districts; red stations stay as busy or get busier, like shopping and leisure areas. Clicking a station draws lines to its top 5 destinations.

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

## How it was built

| Step | Notebook | What it does |
|---|---|---|
| 1 | `01_prepare_v1_data.ipynb` | Downloads ridership and station data, cleans and joins them, and saves small ready-to-map files in `data/processed` |
| 2 | `02_build_v1_map.ipynb` | Reads those files and draws the interactive map, saved as `docs/index.html` |

Main methods:

- **Separating totals from flows.** The source file mixes daily station totals with station-to-station trips; mixing them doubles every count, so they are split before any analysis.
- **Station matching.** Ridership station codes (e.g. `KG18: Bukit Bintang`) are matched to GTFS coordinates by cleaned name and line code, handling interchanges, sponsor names (e.g. KL Sentral - REDONE) and differently padded codes. All 154 places are matched.
- **Working days vs weekends.** Public holidays in Kuala Lumpur are grouped with weekends, because travel on those days behaves like a weekend. The period has 61 working days and 31 weekend and holiday days.
- **Spatial joins.** Each station is tagged with its district and parliamentary constituency using DOSM boundary files.

**Tools:** Python, pandas, GeoPandas, Folium (Leaflet), holidays, Jupyter.

## Data sources

| Data | Source | Licence |
|---|---|---|
| Daily origin-destination ridership, Rapid Rail (Klang Valley) | [data.gov.my](https://data.gov.my/data-catalogue/ridership_od_rapidrail_daily) (Prasarana) | CC BY 4.0 |
| Station locations (GTFS static feed) | [data.gov.my Open API](https://developer.data.gov.my/realtime-api/gtfs-static) (Prasarana) | CC BY 4.0 |
| District and constituency boundaries | [Department of Statistics Malaysia (DOSM)](https://github.com/dosm-malaysia/data-open) | Open data |
| Base map | [OpenStreetMap](https://www.openstreetmap.org/copyright) contributors | ODbL |

## Limitations

- **Putrajaya Line exits are missing in the source data.** The 32 stations from Damansara Damai (PYL05) to Putrajaya Sentral (PYL41) record entries, but no trip is ever recorded as ending there. Network totals still balance, so those exits are credited to other stations and cannot be recovered. For this reason the map measures activity by entries only, and no destination line ends at those stations.
- **Daily data only.** Morning and evening peaks cannot be separated; the working-day and weekend comparison stands in for that.
- **Stations, not journeys.** Tap-in and tap-out data shows which stations people used, not where their journeys actually began or ended.
- **Rapid Rail only.** KTM Komuter and the airport rail link are not included.
- **The weekend ratio is a clue, not proof** of land use. A station can be quiet at weekends for other reasons, such as a nearby university.

## Running it yourself

Requires Python 3.11 or later.

1. Install the libraries: `pip install pandas pyarrow geopandas folium requests holidays jupyter`
2. Run `01datapreparation.ipynb`. The first run downloads about 5 MB of ridership data and caches it in `data/raw`.
3. Run `02buildmap.ipynb`. All visual settings (marker shape, colours, sizes, text) are in its first code cell.

To refresh with newer data, change the two dates at the top of notebook 1 and follow the steps in its final section.

## Roadmap

- [x] **Version 1:** station activity, weekend ratio and top destinations
- [ ] **Version 2:** what surrounds each station (shops, offices and land use from OpenStreetMap)
- [ ] **Version 3:** who lives there (household income and population by area)
- [ ] **Version 4:** how activity changes over time (data since 2023)

## Author
Lik Ren Tai
