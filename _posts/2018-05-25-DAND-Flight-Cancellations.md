---
layout: post
title: American Flight Cancellations
---

Being tired of missing engagements because of delayed flights, I've used the American flight data to understand what the key causes of cancellations and delays are in the sky! 
*As part of the Udacity Data Analyst Nanodegree*

```python
import IPython.core.display as di

# This line will hide code by default when the notebook is exported as HTML
di.display_html('<script>jQuery(function() {if (jQuery("body.notebook_app").length == 0) { jQuery(".input_area").toggle(); jQuery(".prompt").toggle();}});</script>', raw=True)

# This line will add a button to toggle visibility of code blocks, for use with the HTML export version
di.display_html('''<button onclick="jQuery('.input_area').toggle(); jQuery('.prompt').toggle();">Toggle code</button>''', raw=True)

```


<script>jQuery(function() {if (jQuery("body.notebook_app").length == 0) { jQuery(".input_area").toggle(); jQuery(".prompt").toggle();}});</script>



<button onclick="jQuery('.input_area').toggle(); jQuery('.prompt').toggle();">Toggle code</button>



```python
import glob
import pandas as pd
pd.options.mode.chained_assignment = None  # default='warn'
import os
import numpy as np
from matplotlib import pyplot as plt

wd = os.getcwd()

data_dir = os.path.join(wd,'data')

df = pd.read_csv(data_dir+"/2008.csv")

df = df[['Month', 'DayofMonth', 'DayOfWeek','DepDelay','ArrDelay', 'Origin', 'Dest', 'Distance','CarrierDelay',
       'WeatherDelay','Cancelled','LateAircraftDelay','CancellationCode', 'Diverted',]]

df.to_csv('2008_condensed.csv')
```

[**INITAL VIZ - Flight Cancellations 2008**](https://public.tableau.com/views/Book3_7569/InitalStory-Flightsin2008?:embed=y&:display_count=yes)

[**FINAL VIZ - Flight Cancellations 2008**](https://public.tableau.com/shared/3P4HRXQRF?:display_count=yes)

# Create Tableau Story: 2008 US Flights


## Summary

Analysing flight data in America to understand what causes delays in flights. The data set is quite comprehensive including over 7 million flights in 2008 with detailed information around departure and arrival times, weather delays, origin and destination as well as cancellation rates for each flight. I have visualised this data to better understand some of the factors which contribute to delayed and cancelled flights.

## Design
I wanted to deep dive into the trends of cancellations and delays in flights when compared to months as well as origin of port for flights. I believe using bar charts and line graphs to understand the trends in this data
     
I also wanted to use the additional information on cancellation code to compare these against origins to understand if there is a trend within specific airports or time of year.

The key aspect of the design for me is to simply the graphics to ensure my audience can easily interpret one major insight from each viz. To keep the visualisations simple, I will use bar charts and heatmaps to display key points. 

One key aspect of simplying the visualisations is to provide minimalise the chart junk and data conveyed through each chart. Limiting the use of colors and only using 2-3 variations where possible is a key aim of mine.

While achieving this I also want to build a narrative which helps guide the readers towards some deeper takeaways. The way to this this is by structuring the story format into a cohesive layout.


## Resources & Data

[**2008 Flight Data**](http://stat-computing.org/dataexpo/2009/the-data.html)
