---
title: "Spatial Analysis"
layout: post
category : Ask some questions
tagline: "Supporting tagline"
tags : [spatial functions]
---

{% include JB/setup %}

####**Pre-requisites:** Intro to Spatial Data, Data Formatting

####**Objectives:**
  - Recognize a few spatial relationships
  - Understand how to clip a raster  

----

# Spatial Functions

## Relational Operations

Relational operators are a set of methods used to test for the existence of a specified toplogical spatial relationship between two geometric objects as they would be represented on a map. The basic approach to comparing towo geometric objects is to project the objects onto the 2D horizonal coordinate reference system representing the Earth's surface, and then make pair-wise test of the intersections between the interiors, boundaries and exteriors of the twp projections and to classify the map relationship between the two geometric objects based on the entries in the resulting 3 by 3 intersection matrix.[^1] 


<table border="0" align="center">
  <tr>
    <td></td>
    <td align="center">
      <table border="0" summary="manufactured viewport for HTML img" cellspacing="0" cellpadding="0">
        <tr>
          <td align="center" valign="middle"><i>b</i> &#160; <a href="/wiki/File:De9im04.png" class="image"><img alt="De9im04.png" src="//upload.wikimedia.org/wikipedia/commons/e/e2/De9im04.png" width="100" height="100" data-file-width="100" data-file-height="100" /></a></td>
        </tr>
      </table>
    </td>
  </tr>
  <tr>
    <td align="center" valign="middle">
      <table border="0" summary="manufactured viewport for HTML img" cellspacing="0" cellpadding="0">
        <tr>
          <td align="center" valign="middle"><i>a</i><br />
            <a href="/wiki/File:De9im03.png" class="image"><img alt="De9im03.png" src="//upload.wikimedia.org/wikipedia/commons/c/c3/De9im03.png" width="100" height="100" data-file-width="100" data-file-height="100" /></a>
          </td>
        </tr>
      </table>
    </td>
    <td>
      <table border="0">
        <tr>
          <th align="center"></th>
          <th align="center"><strong>Interior</strong></th>
          <th align="center"><strong>Boundary</strong></th>
          <th align="center"><strong>Exterior</strong></th>
        </tr>
        <tr>
          <td align="center"><strong>Interior</strong></td>
          <td align="center" style="border-right: 2px solid #BCB; border-bottom: 2px solid #BCB;">
            <a href="/wiki/File:De9im05.png" class="image"><img alt="De9im05.png" src="//upload.wikimedia.org/wikipedia/commons/4/44/De9im05.png" width="100" height="100" data-file-width="100" data-file-height="100" /></a>
            <p>&#160; <i>dim</i>[<span style="font-family: monospace, monospace;">&#123;&#123;&#123;1&#125;&#125;&#125;</span>] = <b>2</b> &#160;</p>
          </td>
          <td align="center" style="border-right: 2px solid #BCB; border-bottom: 2px solid #BCB;">
            <a href="/wiki/File:De9im06.png" class="image"><img alt="De9im06.png" src="//upload.wikimedia.org/wikipedia/commons/9/9b/De9im06.png" width="100" height="100" data-file-width="100" data-file-height="100" /></a>
            <p>&#160; <i>dim</i>[<span style="font-family: monospace, monospace;">&#123;&#123;&#123;1&#125;&#125;&#125;</span>] = <b>1</b> &#160;</p>
          </td>
        <td align="center" style="border-bottom: 2px solid #BCB;">
          <a href="/wiki/File:De9im07.png" class="image"><img alt="De9im07.png" src="//upload.wikimedia.org/wikipedia/commons/0/03/De9im07.png" width="100" height="100" data-file-width="100" data-file-height="100" /></a>
          <p>&#160; <i>dim</i>[<span style="font-family: monospace, monospace;">&#123;&#123;&#123;1&#125;&#125;&#125;</span>] = <b>2</b> &#160;</p>
        </td>
      </tr>
      <tr>
        <td align="center"><span class="bold"><strong>Boundary</strong></span></td>
        <td align="center" style="border-right: 2px solid #BCB; border-bottom: 2px solid #BCB;">
          <div class="informalfigure">
            <div>
              <a href="/wiki/File:De9im08.png" class="image"><img alt="De9im08.png" src="//upload.wikimedia.org/wikipedia/commons/7/76/De9im08.png" width="100" height="100" data-file-width="100" data-file-height="100" /></a>
            </div>
          </div>
          <p>&#160; <i>dim</i>[<span style="font-family: monospace, monospace;">&#123;&#123;&#123;1&#125;&#125;&#125;</span>] = <strong>1</strong> &#160;</p>
        </td>
        <td align="center" style="border-right: 2px solid #BCB; border-bottom: 2px solid #BCB;">
          <div class="informalfigure">
            <div>
              <a href="/wiki/File:De9im09.png" class="image"><img alt="De9im09.png" src="//upload.wikimedia.org/wikipedia/commons/b/b3/De9im09.png" width="100" height="100" data-file-width="100" data-file-height="100" /></a>
            </div>
          </div>
          <p>&#160; <i>dim</i>[<span style="font-family: monospace, monospace;">&#123;&#123;&#123;1&#125;&#125;&#125;</span>] = <strong>0</strong> &#160;</p>
        </td>
        <td align="center" style="border-bottom: 2px solid #BCB;">
          <div class="informalfigure">
            <div>
              <a href="/wiki/File:De9im10.png" class="image"><img alt="De9im10.png" src="//upload.wikimedia.org/wikipedia/commons/1/1d/De9im10.png" width="100" height="100" data-file-width="100" data-file-height="100" /></a>
            </div>
          </div>
          <p>&#160; <i>dim</i>[<span style="font-family: monospace, monospace;">&#123;&#123;&#123;1&#125;&#125;&#125;</span>] = <strong>1</strong> &#160;</p>
        </td>
      </tr>
      <tr>
        <td align="center"><span class="bold"><strong>Exterior</strong></span></td>
        <td align="center" style="border-right: 2px solid #BCB;">
          <div class="informalfigure">
            <div>
              <a href="/wiki/File:De9im11.png" class="image"><img alt="De9im11.png" src="//upload.wikimedia.org/wikipedia/commons/7/73/De9im11.png" width="100" height="100" data-file-width="100" data-file-height="100" /></a>
            </div>
          </div>
          <p>&#160; <i>dim</i>[<span style="font-family: monospace, monospace;">&#123;&#123;&#123;1&#125;&#125;&#125;</span>] = <strong>2</strong> &#160;</p>
        </td>
        <td align="center" style="border-right: 2px solid #BCB;">
          <div class="informalfigure">
            <div>
              <a href="/wiki/File:De9im12.png" class="image"><img alt="De9im12.png" src="//upload.wikimedia.org/wikipedia/commons/6/63/De9im12.png" width="100" height="100" data-file-width="100" data-file-height="100" /></a>
            </div>
          </div>
          <p>&#160; <i>dim</i>[<span style="font-family: monospace, monospace;">&#123;&#123;&#123;1&#125;&#125;&#125;</span>] = <strong>1</strong> &#160;</p>
        </td>
        <td align="center">
          <div class="informalfigure">
            <div>
              <a href="/wiki/File:De9im13.png" class="image"><img alt="De9im13.png" src="//upload.wikimedia.org/wikipedia/commons/c/ca/De9im13.png" width="100" height="100" data-file-width="100" data-file-height="100" /></a>
            </div>
          </div>
          <p>&#160; <i>dim</i>[<span style="font-family: monospace, monospace;">&#123;&#123;&#123;1&#125;&#125;&#125;</span>] = <strong>2</strong> &#160;</p>
        </td>
      </tr>
    </table>
  </td>
</tr>
</table>

----

### Spatial Predicates
The above figure demonstrates the criteria outlined by the Dimensionally Extended Nine-Intersection Model(DE-9IM). The matrix is used to outline specific spatial relationships or predicates which are more accessible to interpretaion. These named spatial predicates are also used by spatial queryies to selet subsets of geometrys that fit in one spatial predicate. 

Here is a quick outline of the Spatial Predicates

Equals
  : The two geometric objects are spatially equal meaning, they represent the same geometry (congruent in geometric terms)

Disjoint
  : The two geometric objects share no points in common, the two objects are completely separate

Intersects 
  : The two geometric objects have at least one point in common

Touches
  : The two geometric objects have at least one boundary point in common, but no interior points

Crosses
  : The two geometric objects share some but not all interior points

Contains
  : One geometry is contained by the other if all the points of one geometry are within the boundary or interior of the other and they share at least one interior point. This means that this predicate may produce unpredictable behavior for linestrings and points which have no interior points.[^2]

Within
  : The inverse of Contains. The first geometry is completely within the second geometry

Overlaps
  : The two geometries share some but not all points in common, the two geometries have the same dimensions and the intersection of the two geometries has the same dimension as the two input geometries

Covers
  : Similar to contains, but does not require that the two geometries share an interior point, which makes it the proper tool for working with inputs of different dimensions.

----

### Methods for Spatial Analysis

There are a few standard methods for geometric analysis available based on the geoetric object model. This is a brief listing of basic analyses and is by no means exhaustive of the available functions.

Distance
  : Measures the shortest distance between two geometries. 

Buffer
  : Creates a geometric object that represents all points within a given distance from the given geometry

Convex Hull
  : Creates a geometric object that represent the minimum bounding convex polygon that contains all points of the given geometry

Intersection
  : Returns a geometric object that represents the intersecting parts of two given geometries

Union
  : Creates a geometric object that represents the combination of two given geometries

Difference
  : Returns the given geometries that do not intersect. If all gemetries intersect, an empty set will be returned.

Symmetric Difference
  : Creates a geometric object of the disjoint parts of two geometries. The output will have all pieceheld by one geometry or the other, but not the areas held by both.

----

#### Examples

* Intersection
  - All rivers within washington state boundary
  - Residential areas with past landslide

* Union 
  - Merging census data for two states in a single layer (Useful for studying phenomena that do not respect political boundaries)
  - Merging state boundaries for Canada, U.S. and Mexico into a North American dataset (GADM makes this a breeze)

* Buffer 
  - Instagram posts vs within 100m of a local restaurant
  - Tweets vs within 1km of a football stadium

* Convex Hull 
  - Bounding boxes, circles
  - Range modeling for small data sets

----

### Raster Functions

  * Slope - This function will calculate a slope surface given a raster holding an elevation model
  * Aspect - This outputs a surface holding values 0-360 referencing the azimuth of the given surface
  * Raster Calculator - Performs a designated mathematical equation on all values within a raster. This can also be used to combine/difference rasters.
  * Interpolation - Useful for resurfacing a dataset. This can also be used to create a continuous surface from a point dataset.
  * Reclassify - Useful for creating discrete value categories from continous values(e.g.float data) also useful for creating presence absence datasets.
  * Polygonize - Creates a set of polygon features representing the given raster dataset
  * Histogram - Create a histogram showing the distribution of values in a raster dataset

<br>

----

## Exercise

The following exercises are a continuation of the previous exercises. Open your project workspace before starting the following steps. See [Data Wrangling]({{BASE_PATH}}/data%20wrangling/data-wrangling).

8. Clip the dem to the state boundary
  * In the top menu, select Raster > Extraction > Clipper
  * In the window, set the following options:
    + **Input file: dem-project.tif**
    + **Output file: dem-washington.tif**
    + **No data value: 0**
    + **Clipping Mode: Mask layer > washington**
    + Load into canvas when finished<br>
  <img alt="demWashington" src="{{BASE_PATH}}{{ASSET_PATH}}/images/dem-washington.png" class="screen-shot" />
9. Remove the dem-project.tif layer<br>
  <img alt="dem-washington-display" src="{{BASE_PATH}}{{ASSET_PATH}}/images/dem-washington-display.png" class="screen-shot" /><br>
10. Create a slope surface <br>
  In the top menu, select **Raster > Analysis > DEM** <br>
  * Input file: dem-washington
  * Output file: slope-washington.tif
  * Mode: Slope
  * Scale: 0.30
  * Load into canvas when finished<br>
  <img alt="create-slope" src="{{BASE_PATH}}{{ASSET_PATH}}/images/create-slope.png" class="screen-shot" /><br>
11. Create a hillshade layer<br>
  In the top menu, select **Raster > Analysis > DEM** <br>
  * Input file: dem-washington
  * Output file: hillshade-washington.tif
  * Mode: Hillshade
  * Z factor: 1.0
  * Scale: 0.30
  * Azimuth of light: 315.0
  * Altitude of light: 45.0
  * Load into canvas when finished<br>
  <img alt="hillshade-calc" src="{{BASE_PATH}}{{ASSET_PATH}}/images/hillshade-calc.png" class="screen-shot" /><br>
12. Save your project workspace.

<br>

----

<br>

[^1]: SQL Simple Features Specification
[^2]: JTS


* [^3] : Esri
* [^4] : PostGIS Reference Manual.  <a href="//postgis.org/documentation/manual-svn/using_postgis_dbmanagement.html">http://postgis.org/documentation/manual-svn/using_postigs_dbmanagement.html</a>
