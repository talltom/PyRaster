PyRaster
========
###Python Geospatial Image Processing

[![Build Status](https://travis-ci.org/talltom/PyRaster.svg?branch=dev)](https://travis-ci.org/talltom/PyRaster)

###About
PyRaster provides two elements:

1. RasterIO module

  The aim of the module is to provide a high-level API for Python software development for processing large suites of geospatial rasters, with a particular focs on satellite imagery.

  The RasterIO module uses the Geospatial Data Abstraction Library ([GDAL](http://gdal.org)) to read and write images and their geospatial metadata to and from NumPy arrays.

2. Scripts

  The utility scripts use RasterIO to provide a series of functions for batch processing remote sensing imagery, including vectorizing array operations and using the multiprocessing module.

###References
This software was developed as part of my PhD thesis to batch process large time series of Earth observation data.
* [PhD Thesis](http://hdl.handle.net/10443/1856) - Holderness, T. 2013
* [PyRaster presentation](https://tomholderness.files.wordpress.com/2012/12/holderness_asu_pyraster_pres_handout.pdf) to Mars Space Flight Facility in 2010
* [Raster Processing Suite](http://talltom.github.io/Raster-Processing-Suite/) QGIS plugin.

###API Documentation
A PDF of API documentation for RasterIO can be found in the "doc" folder.

###Supported Formats
* Input: Any [GDAL supported format](http://gdal.org/formats_list.html)
* Output: Default is GeoTiffs created with embedded binary header files containing geospatial information
* Default data-type: 32-bit float
* RasterIO handles mapping of GDAL (C) data-types to Python

###Dependencies
* Python 2.7
* Numpy 1.9
* OSGeo (GDAL) 1.11

###Installation

###In-built help
Documentation for module functions is provided as Python docstrings, accessible from an interactive Python terminal. For example:

```python
>>> from pyraster import rasterio
>>> help(rasterio.wkt2epsg)
Help on function wkt2epsg in module rasterIO
wkt2epsg(wkt)
Accepts well known text of Projection/Coordinate Reference System and generates EPSG code
```

###1 Getting Started with the RasterIO API
####1.1 Reading a file
To read a raster file from disk and covert it to a NumPy array three RasterIO functions are required.

1. opengdalraster(file name)
Accepts a GDAL compatible file on disk and returns GDAL dataset.

2. readrasterband(dataset, band_number, NoDataVal=None, masked=True):
Accepts a GDAL raster dataset and band number, returns Numpy 2D-array.'''

3. readrastermeta(dataset):
Accepts GDAL raster dataset and returns a dictionary containing the GDAL driver, number of rows, number of columns, number of bands, projection info (well known text), and geotranslation metadata.

Figure 1 shows the process of loading a raster file into a NumPy array using RasterIO open and read functions. For example:

```python
from pyraster import rasterio as rio
dataset = rio.opengdalraster('file.tif')

band_number = 1
b1_data = rio.readrasterband(dataset, band_number)

print type(b1_data)
<class 'numpy.ma.core.MaskedArray'>
```

![Figure 1 - loading raster data](https://raw.githubusercontent.com/talltom/PyRaster/dev/doc/diagrams/rasterIO_processing_flowline_read.jpg)
Figure 1. Loading raster data.

####1.2 Masked Arrays & NoData Values
If the input data has no recognisable NoDataValue (readable by GDAL) then the input NoDataValue is assumed to be 9999. This can be changed by manually specifying an input NoDataVal when calling read- rasterbands() In accordance with GDAL the output data NoDataValue is 9999 or 9999.0 or can be manually set by when writrasterbands() When using unsigned integer data types the default output NoDataValue will be 0.

####1.3 Metadata Handling
The `readrastermeta()` function complements the `readrasterband()` function to read the geospatial raster meta-data from a raster file. Using `readrastermeta()` with `readrasterband()` means that when a raster is loaded into a NumPy array the geospatial information can be retained through the Python processing flow-line, and written with output data if required.

Continuing the example above to find the GDAL drive for the input data:

```python
metadata = rio.readrastermeta(dataset)
print metadata['driver']
GTIFF
```

###2 Raster Processing
####Simple Example

###3 Writing Data

###4 Scripts
####
