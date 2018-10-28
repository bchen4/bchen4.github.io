---
layout: post
title:  Use Numpy to get date range matrix
date:   2018-05-13 21:36
categories: Tech
tags: Python
author: bchen
---

* content
{:toc}

In a recent task I need to generate a 2-column date matrix, for each date in the 1st column, there will be N rows containing N previous dates. Something look like:


date1|date2
----|----
|2017-10-01 |2017-10-01|
|2017-10-01 |2017-09-30|
|2017-10-01 |2017-09-29|
|2017-10-02 |2017-10-02|
|2017-10-02 |2017-10-01|
|2017-10-02 |2017-09-30|





To avoid using loop, I utilized broadcasting in Numpy:

### 1: get a date range using in-house scripts
``` python
date_range = pickup.date_range('20171001','20171003')
horizon_range_time = datetime.timedelta(days=1)*range(3)

date_range

```

> [datetime.datetime(2017, 10, 1, 0, 0),
> datetime.datetime(2017, 10, 2, 0, 0),
> datetime.datetime(2017, 10, 3, 0, 0),]



### 2: transverse date_range and add horizon_range
```python
np.matrix(date_range).T - horizon_range_time

```
>matrix([[datetime.datetime(2017, 10, 1, 0, 0),
         datetime.datetime(2017, 9, 30, 0, 0),
         datetime.datetime(2017, 9, 29, 0, 0)],
        [datetime.datetime(2017, 10, 2, 0, 0),
         datetime.datetime(2017, 10, 1, 0, 0),
         datetime.datetime(2017, 9, 30, 0, 0)],
        [datetime.datetime(2017, 10, 3, 0, 0),
         datetime.datetime(2017, 10, 2, 0, 0),
         datetime.datetime(2017, 10, 1, 0, 0)]], dtype=object)
         
 
### 3: Flat the matrix and make a DataFrame
```python
## Here cannot use date_range * 3, will end up [1,2,3,1,2,3,1,2,3]
col1 = np.repeat(date_range,3)  ## [1,1,1,2,2,2,3,3,3]
col2 = (np.matrix(date_range).T - horizon_range_time).flat
a = pd.DataFrame({'date1':col1, 'date2':col2})
a.head(6)
```

date1 |	date2
----|----
2017-10-01 | 	2017-10-01
2017-10-01 |	2017-09-30
2017-10-01 |	2017-09-29
2017-10-02 |	2017-10-02
2017-10-02 |	2017-10-01
2017-10-02 |	2017-09-30


 
