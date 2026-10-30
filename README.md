# MaticTechMapper

A local Windows desktop photogrammetry application. It turns overlapping drone photographs (such as DJI JPGs) into a georeferenced dense point cloud, textured 3D mesh, orthomosaic and DSM, and provides tools to inspect, measure and export the results.

> **Development preview.** Output accuracy is not independently certified. Verify results against your own survey controls before relying on them for production work.

## Getting started

### Install and run

- **Portable:** extract the full application folder (including `_internal`) and run `Start_MaticTechMapper_Portable.cmd`. No Python installation is required.
- **Setup EXE:** close Mapper, then run the installer. It packages the same application and bundled processing runtime.
- Projects are stored in `D:/MaticTechMapper/MaticTechMapperData`. The folder is created when needed. Use `--data-dir` to select a different workspace.
- The local service binds only to the loopback address. Opening Mapper starts it automatically, so no manual server command is needed.

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

## Update history

**0.105** repairs independent camera-group calibration and landmark conversion. Resume a saved failed dataset to reuse its High-detail feature and matching caches. A validated camera export is published after the geometry checks pass. See [SFM_COMPONENTS_0.105.md](SFM_COMPONENTS_0.105.md).

**0.104** adds official DGU imagery selection and fresh visible-area detail when zooming the comparison. It works with completed surveys, so no new mapping run is needed. See [DGU_IMAGERY_0.104.md](DGU_IMAGERY_0.104.md).

**0.99** fixes GUI startup recovery. Mapper starts its embedded local service, checks readiness before the native window opens and reconnects after a failed first request. The first survey load has a longer timeout, and Help waits for the connection. See [GUI_STARTUP_FIX_0.99.md](GUI_STARTUP_FIX_0.99.md).

**0.94** adds surface colour blending across all model parts. Geometry, UVs, DSM and saved measurements are unchanged, and no full remapping is required. See [TEXTURE_CONTINUITY_0.94.md](TEXTURE_CONTINUITY_0.94.md).

**0.93** recovers grey triangles, reduces photo joins on planar surfaces and adds a denser point overlay with display-size controls. No full remapping is required. See [PHOTO_QUALITY_0.93.md](PHOTO_QUALITY_0.93.md).

**0.92** repairs missing photo assignments and colour seams. Visibility is checked against the complete model, and no new mapping run is needed. See [WHOLE_MODEL_TEXTURE_FIX_0.92.md](WHOLE_MODEL_TEXTURE_FIX_0.92.md).

**0.89** fixes the Original-resolution depth failure with serialized GPU estimation, bounded geometric groups, immutable checkpoints and no silent CPU fallback. Use **Resume previous run** to continue at fusion. See [DEPTH_RECOVERY_0.89.md](DEPTH_RECOVERY_0.89.md).

**0.88** adds cleaner original-photo texturing with calibrated image windows and full-photo recovery, plus ortho-image repair at existing heights. See [QUALITY_UPDATE_0.88.md](QUALITY_UPDATE_0.88.md).

**0.86** adds automatic texturing-engine selection from calibrated view overlap, original-resolution recovery for unsupported faces and verified texture checkpoints. See [TEXTURE_SPEED_0.86.md](TEXTURE_SPEED_0.86.md).

**0.83** adds permanent Help, F1 explanations, tooltips and the twelve-step guide, plus distinct colours for points, lines, polygons, volumes, cut and fill. See [WINDOWS_INSTALLATION.md](WINDOWS_INSTALLATION.md), [LAPTOP_QUICK_START.md](LAPTOP_QUICK_START.md) and [HELP_WORKFLOW.md](HELP_WORKFLOW.md).

**0.81** improves viewer response and mouse operation: navigation stays active while measuring, annotation selection keeps the existing mesh, mesh parsing runs in a local worker, idle drawing stops and vertex handles are editable. See [VIEWER_RESPONSE.md](VIEWER_RESPONSE.md).

**0.80** reworks the measurement workflow with labelled tools, automatic two-point distances, multi-vertex completion, live feedback, saved measurements and six volume base choices. See [MEASUREMENT_WORKFLOW.md](MEASUREMENT_WORKFLOW.md).

**0.67** adds confirmed, recoverable deletion for inactive datasets. Active processing, scheduled jobs and imports are protected. See [PROJECT_BROWSER.md](PROJECT_BROWSER.md).

**0.66** makes all detected CPU threads available, refreshes the RAM ceiling before each phase and increases GPU matching headroom, without lowering image quality. See [PERFORMANCE_CONTROLS.md](PERFORMANCE_CONTROLS.md).

**0.65** replaces the CPU/RAM/GPU sliders with automatic resource detection and makes the portable Windows application the primary delivery.

**0.64** fixes a feature-extraction memory crash, removes the dashboard ZIP restore control, separates **Process DJI map** from the image-table scroll area and refreshes failed-run status in the header. See [STARTUP_FIX.md](STARTUP_FIX.md).

**0.63** adds output-boundary drawing on original DJI camera GPS before reconstruction. See [PROCESSING_REGION.md](PROCESSING_REGION.md).

**0.62** adds saved projected output boundaries for the selected cloud, textured mesh, orthomosaic and DSM.

**0.61** adds elevation profiles from saved lines, with CSV export. See [LINE_PROFILES.md](LINE_PROFILES.md).

Earlier previews added original-photo inspection ([PHOTO_INSPECTION.md](PHOTO_INSPECTION.md)), PLY face-aware editing and export, full-cloud exchange exports, orthomosaic region editing, contour export for visible source cells, raster comparison stacks, camera calibration groups and terrain-reference volumes.

## Known limitations

- Point-cloud previews are sampled for display. Edits and counts use the full cloud.
- Some grey (unobserved) faces, texture seams and geometry gaps can remain.
- Unobserved DSM cells stay NoData. Mapper does not invent ground surface.
- Raster pixel size is not a verified source-image GSD, and a completed reconstruction does not certify survey accuracy. GPS fit residuals are not independent checkpoint accuracy.
- PLY meshes cut by an edit lose faces crossing the selection, and watertightness is not guaranteed.
- Rolling-shutter correction, orthomosaic seamline editing, thermal and multispectral calibration, and a signed installer are not yet available.
- Total disk and time forecasts for a run are approximate. Make sure the working drive has plenty of free space; Mapper stops a run and keeps its checkpoints if the reserve falls too low.

## Building from source

Install `requirements-build.txt`, prepare the native runtime folders and run `python desktop.py`. `build_windows.ps1` creates the executable and the accompanying license and source files. Distribute the application folder together with the OpenMVS corresponding source and third-party notices, and exclude project data from release packages.
