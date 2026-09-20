# GCP Coordinate Transformation

A MATLAB-based photogrammetric coordinate transformation project that implements and evaluates multiple mathematical transformation models using **Ground Control Points (GCPs)** and **check points**.

The project uses least-squares adjustment to estimate transformation parameters and evaluates the resulting models using residuals and Root Mean Square Error (RMSE).

## Overview

The program reads GCP and check-point data from a CSV file and allows the user to select one of four transformation models:

* **Conformal**
* **Affine**
* **Projective**
* **Second-Order Polynomial**

For each model, the program:

1. Reads the input control-point data.
2. Separates GCPs and check points.
3. Estimates transformation parameters using least squares.
4. Calculates residuals for the GCPs.
5. Visualizes the residual vectors.
6. Applies the estimated transformation to the check points.
7. Calculates RMSE in the X and Y directions and the total RMSE.

## Transformation Models

### Conformal Transformation

The conformal model uses **4 parameters** and preserves angles and scale while allowing translation and rotation.

A minimum of **2 GCPs** is required.

### Affine Transformation

The affine model uses **6 parameters** and can account for translation, rotation, scaling, and shear.

A minimum of **3 GCPs** is required.

### Projective Transformation

The projective model uses **8 parameters** and represents a general planar projective transformation.

A minimum of **4 GCPs** is required.

### Second-Order Polynomial Transformation

The second-order polynomial model uses **12 parameters** and includes quadratic and cross-product terms:

```text
1, x, y, x², y², xy
```

A minimum of **6 GCPs** is required.

## Least-Squares Adjustment

The transformation parameters are estimated by constructing a design matrix from the GCP coordinates and solving the resulting system using MATLAB's least-squares operator:

```matlab
xcap = A \ L0;
```

The adjusted observations and residuals are then calculated from the estimated parameters.

## GCP and Check-Point Workflow

The input CSV file contains:

* Easting
* Northing
* Longitude
* Latitude
* Point type

Points are separated according to their type:

```text
GCP
CHECK
```

The selected GCPs are used to estimate the transformation parameters, while the check points are reserved for independent accuracy assessment.

## Accuracy Assessment

The transformation is evaluated using the check points.

The program calculates:

* **RMSE X**
* **RMSE Y**
* **Total RMSE**

The total RMSE is calculated from the X and Y components:

```text
RMSEtotal = √(RMSEX² + RMSEY²)
```

## Residual Visualization

Residual vectors for the GCPs are plotted using MATLAB's `quiver` function.

The visualization shows the difference between the original ground coordinates and their adjusted coordinates, providing a graphical representation of the residuals.

## Requirements

* MATLAB
* A CSV file containing the required GCP and check-point data
* MATLAB functions included with the project

## Project Structure

```text
gcp-coordinate-transformation/
│
├── main.mlx
├── gcp_with_types.csv
├── valueCollector.m
├── gcpCheckProvider.m
├── conformal.m
├── affine.m
├── projective.m
├── polynomial.m
├── plotVcapsGcp.m
├── rmseCalculator.m
└── README.md
```

> The function files listed above represent the logical components implemented in `main.mlx`. If the functions are kept inside the Live Script, only `main.mlx` and the input CSV file are required.

## How to Run

1. Open `main.mlx` in MATLAB.

2. Make sure `gcp_with_types.csv` is located in the MATLAB working directory.

3. Run the script.

4. Select one of the available transformation models when prompted:

```text
conformal
affine
projective
polynomial
```

5. Enter the number of GCPs and check points.

6. The program estimates the transformation parameters, displays the adjusted points and residual vectors, and reports the RMSE values.

## Input Data

The CSV file should contain the required coordinate and point-type fields:

```text
Easting
Northing
Longitude
Latitude
Type
```

The `Type` field identifies whether each point is a `GCP` or `CHECK` point.

## Key Concepts

This project demonstrates practical implementation of:

* Photogrammetric coordinate transformations
* Ground Control Points
* Check-point validation
* Least-squares adjustment
* Conformal transformations
* Affine transformations
* Projective transformations
* Polynomial transformations
* Residual analysis
* RMSE-based accuracy assessment
* MATLAB matrix operations
