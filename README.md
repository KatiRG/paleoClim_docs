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

##CB

## Create a JSON file from netCDF

Convert NetCDF files to Json files using NonScalar.py (to convert files containing wind or current data, i.e. vectors) and Scalar.py (to convert scalar data)

	-open the appropriate converting script

	-change the following statement as you wish :

	   * Data = open("NameOfTheJsonFile.json", "w") 

		* nc = netCDF4.Dataset('NameOfTheNetcdfFile.nc')

		* data = nc.variables['NameOfTheVariable'][:,:,:]

	-launch the script with python

## How does the application work? 

    1. The menu is displayed by index.html, each button have a specific name that will be used later

    2. earth.js gives each button specific information to send to product.js such as "overlayType or mode"

    3. product.js use the informations sent by earth.js to display specific data.It contains multiple functions 
       (one for each type of data), using the mode's parameter, it knows wich function to use and in wich file to look.

## Customization of the code 

    - **on index.html**
        *added modes button (temperature, precipitation and cloudness)

    - **on earth.js**
        * d3.selectAll(".mode").each(function() {
            var id = this.id, type = id;
            bindButtonToConfiguration("#" + id, {param: "wind", mode: type});
        });
        
        * the mode's buttons will send param : wind, their name as the mode 

        * d3.selectAll(".mois").each(function() {
            var id = this.id, ot = id;
            bindButtonToConfiguration("#" + id, {param: "wind", overlayType: id});
        });

        * the month's buttons will send their name as the overlayType 

     - **on product.js**

        * function gfs1p0degPath(type) {
        var file = type +".json";
        return [WEATHER_PATH,file].join("/");
        }

        * the function to build the file's name is using one variable wich will be the mode's parameter sent by the mode's buttons.

        * dict= ['Janvier','Fevrier','Mars','Avril','Mai','Juin','Juillet','Aout','Septembre','Octobre','Novembre','Decembre']

        * will be used to find the data corresponding to the choosen month 

        * if (dict.indexOf(attr.overlayType)!==-1){
                            k=dict.indexOf(attr.overlayType)
                        }else{
                            k=0
                        }
                        var record = file[k], data = record.data;
        *this is how the code fetch the correct data, for example, if overlayType is 'Janvier', k will be equal to 0,
        and file[0] will be the first set of data of the file, in our organisation, it correspond to the first month of the year. (if overlayType is note in the dict, it takes january's data)

        * "temp": {
            matches: _.matches({param: "wind", mode :"temp"})
            
        * this is how the code enter the right fonction, by checking the informations sent by earth.js