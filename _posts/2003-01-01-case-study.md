---
title: "Landslide Exercise"
layout: post
category : Ask some Questions
tagline: ""
tags : [case study, landslide]
---

{% include JB/setup %}


#### Pre-requisites:

1. one
2. two
3. three

#### Objectives:

- one
- two
- three

#### Data:

iPlant Data Store: <br>&nbsp;&nbsp;&nbsp;``Community Data/aegis/Spatial-bootcamp/spatial-analysis/landslide-exercise/``

- USA_states.shp
- DEM.tif
- WA_geology.shp

----

## Landslide Susceptibility Model

### Overview

This case study has been chosen to demonstrate the concepts being highlighted by the bootcamp. We have chosen a landslide susceptibility analysis due to the diversity of inputs and a general curiousity into the methodology. The study area has been chosen in response to the Oso Mudslide[^1] which took place in Washington State on March 22nd, 2014. The methodology for this analysis is based off of work done by the California Geological Survey in Conjunction with the USGS[^2].

----

### Data Wrangling

<h4>Set project projection</h4>
<p>Before starting any project in a GIS program, you should first set the project projection to make sure your data comes in with the same extent. If you don't set the project's projection, the program will use the projection of the first layer added or EPSG:4326.</p>
 <p> You can set the projection with the following steps:</p>
 <ul>
  <li> In the top navbar go  <em>Project > Project Properties</em> <br>
  The following window should open up.<br><br>
  <img src='{{BASE_PATH}}{{ASSET_PATH}}/images/qgis-project-properties.png' class="screen-shot" /><br></li>

  <li> Select CRS in the Left menu</li>
  <li> Check <em>Enable On-the-fly CRS transformation</em></li>
  <li>Select <em>NAD83(HARN)/Washington South(ftUS) EPSG:2927</em> from the List of Projections<br><br>
  <img src="{{BASE_PATH}}{{ASSET_PATH}}/images/qgis-projection.png" class="screen-shot" /><br></li>
</ul>
<h4>Add the US States shapefile</h4>
<p>Since our project is directed at the state of Washington, we should use the state boundary as our study area. The Global Administrative Areas[^7] project provides high-quality boundary data on country,state and county levels. We can use the US-state level dataset to get the Washington boundary. </p>
<ul>
  <li> Open the iRods Plugin and navigate to the <em>Exercise_3</em> directory</li>
  <li>Load the USA_states.shp layer<br><br></li>
  <img src="{{BASE_PATH}}{{ASSET_PATH}}/images/usa-states.png" class="screen-shot" /><br>
</ul>
<h4>Create a layer for the state of Washington</h4> 
  <p>SCreating a layer for the state of Washington will let us define the boundary for our study.</p>
  <ol>
    <li>In the top toolbar, select <b>Select Single Feature</b></li>
    <li> Click on the state of Washington to select it<br><br>
    <img src="{{BASE_PATH}}{{ASSET_PATH}}/images/washington-selected.png" class="screen-shot" /><br></li>
    <li> Right click on the USA_states layer and select <em>Save Selection As...</em></li>
    <ul>
      <li>Format: ESRI Shapefile</li>
      <li> Save as: washington.shp</li>
      <li> Encoding: UTF-8</li>
      <li>CRS: Project CRS</li>
    </ul>
    <img src="{{BASE_PATH}}{{ASSET_PATH}}/images/save-washington.png" class="screen-shot" /></li>
  </ol>
<h4>Load the new washington layer</h4>
  <ol> 
    <li>Select <b>Add Vector Layer</b></li>
    <li> Browse to the new washington layer and click *Open*</li>
  </ol>
<b>Remove the USA_states layer from the project</b>
  <p>Now that we have the Washington layer, we can get rid of the U.S. layer. </p>
  <ol>
    <li>Right click the layer in the table of contents</li>
    <li> Select <b>Remove</b></li>
  </ol>

<h4> Add the DEM layer from the iPlant data store</h4>
  <p>DEMs or <em>Digital Elevation Models</em> are very useful for establishing topological context in a map. DEMs generally come as a raster dataset that consists of elevation values. There are several different byproducts that can be created from DEMs.<br>
  This DEM was taken from GTOPO30 satellite data.</p>
  <ol>
    <li> Open the iRods plugin and navigate to the directory <em>Exercise_3</em></li>
    <li>Select DEM.tif</li>
    <li> Click Load<br><br>
    <img src="{{BASE_PATH}}{{ASSET_PATH}}/images/dem-load.png" class="screen-shot" /></li>
  </ol>
<h4>Project the DEM </h4>
<p>The DEM uses the WGS 84 coordinate system, which means it is measured in degrees. Since our project is in Washington State Plane which is measured in meters, we need to reproject the DEM to use it in our analysis.</p>
  <ol>
    <li>In the top menu, go to <b>Raster > Projections > Warp (Reproject)</b></li>
    <li>Use the following options
    <ul>
      <li>Input File: <b>DEM</b></li>
      <li> Output File: <b>dem_project.tif</b></li>
      <li>Source SRS: <b>EPSG:4326</b></li>
      <li>Resampling Method: <b>Near</b></li>
      <li>No data values: <b>0</b></li>
    </ul>
    <img src="{{BASE_PATH}}{{ASSET_PATH}}/images/project-dem.png" class="screen-shot" /><br></li>
  </ol>
<h4> Clip the dem to the state of Washington</h4>
<p>Clipping the dem will remove any values outside of the study area, which will make the analysis more efficient.</p>
  <ol>
    <li>In the top menu, select <b>Raster > Extraction > Clipper</b></li>
    <li> In the window, set the following options:
      <ul>
        <li> Input file: dem_project.tif</li>
        <li> Output file: WA_dem.tif</li>
        <li> No data value: 0</li>
        <li> Clipping Mode: Mask layer > washington</li>
        <li> Load into canvas when finished</li>
      </ul>
    <img src="{{BASE_PATH}}{{ASSET_PATH}}/images/dem-washington.png" class="screen-shot" /></li>
    <li> Remove the DEM layer</li>
  </ol>
  <p> The project area should now look like this:</p><br>
  <img src="{{BASE_PATH}}{{ASSET_PATH}}/images/dem-washington-display.png" class="screen-shot" />
<h4> Create a slope surface </h4>
  <ol>
    <li>In the top menu, select <b>Raster > Analysis > DEM</b>
    <ul>
      <li>Input file: <b>WA_dem</b></li>
      <li>Output file: <b>WA_slope.tif</b></li>
      <li>Mode: <b>Slope</b></li>
      <li>Scale: <b>0.30</b></li>
      <li>Load into canvas when finished</li>
    </ul>
    </li>
  </ol>
  <img src="{{BASE_PATH}}{{ASSET_PATH}}/images/create-slope.png" alt="create-slope" class="screen-shot"/><br>
<h4>Create a hillshade layer</h4>
<ol>
  <li>In the top menu, select <b>Raster > Analysis > DEM</b>
  <ul>
    <li>Input file: <b>WA_dem</b></li>
    <li>Output file: <b>WA_hillshade.tif</b></li>
    <li>Mode: <b>Hillshade</b></li>
    <li>Z factor: <b>1.0</b></li>
    <li>Scale: <b>0.30</b></li>
    <li>Azimuth of light: <b>315.0</b></li>
    <li>Altitude of light: <b>45.0</b></li>
    <li>Load into canvas when finished</li>
  </ul>
  </li>
</ol>
  <img src="{{BASE_PATH}}{{ASSET_PATH}}/images/hillshade-calc.png" alt="hillshade-calc" title="" />



<h4>Load the Geology Layer</h4>
  <ol>
    <li>Open the iRods plugin</li>
    <li>Load the WA_geology.shp file<br>
      <img src="{{BASE_PATH}}{{ASSET_PATH}}/images/lesson-1/lesson-1-01.png" alt="QGIS: Add geology vector layer into map view" class="screen-shot" />
    </li>
  </ol>
<h4>Add Rock strength index</h4>
<p>We are going to create a numeric field to represent the strength of the geologic unit</p>
<ol>
  <li>Open field calculator in Attribute Table:
    <ul>
      <li>Right-click <b>WA_geology</b> in <em>Layers List > Open Attribute Table > Toggle editing mode (top left)</em></li>
      <li>While in edit mode,click <b>Open field calculator (top right)</b><br>
        <img src="{{BASE_PATH}}{{ASSET_PATH}}/images/lesson-1/lesson-1-03.png" alt="QGIS: Open field calcultor" class="screen-shot" />
      </li>
    </ul></li>
  <li>Add and calculate new field<br>
    Match your Field Calculator window to the one below for creating a new field <b>strength</b> with a range of values of 0-3. Be sure to match it exactly as it's displayed
    <br>Note: <em>double-quotes are evaluated as a column whereas single-quotes are evaluated as column values. Save your changes once finished</em>.<br>
    Expression: <br>
    <code>
      
      case<br>
      &nbsp;&nbsp;&nbsp;&nbsp;when "rock_type" = 'soft' then 1<br>
      &nbsp;&nbsp;&nbsp;&nbsp;when "rock_type" = 'medium' then 2<br>
      &nbsp;&nbsp;&nbsp;&nbsp;when "rock_type" = 'hard' then 3<br>
      else 0<br>
      done
     </code><br>
    <img src="{{BASE_PATH}}{{ASSET_PATH}}/images/lesson-1/lesson-1-04.png" alt="QGIS: Add and calculate new field" title="" /></li>
</ol>
<h4>Visualize the geology by strength</h4>
<ol>
  <li>Right-click on WA_geology.shp in <b>Layer List > Properties > Style</b></li>
  <li>Change style type from <em>Single Symbol</em> to <em>Categorized</em>.</li>
  <li>Categorize by <em>strength</em> and click <em>Classify</em> to add classes.</li>
  <li>Match your colors with the example below or get fancy with your own styling.<br>
    <img src="{{BASE_PATH}}{{ASSET_PATH}}/images/lesson-1/lesson-1-07.png" alt="QGIS: Style shapefile" title="" /></li>
</ol>
<h4>Rasterize the rock strength layer</h4>
  <p>GDAL's rasterize is a prefered method. This can be accessed through <em>Menu Bar > Raster > Conversion > Rasterize (Vector to raster)</em>. Notice how the parameters have been diabled in the example below. There's a specific reason for this. Complete the parameter inputs: <em>Define your output file and location (in working directory); Attribute field = strength; Raster size in pixels = 1000 x 1000</em>. Because GDAL is more powerful in the commandline, there are certain parameters that cannot be set within QGIS.</p>
<ol>
  <li>Click the edit icon (pencil) locate near the block of code</em>. This enables you to add custom parameters.</li>
  <li>Add the line of code 
    <code>-at -a_nodata 0</code> just after <code>gdal_rasterize</code><br>
    The <code>-at</code> flag enables the ALL_TOUCHED rasterization option so that all pixels touched by lines or polygons will be updated, not just those on the line render path, or whose center point is within the polygon. Defaults to disabled for normal rendering rules.[^3]</li>
  <li><code>-a_nodata 0 </code> The above snippet specifies that 0 should be recognized as a missing or null value, which prevents pixels with 0 value from being visualized or used in analysis in the output raster.[^3]</li>
  <img src="{{BASE_PATH}}{{ASSET_PATH}}/images/lesson-1/lesson-1-11-2.png" class="screen-shot" /></li>
</ol>
<h4>Style the new Rock Strength Raster</h4>
<ol>
  <li>Open WA_geology.tif style properties. HINT: See step 4.</li>
  <li>For <b>Render type</b>, select <b>Singleband pseudocolor</b></li>
  <li>On the right select: 
  <ul>
    <ul>
      <li>Load min/max values: Min/max; </li>
      <li>Extend: Actual (slower); then Load</em>.</li>
    </ul>
  This loads actual values into the classification. QGIS Approximates the Minimum and Maximum values by default. </li>
  <li><em>Change Mode to Equal Interval with 3 classes</em>. Remember, we only have three rock strength classes: soft, medium, hard. Change your color map if you'd like. The color maps are not stored into the raster, so the raster will load with the default color map (black to white) each time its opened in a new project.</li>
  <li>Click <em>Classify</em>. Apply and OK.<br>
    <img src="{{BASE_PATH}}{{ASSET_PATH}}/images/lesson-1/lesson-1-13.png" alt="QGIS: Style raster" title="" /></li>
</ol>

<hr />

<h3>Data Analysis</h3>

<p>Reclassifying inputs:</p>

<h4>Slope</h4>

<table class="rules-table" style="width=30%">
  <col width="90" style="text-align:center;">
  <col width="50" style="text-align:center;">
  <tr>
    <th>Category</th>
    <th>Slope</th>
  </tr>
  <tr>
    <td>1</td>
    <td>&lt; 3&deg;</td>
  </tr>
  <tr>
    <td>2</td>
    <td>3-5&deg;</td>
  </tr>
  <tr>
    <td>3</td>
    <td>5-10&deg;</td>
  </tr>
  <tr>
    <td>4</td>
    <td>10-15&deg;</td>
  </tr>
  <tr>
    <td>5</td>
    <td>15-20&deg;</td>
  </tr>
  <tr>
    <td>6</td>
    <td>20-30&deg;</td>
  </tr>
  <tr>
    <td>7</td>
    <td>30-40&deg;</td>
  </tr>
    <td>8</td>
    <td>&gt; 40&deg;</td>
  </tr>
</table>



<p>In order to reclass our slope layer into something useful, we will use the grass <em>r.reclass</em> tool</p>

<p>The r.reclass tool reads text files which define the classification rules(rule files). For this example, our rule fill will have the following contents:</p>

<h5><em>slope-rule.txt</em></h5>

<p><code>
0 thru 2.99 = 1<br>
3 thru 4.99 = 2<br>
5 thru 9.99 = 3<br>
10 thru 14.99 = 4<br>
15 thru 19.99 = 5<br>
20 thru 29.99 = 6<br>
30 thru 39.99 = 7<br>
40 thru 90 = 8
</code></p>

<p>Once we have created the slope rule file. We can select the r.reclass tool from the Processing Toolbox.</p>

<p>In the processing toolbox, select <em>GRASS commands</em> > <em>Raster</em> > <em>r.reclass</em></p>

<p>We will use the following settings for the reclassify tool:<br></p>

<ul>
<li>Input Raster: <em>WA_slope</em></li>
<li>File containing reclass rules: <em>slope-rules.txt</em></li>
<li>Output raster layer: <em>slope-reclass.tif</em> (NOTE: select <em>Save to file...</em> or else QGIS will not create a permanent output)<br>
<img src="{{BASE_PATH}}{{ASSET_PATH}}/images/slope-reclass.png" alt="slope-reclass" title="" /></li>
</ul>

<h4>Susceptibility</h4>

<p>We will have to do some raster calculations to create the output data set. The classifications are taken from the California study[^2] and are as follows:</p>

<p><img src="{{BASE_PATH}}{{ASSET_PATH}}/images/landslide-key.png" alt="landslide-key" title="" /></p>

<p>Since we need to cross reference these categories, we need to create a raster which represents each unique combination of the slope and rock strength classes. A method to create unique combinations is to promote the rock strength categories a decimal place. This would make the rock classes: 10,20, and 30.</p>

<p>To create these new classes, we can load the <em>WA_geology.tif</em>, and slope-reclass.tif</p>

<p>In the top menu, select <em>Raster</em> > <em>Raster Calculator</em></p>

<p>Our raster calculator expression will be: </p>

<p><code>
("geo_coded@1" * 10) + "slope-reclass@1"
</code></p>

<p>Output layer: slope-geo-sect.tif</p>

<p><img src="{{BASE_PATH}}{{ASSET_PATH}}/images/raster-calc.png" alt="raster-calc" title="" /></p>

<p>We can apply the landslide susceptibility classification using r.reclass and the following rule file:</p>

<p><em>landslide-rule.txt</em></p>

<p><code>
11 12 13 21 31 = 0<br>
14 = 3<br>
22 23 = 5<br>
15 = 6<br>
16 32 33 = 7<br>
17 18 24 = 8<br>
25 thru 28 34 = 9<br>
35 thru 38 = 10<br>
</code></p>

<h3>Continue to SHOW YOUR RESULTS</h3>


[^1]: http://www.usgs.gov/blogs/features/usgs_top_story/landslide-in-washington-state
[^2]: http://www.conservation.ca.gov/cgs/information/publications/Documents/MS58.pdf
[^3]: http://www.spatialreference.org/ref/epsg/2927
[^4]: https://lta.cr.usgs.goc/GTOPO30
[^5]: http://www.dnr.wa.gov/ResearchScience/Topics/GeosciencesData/Pages/gis_data.aspx
[^6]: http://wdfw.wa.gov/conservation/gap/land_cover_data.html
[^7]: http://www.esrl.noaa.gov/thredds/catalog/Datasets/cpc_us_precip/catalog.xml#Datasets/cpc_us_precip/RT
[^8]: http://gadm.org/country
