#### Radiation Therapy
# Geometric Optimization of Dose Distribution in SFRT

This repository contains the code accompanying the following article:

> S. Hosseinian, N. Kuma, V. Takiar, and A. Frankart. [Geometric Optimization of Dose Distribution in Spatially Fractionated Radiation Therapy.](https://doi.org/10.1088/1361-6560/ade7d2) Physics in Medicine & Biology 70 (2025): 165007.

## Updated Version: Boundary Densification

This branch contains an updated version of the candidate-generation workflow in `scripts/main_discretize_single.ipynb`.

The original code used dotted tumor boundary points from each CT slice when determining where candidate sphere centers could be placed. Because the boundary points were not continuous, gaps between points could affect the candidate-generation step.

This branch adds a boundary densification step before candidate locations are generated.

## Boundary Densification Method

The densification method interpolates additional points between neighboring tumor boundary points on each slice. This creates a more continuous tumor contour while still preserving the original tumor geometry.

Instead of relying only on the sparse/dotted boundary points, the code creates a denser boundary representation and uses that updated boundary during candidate filtering and visualization.

## Original Case Result

On the original CH tumor case, the boundary densification method improved the candidate-generation process. The initial vertex count changed from:

```text
3988 to 3948
```

## Multi-Tumor Testing Observation

This branch was also tested on additional tumor geometries:

- `BL_GTV_GRID.csv`
- `CH_GTV_GRID.csv`
- `CJ_GTV_GRID.csv`
- `JM_GTV_GRID.csv`
- `SR_GTV_GRID.csv`
- `TK_GTV_GRID.csv`

The boundary densification method worked well for some tumor geometries and helped improve the continuity of the boundary representation.

In general, this method appears to work well for more regular tumor shapes where each slice has a relatively smooth and simple boundary. In those cases, filling the gaps between the dotted contour points gives the code a better boundary to work with and improves the candidate-generation process.

When the tumor geometry becomes more irregular, curved, or concave, the row-wise candidate check can still misinterpret the true shape of the boundary. In those cases, the code may place a candidate point outside the actual tumor contour even though it falls between the minimum and maximum boundary values for that row.

In particular, the CJ tumor geometry showed that the row-wise minimum/maximum candidate check could still allow some candidate points outside the actual tumor contour, even after boundary densification.

## Contribution Note

Bryson Wall assisted with this updated code version, including boundary densification testing, polygon-based candidate filtering, and multi-tumor robustness testing.
