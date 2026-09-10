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

Directory fields first, then coordinates, then the full set of fields returned per school. The
**example** column shows one real row — `3 R'S PUBLIC SCHOOL BARA CHANDGANJ LUCKNOW` (a private
primary school in Lucknow, Uttar Pradesh).

| column | meaning | example |
|---|---|---|
| `udise_code` | 11-digit UDISE school code (primary key) | `09270912620` |
| `school_name` | school name | `3 r'S PUBLIC SCHOOL BARA CHANDGANJ LUCKNOW` |
| `state_code`, `state` | UDISE state code + name | `09`, `UTTAR PRADESH` |
| `district_code`, `district` | UDISE district code + name | `0927`, `LUCKNOW` |
| `block_code`, `block` | UDISE block code + name | `092712`, `NAGAR KSHETRA ZONE-3` |
| `village_or_ward`, `cluster` | village/ward, cluster | `WARD 109`, `CHHAVNI MANIYAON` |
| `category`, `management`, `school_type`, `location` | school category / management / type / rural-urban | `Primary`, `Private Unaided (Recognized)`, `3-Co-educational`, `Urban` |
| `status` | Operational / Closed / Merged / etc. | `Operational` |
| `pincode` | postal code | `226018` |
| **`latitude`, `longitude`** | **GPS coordinates (decimal degrees); blank for non-operational schools** | **`26.87782`, `80.99698`** |
| `coord_source` | `kys_by_year` where a coordinate is present | `kys_by_year` |
| `coord_pulled_at` | when the coordinate was fetched (UTC) | `2026-09-03T17:07:31+00:00` |
| `schoolId` | KYS internal id (the key used to fetch coordinates) | `2189009` |
| `classFrm`, `classTo` | lowest / highest class taught | `1`, `5` |
| `address` | street address | `521/50 3rS Public School Bada Chandganj Lucknow` |
| `email` | contact email (source obfuscates `@`/`.`) | `3rspublicschool.lko[at]gmail[dot]com` |
| `lgdurbanlocalbodyName`, `lgdwardName` | local-government urban body / ward | `Lucknow-Municipal Corporations`, `Lucknow (M Corp.) - Ward No.2` |
| `sessionYear`, `yearId` | academic session of the record | `2026-27`, `13` |
| `lastmodifiedTime` | when the source last touched the record | `2026-08-13 18:05:58` |
| … | the remaining columns are every other field the source returns: management/type/category ids (`schMgmtId=5`, `schCategoryId=1`, `schType=3`), the `isOperational2018To19`…`2022To23` history, `villageId`/`clusterId`, and the rest of the `lgd*` local-government ids. See `data/sample_1000.csv` for a full 1,000-row sample. |

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
