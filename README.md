# MaticTechMapper

A local Windows desktop photogrammetry application. It turns overlapping drone photographs (such as DJI JPGs) into a georeferenced dense point cloud, textured 3D mesh, orthomosaic and DSM, and provides tools to inspect, measure and export the results.

## Tested hardware

MaticTechMapper has been tested **only on an Alienware 15 laptop from 2015**. Other hardware has not been tested.

| Component | Specification |
| --- | --- |
| CPU | Intel Core i7-4720HQ @ 2.60 GHz (4 cores, 8 logical processors) |
| Memory | 16 GB DDR3, 1600 MHz |
| GPU | NVIDIA GeForce GTX 970M, 3 GB dedicated memory |
| Integrated graphics | Intel HD Graphics 4600 |
| Storage | SSD and HDD (both RAID) |

> **Development preview.** Output accuracy is not independently certified. Verify results against your own survey controls before relying on them for production work.

## Getting started

### Install and run

1. Run the setup file, `MaticTechMapper-0.114.0-preview-windows-x64-setup.exe`, and follow the installer.
2. Start MaticTechMapper from the Start menu or desktop shortcut.

The installer includes the application and its bundled processing runtime, so no Python installation is required. Updating works the same way: close Mapper and run the newer setup file. Existing surveys are kept.

Opening Mapper starts its local service automatically, and the service listens only on the loopback address.

If Microsoft Edge WebView2 or .NET is missing, a **System requirements** window opens first. Click **Install** to download the official Microsoft installer (internet access is needed, and Windows may ask for administrator approval or a reboot), then **Recheck** and **Open mapper**. The window is also available from the app header or with `MaticTechMapper.exe --dependencies`. An optional NVIDIA driver link is provided separately.

### Map DJI photographs

1. Create a dataset.
2. Drop the original DJI JPG photographs into the image-import step and wait for the import to finish.
3. Click **Process DJI map**.

This selects High processing, the UTM zone from the photo GPS, EGM2008 output heights, and automatic CPU, RAM and GPU detection. Photo GPS weighting assumes standard deviations of 5, 5 and 10 metres for X, Y and Z; this is an assumption, not a measure of survey accuracy. Marked ground controls remain available.

Keep the app open while processing. The processing page shows the active stage, and **View results** appears when the run completes. Before reporting completion, Mapper decodes the linked texture images and reads every map band, so empty or unreadable products produce a failed status with the generated files kept. A failed run retains its logs and checkpoints, and you can use **Resume previous run** to continue.

Overlapping original drone photographs are required. Importing existing PIX4D outputs is a separate, viewing-only workflow.

### Help

Help is built in: press **F1** for explanations, hover controls for tooltips, or follow the twelve-step guide from first launch to closing the application. Search, saved startup preferences and the offline guide also work before native prerequisites are installed.

## Features

### Processing

- Dataset wizard, local dashboard, project browser, settings and persistent projects.
- Complete dataset ZIP export and restoration into a separate project.
- Atomic photo and output imports, JPG/MPO compatibility, background jobs, a single-worker dataset queue, cancellation and interrupted-job recovery with checkpoints.
- Feature extraction, optional GPU (CUDA) matching, camera calibration and sparse reconstruction, with quality diagnostics.
- Camera calibration groups that assign photos to shared cameras (five camera models, automatic or supplied starting parameters).
- Depth estimation, dense cloud fusion, surface reconstruction and textured OBJ export, with Preset, Original, Half and Quarter depth-image resolution.
- Automatic texturing-engine selection, original-photo texturing and colour blending across model parts.
- Large surfaces are reconstructed in overlapping tiles. Native stages are monitored against a memory budget.
- Automatic CPU, RAM and GPU detection, refreshed before each phase.
- **Check processing capacity** reads image headers and local resources without starting a job.
- **Schedule start** queues a one-time run at a chosen local date and time within 365 days. Keep the app open and the computer awake.
- Source-photo fingerprints reject changed photos before a resume, so start a new run after replacing photos.
- Resume restores saved feature and matching caches, so a failed run does not repeat finished work.

### Coordinates and control

- Robust camera-GPS similarity alignment in a metric projected CRS, with offline ellipsoidal-to-EGM2008 height conversion.
- Optional GPS standard deviations in Coordinates settings.
- Manual tie-point marking and bundle reoptimization.
- Ground control points (GCPs) and withheld checkpoints, imported from CSV. Optional `sigma_x`, `sigma_y` and `sigma_z` columns weight GCP coordinates. Controls without standard deviations stay fixed, and checkpoints never enter the adjustment.
- Per-mark XYZ precision diagnostics in control and checkpoint reports.
- Draw an output boundary on the camera-GPS preview before reconstruction, or save a projected boundary on the selected cloud, mesh, orthomosaic or DSM. Boundaries support coordinate editing and clear/reload.

### Viewing

- Georeferenced TIFF and DSM previews with full-resolution raster windows.
- LAS/LAZ and PLY point-cloud previews. Large clouds are sampled for display.
- Textured mesh viewer with orbit controls, a local mesh-parsing worker and exact full-resolution triangle picking.
- Raster comparison stacks of up to four overlays per base, with independent opacity and visibility.
- Official DGU imagery selection (defaults to 2025 / 2026) with fresh visible-area detail when zooming the comparison. Layer changes keep the reviewed TIFF placement and camera view.
- Original-photo inspection from mesh and map points: calibrated projection, visibility categories, pan/zoom/full screen, pinned photographs, drawn or numeric boxes and lossless crop export.

### Measurement

- Labelled tools: Distance, Polyline, Polygon, Circle/radius, Volume, Point and Elevation profile.
- Automatic two-point distances, multi-vertex completion and live feedback.
- Measurements on projected rasters and on the surfaces of generated georeferenced meshes, including 3D line lengths, XYZ points, planar and non-planar polygon areas, and vertical walls.
- **Mesh total area**, and mesh surface area inside an XY boundary.
- DSM/DTM surface area inside a drawn polygon. Heights must be in metres, and missing cells are not extrapolated.
- Elevation profiles from saved map or mesh lines, with CSV export.
- Cut/fill volumes against a horizontal reference plane or another elevation raster, with six volume base choices on generated maps and meshes. Raster comparisons require a matching CRS, pixel grid and dimensions, and you must confirm a common height datum in metres.
- Measurements are saved with the project and included in backups. Source-photo measurements are in pixels.
- Colours distinguish points, lines, polygons and volumes. Cut and fill use separate colours.

### Editing

- **Edit cloud:** count, classify, crop or remove an XYZ region (or an XY polygon intersected with height bounds) against every LAS/LAZ point. Edits create separate layers and the source files stay unchanged. PLY vertex clouds (ASCII and both binary byte orders) are supported, and PLY meshes with faces are edited with topology-aware crop and removal.
- **Ground / DTM:** classifies ground candidates in projected LAS/LAZ (metres) and exports a separate classified cloud. Review the candidates before using them.
- **Orthomosaic region editing:** gain, offset and gamma corrections, or replacement from another RGB orthomosaic, written to a separate RGBA GeoTIFF with a provenance report.

### Export

- Georeferenced LAZ, textured-surface orthographic GeoTIFF and mesh-derived DSM.
- Full-cloud LAS, LAZ, CSV, XYZ and PLY exchange exports, with reports and CRS `.prj` files where available.
- Textured OBJ with all texture parts.
- DSM contours as GeoJSON (WGS84), projected XYZ CSV and 3D DXF. Set drawing units to metres in the receiving CAD application.
- Self-contained HTML reports for browser printing, and log export.
- Raster, orthomosaic and DSM publication is staged and journaled, so a failed write keeps the previous products.

## Known limitations

- Point-cloud previews are sampled for display. Edits and counts use the full cloud.
- Some grey (unobserved) faces, texture seams and geometry gaps can remain.
- Unobserved DSM cells stay NoData. Mapper does not invent ground surface.
- Raster pixel size is not a verified source-image GSD, and a completed reconstruction does not certify survey accuracy. GPS fit residuals are not independent checkpoint accuracy.
- PLY meshes cut by an edit lose faces crossing the selection, and watertightness is not guaranteed.
- Rolling-shutter correction, orthomosaic seamline editing, thermal and multispectral calibration, and a signed installer are not yet available.
- Total disk and time forecasts for a run are approximate. Make sure the working drive has plenty of free space; Mapper stops a run and keeps its checkpoints if the reserve falls too low.
