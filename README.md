#### Radiation Therapy
# Geometric Optimization of Dose Distribution in SFRT

This repository contains the code accompanying the following article:

> S. Hosseinian, N. Kuma, V. Takiar, and A. Frankart. [Geometric Optimization of Dose Distribution in Spatially Fractionated Radiation Therapy.](https://doi.org/10.1088/1361-6560/ade7d2) Physics in Medicine & Biology 70 (2025): 165007.

## Version Note: Polygon and Distance Boundary Check

This branch contains a second experimental version of the candidate-generation code. It builds on the boundary densification update in `scripts/main_discretize_single.ipynb` and adds a stricter polygon/distance-based boundary check.

In this version, candidate sphere locations are tested against a filled tumor boundary representation instead of relying only on the original dotted contour points. The goal was to check whether a more explicit filled-boundary method would remove additional invalid candidates.

For the tested dataset, this method produced the same initial vertex count as the boundary densification version: 3948. This suggests that the boundary densification step was already capturing the main correction needed for this dataset.

Bryson Wall assisted with this updated code version and helped test the polygon/distance boundary-checking approach.
