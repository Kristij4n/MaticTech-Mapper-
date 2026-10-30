Latest version is MaticTechMapper-0.114.0-preview-windows-x64-setup

Consider to support this project on https://ko-fi.com/matictechsystems

Web: https://matictechsystems.com/
Web MaticTech Mapper: https://mapper.matictechsystems.com/

MaticTech Mapper stores substantially more mesh triangles and dense points than the supplied PIX4D export. The recent manually selected distances differ by less than 1%. Coverage and controlled volume comparisons still show remaining differences. These results do not establish overall superiority or independent surveyed accuracy.

This compares the existing saved reconstruction opened in 0.97, including its published texture repairs. Version 0.97 changes volume calculation and viewer appearance; a new complete mapping run was not performed for this document. Counts describe these particular exported files, rather than every internal model or display level used by PIX4D.

## 1. Input and reconstruction

| Parameter | Supplied PIX4D SR9 | MaticTechMapper sr9v5 | Interpretation |
|---|---:|---:|---|
| Input photographs | 98 | 98 | Same input count |
| Calibrated / registered photographs | 97 / 98 (98.98%) | 98 / 98 (100%) | Mapper registered one additional photograph |
| Sparse reconstruction points | 146,744 | 99,802 | A larger tie-point count alone does not establish better calibration |
| Dense point-cloud points | 4,571,205 | 11,414,283 | Mapper stores **2.50 times** as many points |
| Exported mesh triangles | 474,925 | 9,929,997 | Mapper stores **20.91 times** as many triangles |
| Mesh export parts | 8 | 14 | Different partitioning of the exported surface |
| Exported OBJ vertex records | 421,266 | 5,051,087 | Mapper count is from its combined export manifest; seam duplicates can occur |
| Mapper geometric vertices before texture partitioning | Not available on the same basis | 5,028,882 | Separate from OBJ seam duplication and viewer indexing |
| Mean reprojection error | Not extracted on a directly comparable basis | 0.7421 pixels | Internal image-fit statistic, not a real-world distance error |
| Mean sparse-point track length | Not extracted on a directly comparable basis | 3.4114 observations | Mapper reconstruction metadata |
| Horizontal coordinate system | WGS 84 / UTM zone 33N | WGS 84 / UTM zone 33N | EPSG:32633 |
| Height reference | EGM2008 | EGM2008 | Shared declared height reference |

More triangles can preserve more surface structure, but can also preserve noise. More points do not automatically make the surface more accurate. The supplied PIX4D OBJ may be simplified relative to its internal reconstruction.

## 2. Orthomosaic and DSM

| Parameter | Supplied PIX4D SR9 | MaticTechMapper sr9v5 |
|---|---:|---:|
| Orthomosaic dimensions, width x height | 12,350 x 12,525 pixels | 16,384 x 15,908 pixels |
| DSM dimensions, width x height | 12,350 x 12,525 pixels | 16,384 x 15,908 pixels |
| Cells in each rectangular raster grid | 154,683,750 | 260,636,672 |
| Output pixel / cell spacing | **1.3780 cm** | **1.0468 cm** |
| Reported average image GSD | 1.3781 cm | Not independently calculated in the saved report |
| Orthomosaic bands | 4, including alpha | 4, including alpha |
| DSM height bands | 1 | 1 |
| Coverage of the supplied reference orthomosaic footprint | **100% relative to itself** | **86.55%** |
| Reference footprint missing from Mapper's valid orthomosaic | 0% relative to itself | **13.45%** |

Mapper's cells are approximately **24.03% smaller along each axis**. That is finer output sampling; it does not prove that the photographs or reconstruction resolve 24.03% more real detail. Rectangular grid dimensions also include empty cells and different extents.

Coverage was remeasured on the current saved files on 19 October using a 1,200 x 1,183 reference-aligned sample grid, nearest sampling, stored coordinates and the actual validity/alpha masks. Of 570,572 valid reference samples, 493,836 have valid Mapper imagery. No alignment was fitted. The 86.55% result is a sampled footprint estimate, not an exact full-resolution area census.

Of the reference footprint, approximately **4.02 percentage points** have Mapper DSM elevation but lack valid orthomosaic imagery, while **9.43 percentage points** lack both at the sampled locations. This distinguishes missing imagery from missing top-surface elevation without identifying every underlying cause.

The earlier **89.77%** and **1.076 cm** figures belong to another output snapshot. They are not used as the current sr9v5 statistics. PIX4D's 100% here means the reference covers its own valid footprint; it does not establish complete coverage of every real object in the survey area.

## 3. Texture files and grey surfaces

| Parameter | Supplied PIX4D mesh | Current Mapper mesh |
|---|---:|---:|
| Referenced colour texture files | 8 | 934 |
| Texture image dimensions | 6,144 to 14,336 pixels per side | 256 to 4,096 pixels per side |
| Total pixels across referenced texture images | 894.44 million | 397.02 million |
| Texture file storage | 67.06 MB | 500.76 MB |

Mapper's active texture dimensions are: 810 at 256 x 256, 80 at 512 x 512, 20 at 1,024 x 1,024, 8 at 2,048 x 2,048, and 16 at 4,096 x 4,096. These counts use the 14 numbered parts displayed by the viewer and exclude the duplicate first-part alias and unused historical materials.

Texture count, atlas dimensions and compressed file size are not visual-quality scores. Packing, unused atlas space, encoding and the photographed surface assigned to each texture differ. The viewer may use a smaller preview than the stored texture.

For the full saved model, the 0.97 viewer check recorded:

| Mapper display metric | Count |
|---|---:|
| Triangles without a photographed texture assignment | 123,552 |
| Those displayed with connected neighbour colours | 123,244 |
| Those remaining neutral grey without connected colour support | 308 |

Neighbour colours reduce the visible grey appearance without adding photographic detail. Those faces remain untextured; this setting does not increase orthomosaic coverage, change geometry, change measurement coordinates or repair the exported OBJ textures. A corresponding PIX4D untextured-face count has not been measured, so no numerical grey-surface ranking is assigned.

## 4. Distance, elevation and polygon examples

The following are user comparisons and the retained annotation snapshot. Vertices were manually selected independently; the comparisons do not use identical exported reference vertices.

| Example | PIX4D | Mapper | Mapper minus PIX4D | Absolute relative difference |
|---|---:|---:|---:|---:|
| Recent nearly vertical 3D distance | 7.229 m | 7.209 m | -0.020 m | **0.277%** |
| Recent distance: minimum elevation | 87.564 m | 87.581 m | +0.017 m | -- |
| Recent distance: maximum elevation | 94.783 m | 94.775 m | -0.008 m | -- |
| Recent distance: elevation span | 7.219 m | 7.194 m | -0.025 m | **0.346%** |
| Cable polyline, earlier screenshot comparison | 28.301 m | 28.325 m | +0.024 m | **0.085%** |
| Roof edge, earlier screenshot / saved Distance 1 | 25.421 m | 25.320 m | -0.101 m | **0.397%** |
| Roof polygon horizontal area, reference Polygon | 345.7124 m² | 342.8793 m² | -2.8331 m² | **0.820%** |
| Roof polygon 3D area, reference Polygon | 345.7246 m² | 342.9044 m² | -2.8202 m² | **0.816%** |

Relative difference is `100 x abs(Mapper - PIX4D) / abs(PIX4D)`. For elevation comparisons, the relative figure uses the elevation span rather than the absolute elevation above the datum.

These examples meet the user's under-1% agreement target against PIX4D. Endpoint and boundary placement contribute to the differences; agreement in these examples does not certify every distance, thin object, surface or volume. The latest 7.229 m comparison and its reference elevations were supplied by the user in this chat, rather than extracted from the earlier reference PDF.

## 5. Volumes

### Controlled comparison with identical boundary and base

For this calculation, both DSMs were integrated by the corrected Mapper 0.97 algorithm using the **same saved Mapper world-coordinate vertices, base heights and triangulated base**. These are values calculated from the supplied reference DSM, not a replay of PIX4D's original UI annotations.

| Object | Supplied PIX4D DSM, shared boundary/base | Mapper DSM, shared boundary/base | Absolute difference | Relative difference |
|---|---:|---:|---:|---:|
| Long pile | 6.341403 m³ | 6.190947 m³ | 0.150456 m³ | **2.373%** |
| Small object | 1.274951 m³ | 1.245302 m³ | 0.029650 m³ | **2.326%** |

This demonstrates a remaining difference between the two height surfaces under a common base. It includes any height bias relative to that base and does not establish which surface is physically correct. These volume comparisons are above the under-1% target.

### Original report annotations: different boundaries

| Object | PIX4D report boundary area | Mapper comparison boundary area | PIX4D report net volume | Recalculated Mapper net volume |
|---|---:|---:|---:|---:|
| Long pile | 7.8646 m² | 7.9403 m² | 6.8668 m³ | 6.190947 m³ |
| Small object | 3.4499 m² | 3.7709 m² | 1.1754 m³ | 1.245302 m³ |

The small-object Mapper boundary is about 9.3% larger. These original annotation totals use different selections and base heights, so their difference cannot be treated as an isolated calculation-error percentage. The report contains totals and pictures but no exact reference vertex coordinates. This table uses the preserved comparison snapshot; later edits to the live survey's volume boundaries are outside its scope.

Raising the selected base by 1 cm changes net volume by approximately **-0.0794 m³** for the long-pile boundary and **-0.0377 m³** for the small-object boundary, assuming the same covered surface. This explains why base-height selection matters. It is sensitivity, not a surveyed error estimate.

The PIX4D report lists estimated cut errors of 0.1626 m³ and 0.0649 m³ for these two objects. Those are software estimates, not independently measured true-volume errors. Mapper's exact cell/base integration is analytically tested, but neither that testing nor this comparison establishes surveyed volume accuracy.

## 6. File sizes

MB below means 1,000,000 bytes. These are final product sizes, not peak temporary processing storage.

| Product | Supplied PIX4D | Mapper |
|---|---:|---:|
| Dense point cloud, LAZ | 22.89 MB | 69.61 MB |
| Orthomosaic, GeoTIFF | 231.59 MB | 184.52 MB |
| DSM, GeoTIFF | 261.68 MB | 225.81 MB |
| Referenced mesh textures, total | 67.06 MB | 500.76 MB |

Different compression, valid coverage, point counts and material organization affect storage. A smaller file is not evidence of lower or higher quality.

## 7. Processing time

| Recorded timing | PIX4D | Mapper |
|---|---|---|
| Available record | Supplied PIX4Dcloud backend log | Saved sr9v5 quality report |
| Recorded interval / duration | 08:00:48 to 08:15:07 on 6 October 2026: **14 min 19 s** | `elapsed_seconds_this_attempt = 9897.70`: **2 h 44 min 57.7 s** |
| Environment / scope | Cloud backend; hardware not established by this comparison | User's laptop; most recent recorded worker attempt |

The supplied PIX4D log span is not a certified total time including upload, queueing, all exports and delivery. Mapper's field records one worker attempt and may involve retained stages; it does not prove a clean run from zero. Subsequent texture-repair work and 0.97 viewer/calculation updates are outside that saved mapping duration. A fresh end-to-end 0.97 timing on identical local hardware is not available. No direct speed ratio is claimed.

## 8. What the numbers establish

- **Larger stored geometry:** Mapper has 20.91 times the triangles and 2.50 times the dense points of the supplied exports.
- **Finer raster sampling:** 1.0468 cm cells versus 1.3780 cm; effective photographic resolving power has not been independently measured.
- **Close agreement in selected measurements:** the listed distances and polygon areas differ by less than 1%.
- **Remaining footprint gap:** current sampled photographic coverage is 86.55% of the reference footprint.
- **Remaining volume difference:** identical-boundary/base comparisons differ by approximately 2.33% to 2.37%.
- **Overall quality:** sharper appearance, physical accuracy and better-than-PIX4D output remain separate questions. The file counts alone do not establish them.

## Evidence and reproducibility

All survey/reference files were read without modifying their geometry, raster values or annotations for this document. No reference geometry was fitted into Mapper's output.
