# India School Coordinates (UDISE+)

Location and directory data for **every school in India**, keyed by UDISE code, with GPS
coordinates for all operational schools.

- **1,730,760 schools** (all statuses) — one row each.
- **1,460,148 operational schools carry latitude/longitude** (84% of all rows; ~100% of operational).
- One source, one vintage: coordinates pulled from the Ministry of Education's public **Know Your
  School** service, keyed by each school's current internal `schoolId`.

## Getting the data

The full CSV is ~962 MB (156 MB gzipped) — too large for the git tree, so it ships as a **Release
asset**. Download the latest from the [Releases](../../releases) page:

```bash
# gzipped (156 MB)
curl -L -o schools_india_with_coords.csv.gz \
  https://github.com/<owner>/india-school-coordinates/releases/latest/download/schools_india_with_coords.csv.gz
gunzip schools_india_with_coords.csv.gz
```

`data/sample_1000.csv` in this repo is the first 1,000 rows, so you can see the schema without
downloading the full file.

## Columns (67)

Directory fields first, then coordinates, then the full set of fields returned per school.

| column | meaning |
|---|---|
| `udise_code` | 11-digit UDISE school code (primary key) |
| `school_name` | school name |
| `state_code`, `state` | UDISE state code + name |
| `district_code`, `district` | UDISE district code + name |
| `block_code`, `block` | UDISE block code + name |
| `village_or_ward`, `cluster` | village/ward, cluster |
| `category`, `management`, `school_type`, `location` | school category / management / type / rural-urban |
| `status` | Operational / Closed / Merged / etc. |
| `pincode` | postal code |
| **`latitude`, `longitude`** | **GPS coordinates (decimal degrees); blank for non-operational schools** |
| `coord_source` | `kys_by_year` where a coordinate is present |
| `coord_pulled_at` | when the coordinate was fetched (UTC) |
| `schoolId` | KYS internal id (the key used to fetch coordinates) |
| … | the remaining columns are every other field the source returns per school: management/type/category ids and descriptions, `isOperational2018To19`…`2022To23`, class range, `address`, `email`, the full `lgd*` local-government fields, `lastmodifiedTime`, etc. See `data/sample_1000.csv`. |

## Provenance & caveats

- **Source:** `kys.udiseplus.gov.in` `school/by-year` endpoint, pulled 2026-09-03 → 2026-09-09.
- **Coordinate vintage:** these are largely the 2019–2021-era UDISE coordinates (a comparison against
  the DataMeet 2021 layer showed a median displacement of 0 m). Treat them as **~2021-vintage geocodes,
  not freshly verified GPS.** A thin fraction have been corrected/moved since.
- **~2.2% of operational schools share a coordinate with 3+ other schools** — these are village/block
  **centroids** typed once and reused, not precise per-school locations. ~200 schools have no usable
  coordinate at all (null/0 at source). No attempt was made to substitute coordinates from any other
  source — every coordinate here comes from the one KYS pull.
- **Not affiliated with or endorsed by the Ministry of Education.** This is a convenience compilation of
  publicly available data.

## License

Underlying data © Government of India (UDISE+), redistributed as public information. This compilation is
released under the [Open Data Commons Open Database License (ODbL) 1.0](https://opendatacommons.org/licenses/odbl/1-0/)
— use freely with attribution and share-alike. If the Ministry requests changes to redistribution, they
will be honored.
