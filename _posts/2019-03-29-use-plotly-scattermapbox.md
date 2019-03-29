---
layout: post
title:  Use Plotly to draw map
date:   2019-03-29 12:18
categories: Tech
tags:  Visualization
author: 豆藏
---

* content
{:toc}

## Purpose

Generate maps using latitude and longitude. Data looks like this. The label column separates locations to two types, and I want to show them in different color.

latitude| longitude  | label 
----|----|----
 30.655627 | -98.425563 | 1     
 30.659360 | -98.484974 | 1     
 30.636712 | -98.411125 | 2     

By browsing around different types of maps provided by Plotly, the scattermapbox is what I need.




### 1: To get it work

Simply follow the instructions from [official tutorial](<https://plot.ly/python/scattermapbox/>) and also you will need to register on [mapbox](<https://help.plot.ly/mapbox-atlas/?_ga=2.223669529.364899302.1553369157-1426543396.1553198605>) to get a token.

Copy the example and change the data to your own data. (Layout and draw function are the same as in the example)

``` python
data = [ go.Scattermapbox(
        lon = df['longitude'],
        lat = df['latitude'],
        mode = 'markers',
        marker=go.scattermapbox.Marker(
            size=5,
            color = 'blue'
        )),
       ]
```

Output figure:

![1.png](https://github.com/bchen4/bchen4.github.io/raw/master/img/20190329/1.png)



So it worked.



### 2: Different marker colors using trace

In Plotly, you can easily add different traces to your data like an array. The latter traces will be draw on top of the previous markers. Also, you can name the trace to make the legend easier to understand.

Now let's process the data and have it like this:

```python
df1 = df[df['label']==1]
df2 = df[df['label']==2]

# Use multiple traces and name them 
data = [ go.Scattermapbox(
        lon = df1['longitude'],
        lat = df1['latitude'],
        mode = 'markers',
        marker=go.scattermapbox.Marker(
            size=5,
            color = 'blue'
        ),
        name = "1st neighbors"),
       go.Scattermapbox(
        lon = df2['longitude'],
        lat = df2['latitude'],
        mode = 'markers',
        marker=go.scattermapbox.Marker(
            size=5,
            color = 'grey'
        ),
       name='2nd neighbors'),
        go.Scattermapbox(
        lon = [-98.440916],
        lat = [30.660593],
        mode = 'markers',
        marker=go.scattermapbox.Marker(
        size=5,
        color = 'red'
        ),
        name = "target"
        )
       ]

```
![1.png](https://github.com/bchen4/bchen4.github.io/raw/master/img/20190329/2.png)

Now we can use different colors to distinguish different locations.

The benefit of using trace is that in the figure, you can turn the data on and off for better exploration.




### 3: Use scale to get different marker color
```python
# First make a column indication the color range, the values should be [0,1]
df['value'] = df['label']-1
# Define color scale. Here we only want two colors so just define two colors
scl = [
    [0.0, 'rgb(165,0,38)'],
    [1.0, 'rgb(49,54,149)']
]
data = [ go.Scattermapbox(
        lon = df['longitude'],
        lat = df['latitude'],
        mode = 'markers',
        marker=go.scattermapbox.Marker(
            size=5,
            color =  df['value'],  # This is used to match the color scale
            colorscale = scl
        ),
        name = "1st neighbors")
       ]
```

![1.png](https://github.com/bchen4/bchen4.github.io/raw/master/img/20190329/3.png)

We can still get different color markers. The down-side is this figure is kind of static. You can not turn data on and off.



Full notebook is [here](<https://github.com/bchen4/learn_viz/blob/master/notebooks/plotly_scattermapbox.ipynb>).

