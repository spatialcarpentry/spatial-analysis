---
title: "Landslide Exercise"
layout: post
category : Ask some Questions
tagline: ""
tags : [case study, landslide]
---

{% include JB/setup %}


#### Pre-requisites:

1. Setup
2. Basics
3. Data Hygiene

#### Objectives:

- Prep data for a spatial analysis
- Perform a spatial analysis utilizing both vector and raster data
- Visualize results from the spatial analysis

#### Exercise: Landslide Susceptibility Model


<!--
#### Data:

Data for the following exercises was prepared in [Spatial Functions](http://localhost:4000/ask%20some%20questions/functions), lesson 5.1. Data is also prepared and available below:

iRods access: <br>&nbsp;&nbsp;&nbsp;``/iplant/home/shared/aegis/Spatial-bootcamp/spatial-analysis/landslide-exercise``

- [Washington geology (wa_geology.shp)](link-here)
- [Washington slope (wa_slope.tif)](link-here)
-->

----

## Landslide Susceptibility Model

### Overview

This case study has been chosen to demonstrate the concepts being highlighted by the bootcamp. We have chosen a landslide susceptibility analysis due to the diversity of inputs and a general curiousity into the methodology. The study area has been chosen in response to the Oso Mudslide[^1] which took place in Washington State on March 22nd, 2014. The methodology for this analysis is based off of work done by the California Geological Survey in Conjunction with the USGS[^2].

----
<!--
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

-->

   
<h4>1. Load the Geology Layer</h4>
  <ol>
    <li>Open QGIS and first set the project projection to ESPG:2927. This will ensure we have the correct projection after importing layers. It's recommended to download and unzip the wa_geology layer due to its larger file size.<br><br></li>
    <ol>
    <li>Load wa_geology.shp through the iRods plugin:<br>&nbsp;&nbsp;&nbsp;<code>/iplant/home/shared/aegis/Spatial-bootcamp/spatial-analysis/landslide-exercise/wa_geology.shp</code></li>
    <li>Or download, unzip, and <strong>Add Vector Layer</strong> <img src="{{BASE_PATH}}{{ASSET_PATH}}/images/add-vector.png"/>:<br><a href="http://de.iplantcollaborative.org/dl/d/433E2335-1732-4BD3-9AA4-CDF08B7D7020/wa_geology.zip">wa_geology.zip</a><br><br></li>
    </ol>
    <img data-featherlight="{{BASE_PATH}}{{ASSET_PATH}}/images/landslide-1.png" src="{{BASE_PATH}}{{ASSET_PATH}}/images/landslide-1.png" alt="QGIS: Add geology vector layer into map view" class="screen-shot" />
    </li>
  </ol>
<h4>2. Inspect Washington Geology Atrributes</h4>
<p>If you open the Washington geology attributes you'll notice there's a column 'rock_type' with values as <a href="http://en.wikipedia.org/wiki/String_%28computer_science%29" target="_blank">string</a>. Our landslide susceptibility model requires that we have a numeric value representing 'rock_type'. Notice the range of 'rock_type': soft, medium, hard. Wow that's an easy one! It's easy as 1, 2, 3 (literally). You're welcome for the heavy lifting. See 'geo_unit' for actual geologic unit types.</p>
<p>We are going to add a new field to the wa_geology shapefile to represent the strength of the geologic unit. This new field data type will be <em>number</em>, not a <a href="http://en.wikipedia.org/wiki/String_%28computer_science%29" target="_blank">string</a>. This will allow us to perform arithmetic opertions later on.<br><br><strong>Think about it</strong>: the value '1' represented as a <a href="http://en.wikipedia.org/wiki/String_%28computer_science%29" target="_blank">string</a> does not equal the numeric value of 1. In this case, an <a href="http://en.wikipedia.org/wiki/Integer_%28computer_science%29">integer</a>.<br><br></p>
<p><img data-featherlight="{{BASE_PATH}}{{ASSET_PATH}}/images/landslide-3.png" src="{{BASE_PATH}}{{ASSET_PATH}}/images/landslide-3.png" alt="Spatial Data Bootcamp: QGIS check geology"/></p>
<h4>3. Add Rock strength index</h4>
<ol>
  <li>Open field calculator in Attribute Table and enter Edit Mode:<br><br>
  <strong>Reminder:</strong> Making changes to your data in <strong>Edit Mode</strong> are <em>permanent</em>. It's good practice to store <em>source</em> data, or the original files in a separate directory so in the event you make a catastrophic mistake you don't have to ask your colleague, boss, or whomever for another copy of the data.<br><br>
    <ol>
      <li><em>Right-click <b>wa_geology</b> (Layers List) > Open Attribute Table > Toggle editing mode (top left)</em> <img src="{{BASE_PATH}}{{ASSET_PATH}}/images/toggle-edit.png"/></li>
      <li>While in edit mode,click <b>Open field calculator (top right)</b> <img src="{{BASE_PATH}}{{ASSET_PATH}}/images/field-calculator.png"/><br><br>
      Whil in <strong>Edit Mode</strong>  the Attribute table icons will be enabled: Save, Delete, New Column.<br><br>
      Notice the shapefile has turned red on the map canvas. You can also edit the shapefile <em>nodes</em> (e.g. coordinates = point = node), changing the <em>geometry</em> of the file, since all we are really dealing with are <em>numbers</em>. We are simply <em>visualizing</em> these numbers (lat/long) and their geometry.<br><br>
        <img data-featherlight="{{BASE_PATH}}{{ASSET_PATH}}/images/landslide-2.png" src="{{BASE_PATH}}{{ASSET_PATH}}/images/landslide-2.png" alt="QGIS: Open field calcultor" class="screen-shot" />
      </li>
    </ol></li>
  <li>Add and calculate new field:<br>
    Be sure to match your Field Calculator window to the one below for creating a new field <b>strength</b> with a range of values of 0-3; if rock_type is not soft, medium, or hard then assign 0 to the value. Be sure to match it exactly as it's displayed
    <br><br>Note: <em>double-quotes are evaluated as a column whereas single-quotes are evaluated as column values. Save your changes once finished</em>.<br><br>
    Configure <strong>Field Calculator</strong> inputs as follows:<br>
    Create a new field: (checked)<br>
    Output field name: <strong>strength</strong><br>
    Output field type: <strong>Whole number</strong> (integer)<br>
    Output field width: <strong>1</strong> (we only need single-digit integers)<br>
    <strong>Expression:</strong> <br>
    <p style="padding-left: 30px;">
    Copy and paste will work for entering the code into the <strong>Expression</strong> field.<br><br>
       <code>
      
      case<br>
      &nbsp;&nbsp;&nbsp;&nbsp;when "rock_type" = 'soft' then 3<br>
      &nbsp;&nbsp;&nbsp;&nbsp;when "rock_type" = 'medium' then 2<br>
      &nbsp;&nbsp;&nbsp;&nbsp;when "rock_type" = 'hard' then 1<br>
      else 0<br>
      end
     </code><br><br>
     </p>
     This field calculation will take a couple of seconds. We're evaluating 57,657 rows.<br><br>
    <img data-featherlight="{{BASE_PATH}}{{ASSET_PATH}}/images/landslide-4.png" src="{{BASE_PATH}}{{ASSET_PATH}}/images/landslide-4.png" alt="QGIS: Add and calculate new field" title="" /></li>
  <li>Confirm field calculation of 'strength':<br><br>
  Be sure to <strong>Save Edits</strong> <img src="{{BASE_PATH}}{{ASSET_PATH}}/images/save-edits.png"/> then <strong>Disable Editing</strong> <img src="{{BASE_PATH}}{{ASSET_PATH}}/images/toggle-edit.png"/> before continuing. Your geology layer will no longer be red (Edit Mode).<br><br><img data-featherlight="{{BASE_PATH}}{{ASSET_PATH}}/images/landslide-5.png" src="{{BASE_PATH}}{{ASSET_PATH}}/images/landslide-5.png" alt="Spatial Data Bootcamp: QGIS - check calculation"/>
  </li>
</ol>
<h4>4. Visualize the geology by strength</h4>
<p>Let's visualize the new field we have just calculated.</p>
<ol>
  <li>Open <strong>wa_geology</strong> Style properties: <em>Right-click wa_geology (Layer list) > Properties > Style</em></li>
  <li>Change style type from <strong>Single Symbol</strong> <img src="{{BASE_PATH}}{{ASSET_PATH}}/images/single-symbol.png"/> to <br><strong>Categorized</strong> <img src="{{BASE_PATH}}{{ASSET_PATH}}/images/categorized.png"/>.</li>
  <li>Categorize by <em>strength</em> and click <em>Classify</em> to add classes.</li>
  <li>Match your colors with the example below or get fancy with your own styling.<br>
  <br>There's a category with no Value. You can leave this one there or highlight and delete it.<br><br>
  Be sure to <strong>APPLY</strong> to make changes then <strong>OK</strong> to close the Properties window<br><br>
    <img data-featherlight="{{BASE_PATH}}{{ASSET_PATH}}/images/landslide-6.png" src="{{BASE_PATH}}{{ASSET_PATH}}/images/landslide-6.png" alt="QGIS: Style shapefile" title="" /><br><br>Drum roll...<br><br>Reminder: Red = soft geology (land) = higher risk of landslides<br><br><img data-featherlight="{{BASE_PATH}}{{ASSET_PATH}}/images/landslide-7.png" src="{{BASE_PATH}}{{ASSET_PATH}}/images/landslide-7.png" alt="Spatial Data Bootcamp: QGIS visualize strenght"/></li>
</ol>
<h4>5. Rasterize the rock strength layer</h4>
<p>To be able to perform the landslide susceptibility model we must intersect rock strength with slope (derrived from a DEM). So the best (highly suggested) method is converting our geology shapefile into a raster. A simple process thanks to <a href="http://www.gdal.org/gdal_rasterize.html" target="_blank">GDAL_rasterize</a>.</p>
  <p>Because GDAL is more powerful in the commandline, there are certain parameters that cannot be set within QGIS.</p>
<ol>
<li>Open <strong>gdal_rasterize</strong>: <em>Menu Bar > Raster > Conversion > Rasterize (Vector to raster)</em></li>
<li>Configure inputs as follows:<br>
Input file: <strong>wa_geology</strong><br>
Attribute filed: <strong>strength</strong> (strength value assigned to pixels)<br>
Output file for rasterized vectors (raster): <strong>wa_geo_coded</strong><br>
<span style="padding-left:20px;"><em>"The output file doesn't exist. You must set up the output size or resolution to create it:"</em> <strong>OK</strong></span><br>
Raster size in pixels: <strong>3280 x 3280</strong> (1km resolution)<br>
Enable editing the gdal_rasterize code by clicking the <strong>Edit</strong> icon <img src="{{BASE_PATH}}{{ASSET_PATH}}/images/gdal-edit.png"/> near the block of code. This allows to add custom parameters.<br><br>
Add the line of code: <code>-at -a_nodata 0</code><br><br>
<img date-featherlight="{{BASE_PATH}}{{ASSET_PATH}}/images/gdal-rasterize-1.png" src="{{BASE_PATH}}{{ASSET_PATH}}/images/gdal-rasterize-1.png" alt="Spatial Data Bootcamp: Gdal edit"/><br><br><img data-featherlight="{{BASE_PATH}}{{ASSET_PATH}}/images/gdal-rasterize-2.png" src="{{BASE_PATH}}{{ASSET_PATH}}/images/gdal-rasterize-2.png" alt="Spatial Data Bootcamp"/><br><br>
The <code>-at</code> flag enables the ALL_TOUCHED rasterization option so that all pixels touched by lines or polygons will be updated, not just those on the line render path, or whose center point is within the polygon. Defaults to disabled for normal rendering rules.[^3]<br><br>
<code>-a_nodata 0 </code> The above snippet specifies that 0 should be recognized as a missing or null value, which prevents pixels with 0 value from being visualized or used in analysis in the output raster.[^3]<br><br>
Drum roll...<br><br>
<img data-featherlight="{{BASE_PATH}}{{ASSET_PATH}}/images/landslide-8.png" src="{{BASE_PATH}}{{ASSET_PATH}}/images/landslide-8.png" alt="Spatial Data Bootcamp"/>
Notice the light blue (water) above. We declared 0 as <code>no_data</code> so water is no longer included in our raster dataset.
</li>
</ol>
<h4>6. Style the new Rock Strength Raster</h4>
It's good to visually check your results, if any large issues may occur. Sometimes you must look at your data values to locate errors. Let's style our new geology raster.
<ol>
  <li>Open wa_geo_coded style properties.</li>
  <li>Render type: <b>Singleband pseudocolor</b></li>
  <li>Load actual values: 
  <ul>
      <li><b>Load min/max values: Min/max</b> </li>
      <li><b>Extent: Actual (slower)</b></li>
      <li><b>Click LOAD</b> to load actual values</li>
    </ul>
  This loads actual values into the classification. QGIS Approximates the Minimum and Maximum values by default. </li>
  <li><b>Change Mode to Equal Interval with 3 classes</b>.<br><br>Remember, we only have three rock strength classes: soft, medium, hard. The color maps are not stored into the raster, so the raster will load with the default color map (black to white) each time its opened in a new project.<br><br></li>
  <li>Click <b>Classify</b> to add categories.<br><br>
  <img data-featherlight="{{BASE_PATH}}{{ASSET_PATH}}/images/landslide-9.png" src="{{BASE_PATH}}{{ASSET_PATH}}/images/landslide-9.png" alt="Spatial Data Bootcamp"/></li>
  <li>Click <b>APPLY</b> to confirm changed and <b>OK</b> to exit properties.<br>
    <img data-featherlight="{{BASE_PATH}}{{ASSET_PATH}}/images/landslide-10.png" src="{{BASE_PATH}}{{ASSET_PATH}}/images/landslide-10.png" alt="QGIS: Style raster" title="" /></li>
</ol>

<h4>7. Prepare slope for analysis:</h4>
<p>Now it's time to perform an analysis. We're going to reclassify our slope into something useful.</p>

<ol>
<li>Import wa_slope.tif<br><br>
<p>This slope raster was created in the <b>Spatial Functions</b>. If you still have it, great. Otherwise the slope raster is available here:<br><br>
iRods access: &nbsp;&nbsp;&nbsp;<code>/iplant/home/shared/aegis/Spatial-bootcamp/spatial-analysis/landslide-exercise/wa_slope.tif</code></p>

<p>Or download file: <a href="http://de.iplantcollaborative.org/dl/d/28E8ADDB-CE25-4A8D-917D-7799803544FA/wa_slope.tif">wa_slope.tif</a></p>
<p><img data-featherlight="{{BASE_PATH}}{{ASSET_PATH}}/images/landslide-11.png" src="{{BASE_PATH}}{{ASSET_PATH}}/images/landslide-11.png" alt="Spatial Data Bootcamp"/></p>
</li>
<li>
<p>In order to reclass our slope layer into something useful, we will use the grass <em>r.reclass</em> tool</p>

<p>The r.reclass tool reads text files which define the classification rules (rule files). We must prepare a text file with the following contents using a <strong>text editor</strong> not Microsoft Word</p>

<b><em>Create slope-rule.txt:</em></b>
<p>Copy and paste the code below into a new file and save it as <b>slope-rule.txt</b></p>
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

<br>

<p>Reclassifying inputs definition - how to interpret slope-rule.txt:</p>

<b>Slope</b>

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
</li>
<li>
<p>Once we have created the slope-rule.txt file, we can select the <strong>r.reclass tool</strong> from the Processing Toolbox.</p>

<p>Open the Processing Toolbox: <em>Menu Bar > Processing > Toolbar</em></p> 

<p>And enable <strong>Advanced Interface</strong> by clicking the dropdowin list at the bottom of the Processing Toolbox:<br><br>
<img data-featherlight="{{BASE_PATH}}{{ASSET_PATH}}/images/landslide-12.png" src="{{BASE_PATH}}{{ASSET_PATH}}/images/landslide-12.png" alt="Spatial Data Bootcamp"/>
</p>

<p>From the Advanced Processing Toolbox, select <em>GRASS commands</em> > <em>Raster</em> > <em>r.reclass</em><br><br>
<img data-featherlight="{{BASE_PATH}}{{ASSET_PATH}}/images/landslide-13.png" src="{{BASE_PATH}}{{ASSET_PATH}}/images/landslide-13.png" alt="Spatial Data Bootcamp"/>
</p>
<br><br>
<p><pre><code><strong>If you receive this error, download the reclassified slope here: <a href="http://de.iplantcollaborative.org/dl/d/40368F38-0383-412A-B49A-DC4C1E86BC77/wa_slope_reclass.tif">wa_reclass_slope.tif</a><br><br>
Or to enable GRASS Provider: <em>Menu Bar > Processing > Options > Providers > (enable GRASS)</em><br><br>
Otherwise continue on.</strong><br><br>
This means you don't have GRASS installed or bad configuration.
<img data-featherlight="{{BASE_PATH}}{{ASSET_PATH}}/images/grass-error.png" src="{{BASE_PATH}}{{ASSET_PATH}}/images/grass-error.png" alt="Spatial Data Bootcamp"/>
</code></pre>
</p>
<p>Continuing on...</p>
<p>We will use the following settings for the reclassify tool:<br></p>

<ul>
<li>Input Raster: <b>WA_slope</b></li>
<li>File containing reclass rules: <b>slope-rules.txt</b></li>
<li>Output raster layer: <b>slope-reclass.tif</b> (NOTE: select <em>Save to file...</em> or QGIS will not create a permanent output)<br><br>
<img data-featherlight="{{BASE_PATH}}{{ASSET_PATH}}/images/landslide-14.png" src="{{BASE_PATH}}{{ASSET_PATH}}/images/landslide-14.png" alt="slope-reclass" title="" /><br><br>Your output should look similar to the input. However, your legend should read different categories.<br><br>The output layer it titled "Output raster layer" but is still saved to the hard drive. Rename this output raster to <strong>wa_slope_reclass</strong>.<br><br>
<img data-featherlight="{{BASE_PATH}}{{ASSET_PATH}}/images/landslide-15.png" src="{{BASE_PATH}}{{ASSET_PATH}}/images/landslide-15.png" alt="Spatial Data Bootcamp"/></li>
</ul>


</li>

</li>
</ol>




<h4>Susceptibility</h4>

<p>We will have to do some raster calculations to create the output data set. The classifications are taken from the California study[^2] and are as follows:</p>

<p><img src="{{BASE_PATH}}{{ASSET_PATH}}/images/landslide-key.png" alt="landslide-key" title="" /></p>

<p>Since we need to cross reference these categories, we need to create a raster which represents each unique combination of the slope and rock strength classes. A method to create unique combinations is to promote the rock strength categories a decimal place. This would make the rock classes: 10,20, and 30. This is done in <strong>Raster Calculator</strong>.</p>

<p>We will be using both <b>wa_geo_coded.tif</b>, and <b>wa_slope_reclass.tif</b></p>

<ol>

<li>
Open the <b>Raster Calculator</b>: <em>Menu Bar > Raster > Raster Calculator</em><br><br>

<p>Our raster calculator expression will be: </p>

<p><code>
("wa_geo_coded@1" * 10) + "wa_slope_reclass@1"
</code></p>

<p>Output layer: <b>slope_geo_sect.tif</b></p>

<p><img data-featherlight="{{BASE_PATH}}{{ASSET_PATH}}/images/landslide-16.png" src="{{BASE_PATH}}{{ASSET_PATH}}/images/landslide-16.png" alt="raster-calc" title="" /></p>

<br>

<p>View the new <b>slope_geo_sect.tif</b></p>

<p><img data-featherlight="{{BASE_PATH}}{{ASSET_PATH}}/images/landslide-17.png" src="{{BASE_PATH}}{{ASSET_PATH}}/images/landslide-17.png" alt="Spatial Data Bootcamp"/></p>
</li>

<li>
<p>We can apply the landslide susceptibility classification to <b>slope_geo_sect.tif</b> using <b>r.reclass</b> and the following rule file:</p>

<p><b>Create landslide-rule.txt:</b></p>

<p>Copy and pase the code into a new file and save it as <b>landslide-rule.txt</b></p>

<p><code>
11 12 13 21 31 = 0<br>
14 = 3<br>
22 23 = 5<br>
15 = 6<br>
16 32 33 = 7<br>
17 18 24 = 8<br>
25 thru 28 34 = 9<br>
35 thru 38 = 10
</code></p>

<br>

<p>
Open <b>r.reclass</b>: <em>Processing Toolbox > GRASS > Raster > r.reclass</em>
</p>

<p>Configure as follows:<br>
Input raster layer: <b>slope_geo_sect</b><br>
File containing reclass rules: <b>landslide-rule.txt</b><br>
Output raster layer: <b>wa_landslide_suscept.tif</b>
</p>

<p>Notice the projection of <b>slope_geo_sect</b> is <a href="http://spatialreference.org/ref/epsg/2286/" target="_blank">EPSG:2286</a>. This is <em>close</em> to <a href="http://spatialreference.org/ref/epsg/2927/" target="_blank">EPSG:2927</a>. <a href="http://www2.arnes.si/~gljsentvid10/datm_faq.html#1.2" target="_blank">Read here about the difference</a>. This is not a published analysis so we will move on.

<p>
<img data-featherlight="{{BASE_PATH}}{{ASSET_PATH}}/images/landslide-18.png" src="{{BASE_PATH}}{{ASSET_PATH}}/images/landslide-18.png" alt="Spatial Data Bootcamp"/>
</p>

</li>
<li>
<p>Our final product of landslide susceptibility within the entire state of Washington.</p>
<p>Style <b>wa_landslide_suscept</b>:<br><br>
Download style rules here: <a href="http://de.iplantcollaborative.org/dl/d/DB7A3C67-24F6-4E78-A332-5342BD87B5F2/wa_landslide_suscept.txt">wa_landslide_suscept.txt</a>
</p>
<p>Open <b>wa_landslide_suscept</b> (layer not text file) style properties and import the <b>wa_landslide_suscept.txt style rules:<br>
<ol>
<li>Switch the Render Type to <b>Singleband pseudocolor</b></li>
<li>Import the <b>wa_landslide_suscept.txt</b> style rule file <img src="{{BASE_PATH}}{{ASSET_PATH}}/images/open-style.png"/><br><br>
Your categories should look like the same as below:<br><br>
<img data-featherlight="{{BASE_PATH}}{{ASSET_PATH}}/images/landslide-19.png" src="{{BASE_PATH}}{{ASSET_PATH}}/images/landslide-19.png" alt="Spatial Data Bootcamp"/>

</p>
<p>The final map:<br><br>
<img data-featherlight="{{BASE_PATH}}{{ASSET_PATH}}/images/landslide-20.png" src="{{BASE_PATH}}{{ASSET_PATH}}/images/landslide-20.png" alt="Spatial Data Bootcamp"/>
</p>
</li>
</ol>
<p>We have just located areas susceptible to landslides within the state of Washginton!</p>
<hr>
<br>
References:
<ol>
<li> http://www.usgs.gov/blogs/features/usgs_top_story/landslide-in-washington-state</li>
<li> http://www.conservation.ca.gov/cgs/information/publications/Documents/MS58.pdf</li>
<li> http://www.spatialreference.org/ref/epsg/2927</li>
<li> https://lta.cr.usgs.goc/GTOPO30</li>
<li> http://www.dnr.wa.gov/ResearchScience/Topics/GeosciencesData/Pages/gis_data.aspx</li>
<li> http://wdfw.wa.gov/conservation/gap/land_cover_data.html</li>
<li> http://www.esrl.noaa.gov/thredds/catalog/Datasets/cpc_us_precip/catalog.xml#Datasets/cpc_us_precip/RT</li>
<li> http://gadm.org/country</li>
</ol>
