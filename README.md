# Lost Los Angeles (prototype)

An interactive 3D view of the Los Angeles Basin before widespread settlement. Every shape in `data/features.geojson` is a hypothesis with a stated confidence and sources, so it can be improved over time.

**Status:** the sample data is placeholder only. Nothing in it is researched.

## How it works

Terrain at year T = modern terrain (USGS 3DEP, via free AWS tiles) + every feature whose dates include T. There are no per-era terrain files.

## Deploy on GitHub Pages

1. Create a GitHub account and a new **public** repository (for example `lost-la`).
2. Click **Add file > Upload files** and drag in `index.html`, `README.md` and the `data` folder. Click **Commit changes**.
3. Go to **Settings > Pages**. Under "Build and deployment", set Source to **Deploy from a branch**, Branch to **main**, folder **/ (root)**, and Save.
4. Wait a minute or two. The site address appears at the top of the Pages screen.

The viewer must be opened from that web address. Double-clicking `index.html` on your computer won't load the data file.

## Data schema

`data/features.geojson` is a list of features. Each has a shape (geometry) and these properties. Keep the names exactly as written.

| Property | Required | Meaning |
|---|---|---|
| `feature_id` | yes | Short unique name, lowercase with hyphens. Versions of the same thing (a river before and after it moved) share one id. |
| `name` | yes | Human-readable name. |
| `type` | yes | `wetland`, `woodland` or `river` (more types later). |
| `valid_from`, `valid_to` | yes | First and last year the shape applies. Use `null` for `valid_to` if still true today. |
| `confidence` | yes | `documented` (directly shown on a source), `inferred` (reasoned from evidence), or `speculative` (a guess). Shown on screen as solid versus dashed outlines and lighter fill. |
| `terrain_edit_m` | no | Polygons only. Flattens the ground in the shape to this elevation in meters above sea level during its dates. `null` for no change. |
| `vegetation` | no | `tree` or `reed`. The viewer scatters plants inside the polygon. |
| `density_per_ha` | no | Plants per hectare. Capped at 5000 per shape for now. |
| `sources` | yes | Citation(s): author, year, title, map or page. |
| `notes` | no | Reasoning, caveats. |

Shapes: wetlands and woodlands are `Polygon`, rivers are `LineString`. Coordinates are `[longitude, latitude]`, in that order.

## Adding or editing a feature

1. Go to https://geojson.io and draw your shape on the map.
2. In the table on the right, add the properties above.
3. Copy the resulting text and add it to `features.geojson` inside the `features` list (watch the commas), or send it to the maintainer.

## Eras

The slider snaps between eras listed at the top of the script in `index.html` (`ERAS`). Add or change years there.

## Known limits

- Terrain edits create a hard step at a polygon's edge. Smoother blending comes later.
- Plants are simple placeholder shapes (cones and thin cylinders). Real tree and rush models can replace them later.
- Terrain is about 16 m per pixel, so it looks smooth up close.
- The ground is colored by elevation, not imagery.
- Modern terrain includes freeways, fill and grading. Features that need correction should use `terrain_edit_m`.
