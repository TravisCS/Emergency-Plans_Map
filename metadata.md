# Metadata: Los Angeles County Emergency Plans Map

**Title.** Los Angeles County emergency plans: who published what, and which plans cover an address.

**Abstract.** An interactive web map of the Los Angeles County emergency plans library: one node for each of the 501 organizations that issued one or more of the library's 648 documents (emergency operations plans, hazard mitigation plans, safety elements, annexes, after-action reports, hazard maps, mutual-aid agreements, frameworks and law), with each document's official link, Internet Archive copy where the official link is dead, link-audit result and content-check result. A lookup lists every document in the library whose jurisdiction covers a chosen address or map point.

**Purpose.** To let a reader see at a glance which jurisdictions in the county have plans on file, open any plan from its publisher's own site, and find out which plans apply to a given address.

**Author and contact.** Travis Cody Smith. Code and layout generated with Claude (Anthropic); data, placement and accuracy reviewed by the author.

**Created.** 2026-10-30. **Last update.** 2026-10-30 (see the "Updated" line on the title card; the date comes from the data file).

**Spatial extent.** Nodes: the organizations' headquarters, from Los Angeles County (33.70 N to 34.85 N, 118.95 W to 117.60 W) out to Sacramento, Mather, Reston (Virginia), Washington (DC), Columbia (Maryland) and Fort Worth (Texas) for state and federal agencies and one railroad. City boundary layer: Los Angeles County.

**Coordinate system.** WGS 84 (EPSG:4326) in the data files; drawn in Web Mercator (EPSG:3857) by Leaflet.

## Sources

1. Smith, T. C. (2026). Los Angeles County emergency plans library, `_tools/results.json` (648 records, built 2026-10-28 to 2026-10-30 from the issuing organizations' official websites). Each record's own APA reference is in `00_Start_Here/LA_Emergency_Plans_References_APA_Master.pdf`.
2. County of Los Angeles, Internal Services Department, Enterprise GIS (2026). City Boundaries, layer 19 of the Political Boundaries map service (Los Angeles County Registrar-Recorder data). https://public.gis.lacounty.gov/public/rest/services/LACounty_Dynamic/Political_Boundaries/MapServer/19 . Retrieved 2026-10-30 as GeoJSON in EPSG:4326 (345 polygons: 154 city pieces for 88 cities, 191 unincorporated pieces). The service publishes no dataset date.
3. U.S. Geological Survey (n.d.). The National Map: USGS Topo and USGS Imagery Topo tile services. https://basemap.nationalmap.gov/arcgis/rest/services . Public domain.
4. OpenStreetMap contributors (2026). Nominatim geocoding service, used for the address search only when the page can reach the internet. https://nominatim.openstreetmap.org/ . Data (c) OpenStreetMap contributors, ODbL.
5. Agafonkin, V. (2023). Leaflet 1.9.4 [JavaScript library, BSD-2-Clause]. https://leafletjs.com/
6. Organization positions: `_tools/map_geocodes.json`, 505 points placed on 2026-10-30 by two Claude research agents at each organization's headquarters, city hall or site, from the organization's name and documents, each with a precision tag (hq, site, city) and a confidence tag (high, medium). Nine organizations added after the agents ran were placed by hand the same way.

## Processing steps (lineage)

1. `_tools/build_map_data.py` reads `results.json` and `map_geocodes.json`, derives the organization from each file name (the prefix before the type code and year), attaches every document to its organization, and writes `data.js` and `data.json` (counts, levels, colors, regional membership table, 501 organizations, 648 documents).
2. `_tools/simplify_boundaries.py` reads the county GeoJSON and writes `boundaries.js` and `boundaries.json`: a Douglas-Peucker pass with a 0.00025 degree tolerance (about 25 meters), rings under five points dropped, coordinates rounded to five decimals (13.3 MB to 0.38 MB, 224 of 345 pieces kept; the dropped pieces are slivers smaller than the tolerance).
3. The page groups organizations that share a position rounded to three decimals (about 100 m) into one dashed node that lists them.
4. The lookup does point-in-polygon (even-odd ray casting, holes honored) against the boundary layer in the browser, then assembles the groups described under "Find my plans" in the page's About dialog. Council-of-governments membership is a table in `build_map_data.py` (Las Virgenes-Malibu COG, San Gabriel Valley COG, South Bay Cities COG, Palos Verdes Peninsula cities, Cal OES Region I and MARAC for every city), taken from the councils' membership lists in 2026.
5. Link audit: `_tools/audit.py` requests the first bytes of every official link (byte-range GET) and records OK, BLOCKED, VIEWER, PORTAL, NOT_PDF, DEAD or an error code; links whose sites refuse scripts were opened again in a signed-in Chrome session and the result stored as `audit_browser`. The map shows both.
6. Content check: the first pages of every downloaded PDF were read and compared with the catalogue entry (`verdict`: match, partial, unreadable); the printed title, author and year are what the map shows.
7. Node audit (2026-10-30): a script checked every point against the city boundaries, then eight Claude reviewer agents (Sonnet) checked each organization's location against its real office (OpenStreetMap Nominatim and official pages) and each document's ownership, year and title. Every proposed new address was geocoded again as a second check. 152 fixes were proposed and the author answered each one; the accepted ones moved 164 points, reassigned 9 documents (adding EPA Region 9, the California Air Resources Board and National VOAD, and removing 7 organizations whose only document belonged to someone else), and corrected or annotated 69 other documents. Document fixes live in `_tools/map_fixes.json` and change only the map; the library files and workbooks keep their original names. Every question, its evidence and its answer: https://traviscs.github.io/Emergency-Plans_Map-150-Question-Audit/

## AI use

Research agents (Claude, Anthropic) found the document links and checked the files; two Claude agents placed the organization points; the page code, data pipeline and this metadata were generated with Claude. The author chose the sources, directed the collection, reviewed the results and is responsible for the content.

## Limitations

- Node positions mark the issuing organization, not the area a plan covers. About one organization in eight is placed with medium confidence (a city hall or office located from memory) and may be a few hundred meters off.
- Special district and school district service areas are not mapped; the lookup lists them for reference or by distance.
- Unincorporated land is one class; community names come from the documents.
- The library reflects documents public in October 2026. Twenty documents could not be downloaded by script and are listed with their official link only.
- The address search needs internet access to OpenStreetMap Nominatim; inside a claude.ai artifact outside requests are blocked, so the page offers map clicks and a city picker instead.
- Local-PDF links work only when the page is opened from the library folder on the Mac that built it.

## Use constraints and attribution

**Not for official use.** This is an independent map, not affiliated with or endorsed by Los Angeles County, the City of Los Angeles, Cal OES, FEMA or any other agency shown. Linked documents may have changed since the build date; use each agency's own website for official decisions and current plans. In an emergency, call 911 and follow local officials.

The page and data files may be shared. Keep the attribution on the map: "Map by Travis Cody Smith. Boundaries: Los Angeles County eGIS. Basemap: USGS The National Map." Address search results are (c) OpenStreetMap contributors (ODbL). The documents linked belong to their publishers.

## Citation

Smith, T. C. (2026). *Los Angeles County emergency plans: Who published what, and which plans cover an address* [Interactive map]. Los Angeles County Emergency Plans library, 00_Start_Here/Map/index.html

Figure note: Data from the organizations listed under Sources (2000 to 2026) and the Los Angeles County eGIS city boundary layer. Basemap USGS The National Map. Built with Leaflet; code generated with Claude and reviewed by the author.
