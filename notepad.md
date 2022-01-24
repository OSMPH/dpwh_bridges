---
description: notepad for the DPWH-RBI dataset mapping challenge
tags: gov.ph, challenge, draft    
---
==**THIS PAD IS A DRAFT.**==

# DPWH National Bridges Dataset 
Mapping DPWH bridges on national highways, from the DPWH Road & Bridges Inventory (RBI).

[toc]

## About
On 13th January 2022, the local OSM community's  [Freedom-of-Information request](https://www.foi.gov.ph/requests/aglzfmVmb2ktcGhyHgsSB0NvbnRlbnQiEURQV0gtMjM2NjM0MzM2MjA4DA) with the Department of Public Works & Highways' Road and Bridges Inventory (DPWH-RBI) dataset was granted explicit permission for use with OpenStreetMap.

![](https://i.imgur.com/6YDmBme.png)

> For posterity, the comms archive with DPWH is available from the challenge [repo](https://github.com/OSMPH/dpwh_bridges).

## What value does it add to OSM?
* **add/improve/validate 8,546 bridges** which are vital infrastructure for highway networks, including **load limit restrictions** (i.e. OSM's `max_weight`) for bridges.
* **add missing highway names**. dataset includes highway name (i.e. value for `nat_name`, or `name` tags, if missing)
* **improved search results**. bridge names are possible proxies for (unmapped) settlements or waterways in the vicinity of the bridge.
* **add route number** (i.e. OSM's `ref`, if missing) for the highway.

## Seek LoCo consensus on:

### Bridge names on Highways
* The current LoCo bridge naming convention is to add the `bridge:name` to the highway name, with exceptions for "historical landmarks"
* ==Proposal:== Amend LoCo convention on mapping bridges, to always retain the highway's name, and move all bridge-related values (including the bridge name) to the `man_made=bridge` element outline itself. Only use `bridge:name`, for bridges mapped without their outline.

  :::danger 
  Using both `bridge:name` and `man_made=bridge` + `name` will return duplicate search results of the same bridge.


### Processing values (e.g. normalizing DPWH name)

#### "Jct" in highway names
* DPWH = "Jct Tablang-Gabaldon Road"
* OpenStreetMap tagging
    * `nat_name` (and `name`, if absent) = "Tablang-Gabaldon Road", without "Jct"
    * "Jct" for junction, refer to branches of the major regional highway
* Why not use `official_name` for name values?  DPWH does not the have the mandate for naming highways. Local Governments have naming rights (which makes them official) for highway sections falling within their jurisdiction.

#### items in parenthesis -- drop them?
* Normalizing DPWH's "Daang Maharlika (LZ)", "Daang Maharlika (MN)", etc
    * `nat_name = Daang Maharlika`
    * `name:en = Maharlika Highway`
    * `name:tl = Daang Maharlika`

### Capture official DPWH road class using `designation` key
* Already conventional.
* Values from DPWH `national_primary_road`, `national_secondary_road`, `national_tertiary_road`
* Potential other values: `provincial_road`, or `provincial_*_road`, or  `municipal_road` or `municipal_*_road` where '*' represent 'primary', 'secondary', or 'tertiary', if known

## Known issues
* `max_weight` values are not consistently available. Check street-level imagery (e.g. Mapillary, KartaView, or other OSM-compatible sources) for readable values
* Ortographic/Typographic errors on names. Names (of bridge, highway) don't exactly meet expected ortography (i.e. painted on bridges), e.g. Sucat Bridge vs Sukat Bridge.
* inconsistency of extrapolated bridge `lanes` values when used to set `lanes` for connecte highway. For example, a bridge is built for 4 lanes, but highway only uses 2.
* 

## Dataset fields

| Fields | OSM | tag |
|---|:---:|---|
| coordinates | ✗ |  |
| ISLAND |  |  |
| REGION |  |  |
| PROVINCE |  |  |
| DEO | 🗸 | `operator` |
| CONG_DIST |  |  |
| ROAD_NAME | 🗸 | highway's `nat_name`, and/or `name` |
| LOCATION |  |  |
| BRIDGE_ID | 🗸 | `ref` |
| BR_NAME | 🗸 | name (on `man_made=bridge`) |
| BR_LENGTH | 🗸 | `length` |
| BR_TYPE1 | 🗸 | `material` |
| BR_TYPE2 |  |  |
| YR_CONST |  |  |
| ACTUAL_YR | 🗸 | `start_date` |
| CONDITION |  |  |
| NUM_ABUTT |  |  |
| NUM_PIER |  |  |
| NUM_SPAN |  |  |
| BR_WIDTH | 🗸 | `width` |
| BR_LIFE |  |  |
| LOAD_LIMIT | 🗸 | `max_weight` |
| HT_OVER | ? | `max_height:physical` ? |
| HT_UNDER |  |  |
| L_SDWALK | ? | infer for `sidewalk:both` ? |
| R_SDWALK | ? | infer for `sidewalk:both` ? |
| NUM_LANES | 🗸 | `lanes` |
| BNR |  |  |
| CROSSING |  |  |
| BRGY | 🗸 | append to: `is_in` |
| MUN | 🗸 | append to: `is_in` |
| REMARKS |  |  |
| created_user |  |  |
| created_date |  |  |
| last_edited_user |  |  |
| last_edited_date |  |  |
| ROAD_SEC_CLASS | 🗸 | infer for `designation` |
| SEC_ID |  |  |
| SEC_LENGTH |  |  |
| ROUTE_NO | 🗸 | `ref` |
| MaxBRHT | ? | `max_height:physical` ? |
| MaxPierHT |  |  |
| COMMENTS |  |  |

## General questions

0. Is it possible to extrapolate an appropriate value for `bridge:structure` ~~and `bridge:support`~~ based on the fields: NUM_ABUTT, NUM_PIER, NUM_SPAN ?
0. Is it acceptable to combine `is_in` values from BRGY + MUN ?
0. Is there a more suitable option than MapRoulette for hosting this challenge?
0. To complete sub-national bridges, do we have region- or province-level LGU contacts to request a similar dataset for their geographic area?
0. Is there any risk of data quality degradation of current OSM data? If so, how can they be mitigated?