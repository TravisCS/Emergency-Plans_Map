# Los Angeles County Emergency Plans Map

Double-click `index.html` to open the map (no server needed). Keep `data.js`, `boundaries.js` and `leaflet-1.9.4.js` beside it; `data.json` and `boundaries.json` are the same data for other tools.

- One node per organization (505), sized by its document count and colored by folder level. Click a node for its documents: official link, Internet Archive copy where the official link is dead, the local PDF when opened from this folder, the link-audit result and the content-check result.
- **Find my plans**: type an address (needs internet; uses OpenStreetMap Nominatim), click the map, use your location, or pick a city. The page finds the city or unincorporated area containing the point and lists every document in the library that covers it, grouped from the city up to federal law.
- **About, sources and how to cite** on the title card: method, link audit, node placement, sources in APA form, limitations and a citation to copy.
- `metadata.md` is the metadata record (sources, lineage, AI use, limitations, use constraints).

Rebuild after the library changes: `python3 -I _tools/build_map_data.py` (reads `_tools/results.json` and `_tools/map_geocodes.json`). Boundaries: `python3 -I _tools/simplify_boundaries.py <county GeoJSON> 00_Start_Here/Map/boundaries.js`.
