---
description: notepad for the DPWH-RBI dataset mapping challenge
tags: gov.ph, challenge, draft    
---

==**THIS IS A DRAFT. New content will be moved to the [OSM-PH repo](https://github.com/OSMPH/dpwh_bridges), because of new hackmd limitations. Final content will be moved to OSM wiki on project commencement.**==

# DPWH National Bridges Dataset 
Mapping DPWH bridges on national highways, and conflating attributes and other inferrable data from the [DPWH Road & Bridges Inventory (RBI)](https://www.dpwh.gov.ph/DPWH/gis/rbi).
![](https://i.imgur.com/K4Wi3Qh.png)

---

[toc]

---

## About
On 13th January 2022, a [Freedom-of-Information request](https://www.foi.gov.ph/requests/aglzfmVmb2ktcGhyHgsSB0NvbnRlbnQiEURQV0gtMjM2NjM0MzM2MjA4DA) with the Department of Public Works & Highways' [Road & Bridges Inventory (RBI)](https://www.dpwh.gov.ph/DPWH/gis/rbi) dataset was granted explicit permission for use with OpenStreetMap.

![](https://i.imgur.com/6YDmBme.png)

> For future reference, the communications with the DPWH is archived and  available from the public [repo](https://github.com/OSMPH/dpwh_bridges).

## What potential value does it add to OSM?
* improve highway networks and **add/review 8,546 bridges**, including adding **load limit restrictions** (i.e. OSM's `max_weight`) for the highway.
* **add/supplement highway names**. dataset includes highway name (i.e. value for `nat_name`, or `name` tags, if missing)
* **improved search results**. bridges are local landmarks, and serve as practical proxies for (unmapped) settlements or waterways in the vicinity of the bridge.
* **add route number** (i.e. OSM's `ref`, if missing) for the highway.
* **address QA issues**, in the course of editing OSM.

# Dataset fields

| Fields | Usable? | man_made=bridge object | highway=* object | notes |
|---|:---:|---|---|---|
| coordinates | ✔ |  |  | Will be used to display a map marker for a task, to visualize the location of a bridge, or starting point for a vicinity search. |
| ISLAND |  |  |  |  |
| REGION |  |  |  |  |
| PROVINCE |  |  |  |  |
| DEO | ✔ | for `operator` key |  |  |
| CONG_DIST |  |  |  |  |
| ROAD_NAME | ✔ |  | for `nat_name`, and/or `name` key | Proposal: use `nat_name` key for the RBI highway name, and to `name` if it's empty. |
| LOCATION |  |  |  |  |
| BRIDGE_ID | ✔ | for `ref` key |  |  |
| BR_NAME | ✔ | for `name` key |  |  |
| BR_LENGTH | ✔ | for `length` key |  |  |
| BR_TYPE1 | ✔ | for: `material` (“reinforced concrete”,“steel”); infer `bridge:structure`   |  | DPWH "concrete": OSM "reinforced_concrete"; DPWH "metal" : OSM "steel" |
| BR_TYPE2 |  |  |  |  |
| YR_CONST |  |  |  |  |
| ACTUAL_YR | ✔ | for `start_date` key |  |  |
| CONDITION |  |  |  |  |
| NUM_ABUTT |  |  |  |  |
| NUM_PIER |  |  |  |  |
| NUM_SPAN |  |  |  |  |
| BR_WIDTH | ✔ | for `width` key |  |  |
| BR_LIFE |  |  |  |  |
| LOAD_LIMIT | ✔ | for `max_weight` key |  | Maximum physical load limit. |
| HT_OVER | ? |  |  | max_height:physical ? |
| HT_UNDER | ? |  |  |  |
| L_SDWALK | ? |  | infer for `sidewalk:both` ? |  |
| R_SDWALK | ? |  | infer for `sidewalk:both` ? |  |
| NUM_LANES | ? |  | maybe used for `lanes` key | Warning: OSM may already have better data |
| BNR |  |  |  |  |
| CROSSING |  |  |  | use info to describe the task, assist for hard-to-see mapper |
| BRGY | ✔ | combine with MUN and add to: `is_in` |  |  |
| MUN | ✔ | combine with BRGY and add to: `is_in` |  |  |
| REMARKS | ✕ |  |  |  |
| created_user | ✕ |  |  |  |
| created_date | ✕ |  |  |  |
| last_edited_user | ✕ |  |  |  |
| last_edited_date | ✕ |  |  |  |
| ROAD_SEC_CLASS | ✔ | infer for `designation` key |  |  |
| SEC_ID | ✕ |  |  |  |
| SEC_LENGTH | ✕ |  |  |  |
| ROUTE_NO | ✔ | for `ref` key |  |  |
| MaxBRHT | ? |  |  | max_height:physical ? |
| MaxPierHT |  |  |  |  |
| COMMENTS | ✕ |  |  |  |
| n/a |  | `source=DPWH-RBI` | `source=DPWH-RBI` | add or append "DPWH-RBI" as source |

# Seek LoCo consensus on:

## Bridge names on Highways
* The current LoCo bridge naming convention is to add the `bridge:name` to the highway name, with exceptions for "historical landmarks"
* ==Proposal:== Amend LoCo convention on mapping bridges, to always retain the highway's name, and move all bridge-related values (including the bridge name) to the `man_made=bridge` element outline itself. Use `bridge:name=*` if there's no man_made=bridge.

  :::danger 
  Using both `bridge:name` and `man_made=bridge` + `name` will return duplicate search results of the same bridge.

## Pre-processing string values 

### "Jct" in highway names
* ==Further clarify how "JCT" is used in DPWH highway names==  
* DPWH = "Jct Tablang-Gabaldon Road"  
* OpenStreetMap tagging
    * `nat_name` (and `name`, if absent) = "Tablang-Gabaldon Road", without "Jct"
    * "Jct" for junction, refer to branches of the major regional highway
    
* Why not use `official_name` for name values?  DPWH does not the have the mandate for naming highways. Local Governments have naming rights (which makes them official) for highway sections falling within their jurisdiction.

### "City" strings in highway names
* It's been observed that some contributors drop the "City" in highway names (e.g. "Ozamis-Oroquieta Road")
    * should we then use `nat_name=Ozamis City-Oroquieta City Road` since this is how the dataset calls the highway, and store the normalized string as `name=Ozamis-Oroquieta Road` ?

### items in parenthesis ?
* Normalizing DPWH's "Daang Maharlika (LZ)", "Daang Maharlika (MN)", etc
    * `nat_name = Daang Maharlika`
    * `name:en = Maharlika Highway`
    * `name:tl = Daang Maharlika`
    * `official_name = Daang Maharlika Highway`
    * `old_name= Pan-Philippine Highway`
* sometimes the parenthesis containers other information, (i.e. bridge structure) or some descriptive text
    * example: "Amper Bridge (truss)"
    * properly tagged as
        * `name=Amper Bridge`
        * `bridge:structure=truss`

### Spell out abbreviations ("Sn.", "Sta.", etc.)
Following OSM conventions, [abbreviations](https://osm.org/wiki/Name_finder:Abbreviations#Philippines) will be spelled out:
  * "Sn." - San  
  * "Sta." - Santa  
  * "Sto." - Santo  
  * "Gen." - General  
  * "Gov." - Governor  
  * ...  

### Roman numerals in bridge names

Example: "Santo Borys Bridge V"

Possible object name in OSM:
* "Santo Borys Bridge Ⅴ" (the correct unicode character)
* "Santo Borys Bridge № 5" (search friendly)
* "Santo Borys Bridge 5th" (search friendly)
* "Santo Borys Bridge V" (sic, the letter "V")
> **Recommendation**: practical to display/search using name with arabic numbers, then use name with roman numeral in alt_name. Example:
> `man_made=bridge` + `name=Santo Borys Bridge 5th` + `alt_name=Santo Borys Bridge Ⅴ`

## Capture official DPWH road class using `designation` key
* Already conventional.
* Values from DPWH `national_primary_road`, `national_secondary_road`, `national_tertiary_road`
* Potential other values: `provincial_road`, or `provincial_*_road`, or  `municipal_road` or `municipal_*_road` where '*' represent 'primary', 'secondary', or 'tertiary', if known or appropriate

## Check for possible dupes
Noted by Maning (i.e. Nangka Bridge between Marikina and San Mateo, IDs: `B02865LZ`, `B01341LZ`)

## Bridge span arrangement in bridge name, enclosed in parenthesis
Occassionally, a record may specify the bridge type in the name field, in a parenthesis (e.g. bridge B01118MN, "Medina Bridge(Cantilever)").

Tag as: `name=Medina Bridge` + `bridge=cantilever`

## Indistinct / Can't See / Visually unverifiable bridges
Some bridge locations are indistinct on aerial imagery. Switching aerial imagery might be helpful but it could also mean the bridge is elsewhere (anywhere between several hundred meter, to a few kilometers), or the location is incorrect.

**Recommended action**: Check the surrounding area for visual clues. OSM-compatible imagery (i.e., Mapillary, KartaView, WikiMedia, etc.) in the vicinity may be helpful, too.  Skip, if no identifiable bridge is found.

Example: Bridge B01144MN is reported by DPWH at 8.2984079, 123.8463154 but is actually found at 8.3015902, 123.8466707, about 350m away.

Sometimes, the bridge name already be in OSM as `bridge:name`. Move all bridge-related tags `man_made=bridge`.
> Use CROSSING to describe the bridge, but don't add to OSM.

# Other known issues
* `max_weight` values are not consistently available. Check street-level imagery (e.g. Mapillary, KartaView, or other OSM-compatible sources) for readable values
* Ortographic/Typographic errors on names. Names (of bridge, highway) don't exactly meet expected ortography (i.e. painted on bridges), e.g. Sucat Bridge vs Sukat Bridge.
* inconsistency of extrapolated bridge `lanes` values when used to set `lanes` for connected highway. For example, a bridge is built for 4 lanes, but highway only uses 2.
* 


# General questions

0. Is the permission letter from DPWH sufficient in form and substance to make the dataset compatible with OSM?
0. Confirm: it's possible to infer an appropriate value for `bridge:structure` ~~and `bridge:support`~~ based on the fields: ~~NUM_ABUTT, NUM_PIER, NUM_SPAN~~ BR_TYPE1.
0. According to the Wiki, it's acceptable to [combine place values](https://osm.org/wiki/Key:is_in#Tag_.22is_in.22) in `is_in`. In this challenge, is it okay with the LoCo for us to use (cleaned-up) BRGY + MUN values then?
0. Is there a more suitable option than MapRoulette for hosting this challenge?
0. To complete sub-national bridges, do we have region- or province-level LGU contacts to request a similar dataset for their geographic area?
0. Is there any risk of data quality degradation of current OSM data? If so, how can they be mitigated?

### Other questions/feedback
* For a road name like "Mindoro Oriental/Occidental East Coastal Rd” is this two road names?  I.e. Mindoro Oriental Road & Occidental East Coastal Road?
  > It looks like a road named after two provinces, probably close to their boundaries. Will look into this closely. [name=erwin]
* Looks like there may be some proposed expressways in the dataset.  We’ll want to be careful how that information is added or if its added quite yet.
  > I didn't see this in the dataset. Will check again [name=erwin]
* Also, if bridge name is added, we’ll want to make sure it’s going in the unique bridge name tag.  Its not a super common tag so I'm not sure if you are aware of it or not.  (https://wiki.openstreetmap.org/wiki/Key:bridge#Naming)
  > That wiki page/section doesn't mention any requirement for bridge names having to be "unique", or did I misunderstand the q? [name=erwin]

---

# LoCo Consultation/Meetings
## 2022-02-02
- Vince: This mapping project seems to require a lot of work. 
    > We should have a demo/workflow of how to map and tag the objects to be affected by this. [name=erwin]
- Timmy: Apart from osm.org, what other data consumers support man_made=bridge? 
    - [x] start page to document apps/data consumers that support man_made=bridge. 
        > during meeting, we quickly verified that Osmand and Organic Maps supports rendering and search for objects tagged with `man_made=bridge`. More tests [here](https://github.com/OSMPH/dpwh_bridges/wiki/Questions-and-Answers#osm-data-consumer-support-for-man_madebridge). [name=erwin]
- on changing bridge mapping convention and exceptions
    - Rally: historical bridge exception. keep a list, could be reviewed again after mapping effort
    - Timmy: where bridge connects highways with <u>different names</u>, the highway name should take the bridge name
    - Rally: the name on man_made=bridge outline can take the DPWH name, but name variants (e.g., loc_name, reg_name, etc) can still be used in the future (i.e. renamed by local authorities)
    - What if current bridge name doesn't match/disimilar to DPWH name?
        > May be prefixed with `was:` (e.g. `was:name=Tulay XYZ`) lifecycyle prefix for easier review in the future, and/or explain with annotation on object? [name=erwin]
- RBI lane count is probably outdated versus what's already OSM
    - Rally: is there AI to determine whether number of indicated lanes are feasible based on road width?
- On object names:
    - maybe nat_name should be as-is from DPWH, and the common name will be clean strings?
        > I don't think we should keep the DPWH string names "as-is", because they won't comform to accepted OSM conventions for naming objects (e.g. abbreviations, known typos, unnecessary technical text etc.). The likelihood of DPWH using this back in their system is very unlikely. [name=erwin]
        > Also, if they wish to conflate any name string cleaning we might do, there are many easier ways to do that with various GIS/scripting tools [name=erwin]
    - To minimize potential data degradation, `nat_name` is being recommended, so mappers don't inadvertently overwrite existing `name` tags added by local mappers. The value  will only be *copied* **IF** `name` key is empty.
    - *Example.* **DPWH: "Dumaguete North Rd (Jct Bais-Kabankalan-Negros Occ Bdry)"**
        - name=Dumaguete North Road
        - nat_name?
            - [ ] Jct Bais-Kabankalan-Negros Occ Bdry
            - [ ] Junction Bais-Kabankalan-Negros Occidental Boundary
            - [ ] Junction Bais-Kabankalan-Negros Occidental Boundary Road
            - [ ] Dumaguete North Road (Junction Bais-Kabankalan-Negros Occidental Boundary)
        - reg_name?
            - [ ] Bais-Kabankalan-Negros Occidental Boundary Road?
    - *Example.* **DPWH: "Ozamis City-Oroquieta City Road"**
        - `name` ?
            - [ ] Ozamis-Oroquieta Road
        - `nat_name` ?
            - [ ] Ozamis City-Oroquieta City Road
        - Timmy: we should not duplicate the values for name keys (e.g. name has the same value as nat_name, official_name, etc).
        - Erwin: Redundant information doesn't improve  value to OSM. Why should `nat_name` be kept as-is to directly correspond with RBI dataset? 
    - ==**This topic should be brought to, and widely discussed with, the OSM community**==
- Inferring `bridge:structure` per Timmy:
    - if BR_TYPE1=concrete ∴ `bridge:structure=beam`
    - if BR_TYPE1=Steel ∴ `bridge:structure=truss`
    - Is there a civil engineer we can ask to confirm this?
- Using `is_in` on bridges
    - Rally: what if bridge is located with border disputes, or between admin boundaries?
        > Boundaries are mostly missing in OSM, and we don't have OSM-compatible boundary layers to determine this. The is_in tag can support multiple place names, so it's possible for disputing LGUs  add their respective place names while dispute is unresolved. Example. Bgy X and Bgy Y with boundary dispute. Bridge may have: `is_in=Barangay X;Barangay Y;Municipality` [name=erwin]