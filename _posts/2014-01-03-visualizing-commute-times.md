---
title: Visualizing commute times
layout: default
category: Visualizations
tagline: "using python to generate GIS data and render it in a pleasing manner"
tags: [python, gis, tableau]
---
## Commute times
I have been house hunting recently and one of the most important things for me is location. Many things can be changed about a house but location is (usually) not one of them.

Here is my [repo](https://github.com/mrtakos/lat_long_grid_generator)

### The goal
I found [this tool](http://www.trulia.com/local/) that Trulia offers that illustrates commute times from various near-by addresses to a point on the map. This is nifty but I would like to do more with this: like average two locations commute times or use public transportation commute times (which is availible in some locations on Trulia apparently).

### The method
I dont have thousands of local addresses but I can generate a grid of thousands of near by latitude and longitudes.

#### Code
{% highlight python %}
diameter = 75
incrementMax = 0.75
increment = incrementMax / diameter
# 47.840353,-122.589684 Port Gamble
# 47.6562513, -122.312971 UW Admissions building
centerLat = 47.6562513
centerLong = -122.312971
oLat = centerLat + (incrementMax / 2)
oLong = centerLong - (incrementMax / 2)
print 'Latitude,Longitude'

for x in range(diameter):
        for y in range(diameter):
                cLat = oLat - increment * x
                cLong = oLong + increment * y
                print '{0},{1}'.format(cLat,cLong)
{% endhighlight %}

![Points rendered across the Seattle area](/assets/img/Grid_of_points.jpg)

#### Water
Water is a problem. If you live in an area with a lot of water you will have a lot of points that arent worth looking up commute times for. The best methodology I know of for handeling this is to get a hydrography shape file from your local government website and use it to detect points in water polygons. I attempted to do this in python and I believe it to be fairly easy but I was lazy so I used ESRI's ArcMaps.

#### Scraping
From here you can loop through each of these points and scrape a commute time from your favorite commute estimator. Origionally I attempted to generate and scrape in the same step but any error would derail my process.

### The results
Embeding the tableau widget is sucking so for now here is a [link](http://public.tableausoftware.com/views/UWcommutetimeVisualization/DrivetimestoUWSeattleCampusDashboard?:embed=y&:display_count=no).

<script type='text/javascript' src='http://public.tableausoftware.com/javascripts/api/viz_v1.js'> </script> 

<div class="tableauPlaceholder" style="width: 654px; height: 798px;"> 
	<noscript> 
		<a href="#"> 
			<img alt="Drive times to UW Seattle Campus Dashboard " src="http://public.tableausoftware.com/static/images/UW/UWcommutetimeVisualization/DrivetimestoUWSeattleCampusDashboard/1_rss.png" style="border: none" /> 
		</a> 
	</noscript> 
	<object class="tableauViz" width="654" height="798" style="display:none;"> 
		<param name="host_url" value="http://public.tableausoftware.com/" /> 
		<param name="site_root" value="" /> 
		<param name="name" value="UWcommutetimeVisualization/DrivetimestoUWSeattleCampusDashboard" /> 
		<param name="tabs" value="no" /> 
		<param name="toolbar" value="yes" /> 
		<param name="static_image" value="http://public.tableausoftware.com/static/images/UW/UWcommutetimeVisualization/DrivetimestoUWSeattleCampusDashboard/1.png" / > 
		<param name="animate_transition" value="yes" /> 
		<param name="display_static_image" value="yes" /> 
		<param name="display_spinner" value="yes" /> 
		<param name="display_overlay" value="yes" /> 
		<param name="display_count" value="yes" /> 
	</object> 
</div> 
