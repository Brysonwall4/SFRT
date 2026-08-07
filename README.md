#### Radiation Therapy
# Geometric Optimization of Dose Distribution in SFRT

This repository contains the code accompanying the following article:

> S. Hosseinian, N. Kuma, V. Takiar, and A. Frankart. [Geometric Optimization of Dose Distribution in Spatially Fractionated Radiation Therapy.]((https://iopscience.iop.org/article/10.1088/1361-6560/adf58f)) Physics in Medicine & Biology 70 (2025): 165007.

## Updated Version: Boundary Cleanup and Valley Dose Improvement

This branch contains two main updates to the original SFRT workflow.

The updates are located mainly in:

- `scripts/main_discretize_single.ipynb`
- `scripts/main_MWIS_parallel.py`

## 1. Tumor Boundary Cleanup

The polygon boundary method was already useful for keeping candidate sphere centers inside the tumor contour. However, some irregular tumor slices showed extra shading or messy boundary connections.

The issue came from the boundary densification method. The earlier method sorted boundary points by angle around the slice centroid, which could accidentally connect non-adjacent points in concave or irregular tumor shapes.

This version fixes that by preserving the original contour point order from the CSV file. The code then interpolates between consecutive contour points in that original order.

This produces cleaner tumor boundary plots and reduces overshading while keeping the polygon-based candidate filtering method.

## 2. Valley Dose Improvement

The original graph-construction code assumed that the minimum valley dose between two candidate spheres occurs at the exact midpoint between their centers.

This branch improves that by adding a sampled valley-dose line search. When the midpoint dose is within 10% of the valley-dose threshold, the code samples points along the line between the two candidates and finds the lowest combined dose point.

This gives a better estimate of the true valley-dose location, especially when candidate spheres have different radii or dose falloff behavior.

## Results

Original midpoint-only method: Number of Edges = 5,416,969

Updated sampled valley-dose method: Number of Edges = 5,414,583

The updated method produced 2,386 fewer conflict edges overall.

Additional diagnostics: Pairs checked with sampled valley search = 259,494; Midpoint conflict but line search safe = 5,955; Midpoint safe but line search conflict = 1,632; Valley points shifted from midpoint = 132,068; Maximum valley shift distance = 0.6829; Maximum midpoint valley dose error = 7.91.

These results show that the midpoint assumption is not always accurate. The sampled line-search method changed the conflict decision for 7,587 candidate pairs and found that the minimum valley-dose point shifted away from the midpoint in 132,068 checked pairs.
