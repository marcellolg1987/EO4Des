# EO4DES WebGIS Demo – GitHub Pages

This package is a static version of the EO4DES Cesium WebGIS. It does not require GeoServer or PostGIS.

## Files to add in `data/`

Export the current server-side layers and add them with exactly these names:

- `regioni.geojson` — Italian regional boundaries (optional but recommended)
- `statistiche_zonali_lecce.geojson` — zonal-statistics polygons including the `fid` attribute
- `thermal_anomaly_lecce_14082024.png` — rendered thermal-anomaly raster
- `lst_lecce_14082025.png` — rendered LST raster

## Important: raster geographic extent

Open `index.html` and update:

```js
const DEMO_EXTENT = {
  west: ...,
  south: ...,
  east: ...,
  north: ...
};
```

Use the exact bounds of the exported raster in EPSG:4326.

You can obtain them from QGIS layer properties or with:

```bash
gdalinfo raster.tif
```

If your two rasters have different extents, create one extent object per raster and use the corresponding one in `addStaticRaster()`.

## Export recommendations

### Zonal statistics
In QGIS:
1. Right-click the layer.
2. Export > Save Features As...
3. Format: GeoJSON.
4. CRS: EPSG:4326.
5. Keep the `fid` and the statistical attributes that must appear in the InfoBox.

### Raster layers
The PNG must already contain the desired cartographic styling, because GitHub Pages does not run GeoServer SLD rendering.

A practical workflow is:
1. Apply the final style in QGIS.
2. Export the rendered layer/image to PNG.
3. Record its EPSG:4326 extent.
4. Put the PNG in `data/`.

## GitHub Pages

Upload the whole folder to the repository root, then enable:

Settings > Pages > Deploy from a branch > `main` / root.

The demo uses:
- CesiumJS 1.89 from the public Cesium CDN.
- OpenStreetMap tiles as the base map.
- Static GeoJSON for clickable polygons.
- Static PNG overlays for raster thematic layers.

The zonal-statistics click is fully client-side:
gray polygon -> click -> orange polygon -> Cesium InfoBox.

## Notes

This is intentionally a public demonstrator. The full EO4DES architecture may continue to use GeoServer/PostGIS in the research environment.
