# Google Earth Engine Script Writing Tutorial

## Introduction

## Upload Shapefiles to GEE
1. Ensure that the shapefile contains all necessary associated files (.shp, .shx, .dbf, .prj). Also, ensure that the shapefile is projected in a supported coordinate reference system (e.g., WGS84, EPSG:4326).
2. Create a compressed file (.zip) containing all shapefile associated files.
3. Open Earth Engine Code Editor
4. Navigate to "Assets" in the left panel. Click on the "NEW" button and select "Shape files" (.zip) to proceed to upload your compressed file. The uploaded shapefile will appear under "Assets"
5. Load the shapfiles onto the viewing panel with following code:


```
var shapefile = ee.FeatureCollection("path/to/uploaded_shapefile");

// Visualize the shapefile
Map.addLayer(shapefile,{} ,'Shapefile name', true);
Map.centerObject(shapefile,  10);  
```


## Date

## Examples of Scripts
1. To calculate the average precipitation by are of interest (in polygon) and months
2. To calculate the daily precipitation by location (in points)

## Further Readings
Gandhi, U. End-to-End Google Earth Engine (Full Course). [https://courses.spatialthoughts.com/end-to-end-gee.html]

Wu, Q. Geospatial Analysis with Google Earth Engine: A Tutorial. [https://datadrivenlab.org/big-data/google-earth-engine-tutorial/]

Data-Driven Lab. Earth Engine Tutorial #34: Interactive extraction of pixel values and interactive region reduction. [https://blog.gishub.org/earth-engine-tutorial-34-interactive-extraction-of-pixel-values-and-interactive-region-reduction]
