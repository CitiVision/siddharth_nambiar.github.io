# Report: Re-engineering "Map to Nap" as a Python workflow for tenant plot-context maps

## 1. What Map to Nap does (the pattern to replicate)

Map to Nap-style products turn a location into a single, glanceable "context map":

1. **Pick a place** (address / plot / neighbourhood) and a radius or catchment.
2. **Pull open geodata** for that area: street network, buildings, green space, water, transit stops, amenities (cafes, schools, shops), and "nuisance" layers (noise, busy roads, nightlife, rail).
3. **Compute context metrics**: walk-time isochrones, distance to nearest amenity of each type, green coverage, noise exposure, etc.
4. **Render a stylised poster-like map** with a restrained palette, clear hierarchy and a legend/score panel, so a non-expert can read the neighbourhood quality at a glance.
5. **Publish** as image / PDF / interactive web map.

The product value is not new data, it is **curation + cartographic design + simple scores**.

## 2. Recommended Python workflow

| Stage | Purpose | Libraries |
|---|---|---|
| 1. Geocoding | Address -> point/polygon | `geopy` (Nominatim), `osmnx.geocode` |
| 2. Data acquisition | Streets, buildings, POIs, parks, water | `osmnx`, `overturemaps` (buildings/places), `pyrosm` (PBF extracts), city open-data portals via `requests` / `geopandas.read_file` |
| 3. Transit | Stops, frequencies | GTFS via `gtfs-kit` / `partridge`, `r5py` or `pandana` for accessibility |
| 4. Environment | Noise, air, tree cover, heat | EU noise maps / local WMS via `owslib`, `rasterio`, `rioxarray`, Copernicus/Sentinel via `pystac-client` + `stackstac` |
| 5. Analysis | Isochrones, nearest amenity, scores | `geopandas`, `shapely`, `networkx`, `osmnx.routing`, `pandana`, `momepy` (urban morphology) |
| 6. Scoring | 15-minute-city style index | `numpy`/`pandas`, min-max or percentile normalisation, weights in a YAML config |
| 7. Static rendering | Poster maps | `matplotlib` + `geopandas.plot`, `contextily` (basemaps), `prettymaps` (artistic OSM maps), `cartopy`, `adjustText`, `matplotlib-scalebar` |
| 8. Interactive rendering | Web maps | `folium`, `lonboard`/`pydeck` (deck.gl), `keplergl`, `leafmap` |
| 9. Layout/export | PDF / social cards | `matplotlib` figure layout, `reportlab` or `weasyprint`, `jinja2` HTML templates |
| 10. Orchestration | Repeatable per plot | `prefect` or `dagster`, `typer` CLI, `pydantic` config, `joblib`/`diskcache` caching, `duckdb` + GeoParquet for storage |

### Pipeline sketch

```
address -> geocode -> buffer(800 m) -> fetch layers (OSM/Overture/GTFS/noise)
        -> clean to common CRS (UTM) -> compute metrics & isochrones
        -> score -> render map + score panel -> export PNG/PDF/HTML
```

### Minimal code skeleton

```python
import osmnx as ox, geopandas as gpd, matplotlib.pyplot as plt

point = ox.geocode("Digbeth, Birmingham, UK")
G = ox.graph_from_point(point, dist=1000, network_type="walk")
edges = ox.graph_to_gdfs(G, nodes=False)
bldg  = ox.features_from_point(point, {"building": True}, dist=1000)
parks = ox.features_from_point(point, {"leisure": "park"}, dist=1000)
water = ox.features_from_point(point, {"natural": "water"}, dist=1000)

fig, ax = plt.subplots(figsize=(10, 14), facecolor="#f4efe6")
bldg.plot(ax=ax, color="#d9d0c1", linewidth=0)
parks.plot(ax=ax, color="#b7cfa4", linewidth=0)
water.plot(ax=ax, color="#9ec3d6", linewidth=0)
edges.plot(ax=ax, color="#3a3a3a", linewidth=0.4)
ax.set_axis_off()
fig.savefig("plot_context.png", dpi=300)
```

## 3. Matching the "CityID Birmingham / interconnect" look

Typical traits and how to reproduce them:

- **Flat, muted palette** with one accent colour for the subject plot (define a palette dict; use `matplotlib` colormaps or `palettable`).
- **Layered figure-ground**: buildings as solid fills, roads as thin dark lines, water/green as soft blocks (z-order control in geopandas plots).
- **Interconnect lines**: draw network links (walk routes/transit lines/desire lines from plot to amenities) with `osmnx.routing.shortest_path` and plot as weighted-width lines, or arcs via `pydeck` ArcLayer; use glow/halo with `matplotlib.patheffects`.
- **Typography**: a clean sans (e.g. Inter / DM Sans) loaded via `matplotlib.font_manager`; small-caps labels placed with `adjustText`.
- **Minimal labels, numbered callouts** tied to a side legend with walk minutes.
- **Isochrone rings** (5/10/15 min) as translucent polygons from `pandana`/`osmnx` + `shapely` unions.
- For more artistic output: `prettymaps`; for crisp vector/print: export SVG and finish in Figma/Illustrator.

## 4. Adding agentic intelligence

Keep the deterministic geo-pipeline as **tools**, and let an LLM agent orchestrate and explain.

| Agent capability | How |
|---|---|
| Understand the request ("quiet, near a park, 10 min to the tram") | LLM parses to a structured `pydantic` criteria object |
| Tool use | Expose `geocode`, `fetch_layers`, `compute_isochrone`, `score_plot`, `render_map` as functions (OpenAI/Anthropic tool calling, `pydantic-ai`, `langgraph`, or `smolagents`) |
| Data-gap handling | Agent chooses fallbacks (OSM vs. Overture vs. local portal) and flags missing layers |
| Narrative | LLM writes a tenant-friendly summary ("Quiet street; 4 min to Moor Street; busy road 120 m north") grounded **only** in computed metrics |
| Comparison | Rank several candidate plots, generate side-by-side maps and pros/cons |
| Critique loop | A vision model reviews the rendered PNG for label overlap/legibility and triggers a re-render |
| Guardrails | Numbers come from tools, never from the LLM; log provenance and data dates; add a fair-housing/neutrality check on language |
| Memory/caching | Store plot results in DuckDB/GeoParquet; reuse across sessions |

Suggested stack: `langgraph` (stateful workflow) + `pydantic-ai` (typed tools) + `streamlit` or `FastAPI` front end.

## 5. Phased plan

1. **MVP (1-2 wks):** address -> OSM layers -> static poster map with amenity distances (`osmnx`, `geopandas`, `matplotlib`).
2. **Style pass:** CityID-like palette, typography, interconnect lines, isochrones.
3. **Metrics:** walkability, transit access (GTFS), green space, noise; YAML-weighted score.
4. **Automation:** CLI + Prefect for batch plots, caching, PDF export.
5. **Agent layer:** NL criteria, tool-calling orchestration, grounded narrative, render critique loop.
6. **Web delivery:** interactive `lonboard`/`folium` map + downloadable PDF.

## 6. Risks and notes

- OSM completeness varies by city; validate against local open data.
- Respect licences: ODbL (OSM, attribution required), Overture (CDLA/ODbL), basemap tile terms.
- Overpass rate limits: cache and prefer PBF extracts for batch runs.
- Noise/crime-type layers can be sensitive; present with sources and caveats.
- I could not inspect Map to Nap's proprietary internals; the above is inferred from its public product behaviour.
