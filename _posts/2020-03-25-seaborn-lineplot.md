---
layout: post
title:  Seaborn line plot
date:   2020-03-25 16:54
categories: Tech
tags: Visualization
author: 豆藏
---

* content
{:toc}

To generate something like this:
![](https://github.com/bchen4/bchen4.github.io/raw/master/img/20200325/sns_line.png)





Code:

```python
# height is actual sub-plot height and aspect determine how wide each figure is
# sharey is set to False so each sub- plot has its own y-lim
g = sns.FacetGrid(test, row='cities',height=3, aspect=6, sharex=False, sharey=False)
# If not set xticks, all dates will be together and hard to see. So reset xticks and labels
g.set(xticks=np.arange(1,365,30), xticklabels=np.arange(1,365,30))

# Map lineplot to each facet
g.map(sns.lineplot, 'event_day_of_year', 'numbers', 'hue').set_titles("{row_name}")

# Add legend
g.add_legend()

# If not adjust, figure title will be overlapping with first sub-plot title
plt.subplots_adjust(top=0.9)
g.fig.suptitle('THIS IS FIGURE TITLE')

# Add vertical reference line
axes = g.fig.axes
for ax in axes:
    ax.axvline(237,color='r')
plt.show()
```

Things learned
1. When plotting, always make sure the columns are numerical.
2. Manually sort x-axis numbers to make sure they will appear in the order you want.
3. Sort hue column so the color for each element stays the same
4. For hue and legend, I have [2017, 2018]. However when they are integer, seaborn will somehow generate 2016 in the legend. So I have to convert int to str.
When converting, had to use
```
df["hue"] = ["$%s$" % x for x in df["hue"]]
```
where the $...$ is a hack (with the side-effect of rendering the number in LaTeX) that prevents:
```
AttributeError: 'str' object has no attribute 'view'
```
Other schemes such as "= %s" % x also work. ([Reference](https://github.com/mwaskom/seaborn/issues/1653#issuecomment-536680369))

