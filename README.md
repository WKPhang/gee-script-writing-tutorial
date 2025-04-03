# Google Earth Engine Script Writing Tutorial

## Introduction

## Components of Earth Engine Code Editor Interface
Detailed information can be found in https://developers.google.com/earth-engine/guides/playground

<p align="center">
  <img src="https://developers.google.com/static/earth-engine/images/Code_editor_diagram.png">
  
  Diagram of components of the Earth Engine Code Editor.
</p>


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


## Working with Date
GEE requires dates to be in the YYYY-MM-DD format when specified as a string. However, it is recommended to use the ee.Date object for better handling and manipulation of time-based data.

Example:
```
var dateExample = ee.Date('2023-01-01');
print('Date Example:', dateExample)

// Console:
// Date Example:
// Date (2023-01-01 00:00:00)

```

### Extracting Date Information from ee.FeatureCollection Object
In Google Earth Engine (GEE), an ee.FeatureCollection object contains tabulated data similar to an attribute table in QGIS or a dataframe in R and Pandas. This means that features within an ee.FeatureCollection can store various attributes, including date information.

When working with date attributes, we often need to ensure that they follow a consistent format. Below, we demonstrate how to extract and reformat date information from an ee.FeatureCollection object. Specifically, we handle date formats in both DD/MM/YYYY and DD/M/YYYY formats (e.g., 01/05/1998 or 01/5/1998).

```
// Sample FeatureCollection with date attributes in mixed formats
var points = ee.FeatureCollection([
  ee.Feature(ee.Geometry.Point([101.6869, 3.1390]), {'date': '01/5/1998'}),
  ee.Feature(ee.Geometry.Point([100.5018, 13.7563]), {'date': '12/11/2005'}),
  ee.Feature(ee.Geometry.Point([103.8198, 1.3521]), {'date': '5/3/2012'})
]);

// Function to parse and format the date
function formatDate(feature) {
  var dateStr = ee.String(feature.get('date'));
  
  // Split date into day, month, and year components
  var parts = dateStr.split('/');
  var day = parts.get(0);
  var month = parts.get(1);
  var year = parts.get(2);
  
  // Ensure month is two digits (e.g., 5 → 05)
  var formattedMonth = ee.String(month).length().eq(1)
    ? ee.String('0').cat(month)
    : month;
  
  // Construct the formatted date string in YYYY-MM-DD format
  var formattedDate = ee.String(year).cat('-').cat(formattedMonth).cat('-').cat(day);
  
  return feature.set('formatted_date', formattedDate);
}

// Apply the formatting function to all features in the collection
var formattedPoints = points.map(formatDate);

// Print the results
print("Formatted FeatureCollection:", formattedPoints);
```
```
// Create function to parse and format the date, handling both DD/MM/YYYY and DD/M/YYYY formats
function formatDate(feature) {
  var dateStr = ee.String(feature.get('date'));
  
  // Check if the month part has a single or double digit
  var parts = dateStr.split('/');
  var day = parts.get(0);
  var month = parts.get(1);
  var year = parts.get(2);
  
  // Format month to ensure it is two digits
  var formattedMonth = ee.String(month).length().eq(1)
    ? ee.String('0').cat(month)
    : month;
  
  // Combine parts into a proper date string
  var formattedDate = ee.String(year).cat('-').cat(formattedMonth).cat('-').cat(day);
  
  return feature.set('formatted_date', formattedDate);
}

// Apply the formatting function to all features
points = points.map(formatDate);
```




### Filtering by Specific Date
```
var singleDay = collection.filterDate('2023-06-15', '2023-06-16');
print('Single Day Collection:', singleDay);
```



## Examples of Scripts
1. To calculate the average precipitation by are of interest (in polygon) and months
2. To calculate the daily precipitation by location (in points)

## Further Readings
Gandhi, U. End-to-End Google Earth Engine (Full Course). [https://courses.spatialthoughts.com/end-to-end-gee.html]

Wu, Q. Geospatial Analysis with Google Earth Engine: A Tutorial. [https://datadrivenlab.org/big-data/google-earth-engine-tutorial/]

Data-Driven Lab. Earth Engine Tutorial #34: Interactive extraction of pixel values and interactive region reduction. [https://blog.gishub.org/earth-engine-tutorial-34-interactive-extraction-of-pixel-values-and-interactive-region-reduction]
