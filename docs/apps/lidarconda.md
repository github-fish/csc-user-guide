# Lidarconda

Lidarconda is a collection of python packages that are used in manipulating and processing of LiDAR data. 

-   [gdal] - GIS translator library for accessing and transforming geospatial data
-   [pdal] - GIS library for translating and manipulating point cloud data
-   [laspy] - Library for reading, modifying, and creating .LAS LIDAR files
-   [laxpy] - Indexing library .las files
-   [laszip] - Compressing library for turning .las files to .laz
-   [pandas] - Data analysis library
-   [scikit-learn] - Libary for data mining and data analysis 
-   [rasterio] - Raster handling library
-   And many more, for retrieving the full list in Puhti use:
    `list-packages`

Additionally geoconda includes:

-   [GDAL/OGR](../apps/gdal.md) commandline tools 3.0.1
-   [LasTools](../apps/lastools.md)) 20171231. Only the opensource tools

Python has also packages for parallel computing, for example
**multiprocessing**, and **joblib**. 

If you need additional libraries, please contact servicedesk@csc.fi

## Available

The `lidarconda` module is available in Puhti:

* 3.7 (version number is the same as Python version)

## Usage

### Using lidarconda

For using Python packages and other tools listed above, you can initialize them with:

`module load gcc/9.10 lidarconda`

To check the exact packages and versions included in the loaded module:

`list-packages`
 

### Adding more Python packages to lidarconda

You can add more Python packages to lidarconda for your own use with `pip`, for example:

`pip install <newPythonPackageName> --user --target=/projappl/<yourProject>/python3.7/site-packages/`.

If you do not give the installation folder as target, the packages are by default installed to your home directory under
`.local/lib/python3.7/site-packages`

## License and citing

All packages are licensed under various free and open source licenses (FOSS), see the linked pages above for exact details.
In your publications please acknowledge also oGIIR and CSC, for example “The authors wish to acknowledge for computational resources CSC – IT Center for Science, Finland (urn:nbn:fi:research-infras-2016072531) and the Open Geospatial Information Infrastructure for Research (oGIIR, urn:nbn:fi:research-infras-2016072513).”

### References


-   [PDAL tutorials](https://pdal.io/tutorial/)
-   [LAStools tutorials](https://rapidlasso.com/category/tutorials/)
-   [LASpy tutorials](https://pythonhosted.org/laspy/tutorial.html)

------------------------------------------------------------------------

  [gdal]: https://pypi.python.org/pypi/GDAL
  [pdal]: https://github.com/PDAL/python
  [laspy]: https://pythonhosted.org/laspy/
  [laxpy]: https://github.com/brycefrank/laxpy
  [laszip]: https://laszip.org/
  [pandas]: https://pandas.pydata.org/
  [scikit-learn]: https://scikit-learn.org/stable/
  [rasterio]: https://mapbox.github.io/rasterio/
  


