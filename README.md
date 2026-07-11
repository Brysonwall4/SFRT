#### Radiation Therapy
# Geometric Pptimization of Dose Distribution in SFRT

This repository contains the code accompanying the following article:

> S. Hosseinian, N. Kuma, V. Takiar, and A. Frankart. [Geometric Pptimization of Dose Distribution in Spatially Fractionated Radiation Therapy.](https://doi.org/10.1088/1361-6560/ade7d2) Physics in Medicine & Biology 70 (2025): 165007.

## Version Note: Boundary Densification

This branch contains an updated version of the candidate-generation code. The main change is a boundary densification step in `scripts/main_discretize_single.ipynb`.

Instead of using only the original dotted tumor boundary points, this version interpolates additional points between neighboring tumor boundary points on each CT slice. This gives the candidate-placement logic a more continuous tumor contour to work with before generating possible sphere locations.

In the tested dataset, this changed the initial vertex count from 3988 to 3948, removing 40 candidate locations that were likely caused by gaps in the original boundary representation.

Bryson Wall assisted with this updated code version and helped test the boundary densification approach.
