# PaleoClim Pipeline

This document is written in markdown. Please see these link for some helpful tips: 
- https://help.github.com/articles/basic-writing-and-formatting-syntax/#lists
- https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet.

To preview a markdown file in your browser, follow these installation and usage instructions:
http://plaintext-productivity.net/2-04-how-to-set-up-sublime-text-for-markdown-editing.html (NB: in the end, hit ctrl+shift+G to locally preview in your browser)

## Create a topoJSON file from netCDF

1. Convert netCDF to a shapefile

	- **Using QGIS**
        * Load netCDF as a **Raster** layer (not a Vector Layer!!)
        
        * In the Layers Panel, right-click on the file and choose "Save as" then select GTiff (in fact, GTiff is the only option)
        
        * On the command line, use `gdal_polygonize.py` to convert the `tif` file to a `shp` file:
        
            `$ gdal_polygonize.py file.tif file.shp`
    
    - **Using nco**
    PS to fill in. 


2. Convert the shapefile into geoJSON on the command line using `ogr2ogr`:

    `$ ogr2ogr -f GeoJSON file.geoJSON file.shp`

3. Convert the geoJSON to topoJSON using the online tool mapshaper.org [mapshaper.org](http://mapshaper.org/). See the  [documentation](https://github.com/mbloch/mapshaper/wiki/Command-Reference) for a list of command line options you can use to simplify or adjust the default parameters.
