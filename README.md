#### Radiation Therapy
# Geometric Optimization of Dose Distribution in SFRT

This repository contains the code accompanying the following article:

> S. Hosseinian, N. Kuma, V. Takiar, and A. Frankart. [Geometric Optimization of Dose Distribution in Spatially Fractionated Radiation Therapy.](https://doi.org/10.1088/1361-6560/ade7d2) Physics in Medicine & Biology 70 (2025): 165007.

## Updated Version: Polygon Boundary Filtering

This branch contains an updated version of the candidate-generation workflow in `scripts/main_discretize_single.ipynb`.

The original method relied on tumor boundary points from each CT slice and used a row-wise minimum/maximum boundary check to decide where candidate sphere centers could be placed. While that approach worked for simpler tumor shapes, testing on additional tumor geometries showed that it can fail for more irregular or concave contours.

## Boundary Densification

The first improvement tested was boundary densification. This interpolates additional points between neighboring tumor boundary points on each slice, making the originally dotted tumor contour more continuous before candidate locations are generated.

Boundary densification improved the original tumor case by reducing boundary-gap effects. However, when tested on additional geometries, especially the CJ tumor case, the row-based method could still allow some candidate points to appear outside the actual tumor boundary.

## Polygon-Based Candidate Filtering

This branch improves the candidate-generation step by adding a polygon-based containment check.

For each tumor slice, the code builds a filled polygon representation of the tumor boundary. A candidate sphere center is only accepted if it falls inside that polygon. This avoids the weakness of the row-wise minimum/maximum method, where a point can fall between the left and right boundary limits but still lie outside the true contour.

## Multi-Tumor Robustness Testing

The polygon method was tested on six tumor geometries:

- `BL_GTV_GRID.csv`
- `CH_GTV_GRID.csv`
- `CJ_GTV_GRID.csv`
- `JM_GTV_GRID.csv`
- `SR_GTV_GRID.csv`
- `TK_GTV_GRID.csv`

Across these tests, the polygon-based version performed the best. In the CJ case, the boundary-densification-only method still allowed candidate points outside the tumor boundary, while the polygon method kept the candidates inside the actual tumor contour.

## Conclusion

Boundary densification helps make the dotted tumor boundary more continuous, but polygon-based containment is the more robust candidate-filtering method across different tumor geometries.

This version should be treated as the stronger updated approach because it checks whether candidate sphere centers are truly inside the tumor boundary, rather than relying only on row-wise minimum and maximum boundary values.

## Contribution Note

Bryson Wall assisted with this updated code version, including boundary densification testing, polygon-based candidate filtering, and multi-tumor robustness testing.
