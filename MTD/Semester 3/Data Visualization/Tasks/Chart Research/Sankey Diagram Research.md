#data_visualization 

![[Pasted image 20261124203325.png|620]]
# Examples
## Good

Nice example showcasing immigration stats. Easy to read.
![[Pasted image 20261124155427.png]]

Pretty version of Sankey diagram, showcasing its potential for visualizations. 
![[Pasted image 20261124155440.png]]

Another example of a stylized Sankey diagram.
![[Pasted image 20261124155542.png]]
## Bad

Ew. 
![[Pasted image 20261124155617.png]]

Hard to read and make anything out here.
![[Pasted image 20261124195054.png]]

Another mess
![[Pasted image 20261124203234.png]]
# How to Create
## Data

Ideally you want to depict information like breaking down electricity usage or spending into sub-categories and further on into more subcategories. 

![[Pasted image 20261124203405.png]]
## How to make one

Ideally your data features several columns that are sources and targets. These get summed up into categories and connected via a flow to each other. The categories in the individual columns can be arranged as wanted, optimized to have as little crossings as possible or be individually arranged by the user in an interactive chart.
## How to make one in python

The best library I’ve found so far to make a Sankey diagram is plotly.

Example graph from their documentation:
```python
import plotly.graph_objects as go

fig = go.Figure(data=[go.Sankey(
    node = dict(
      pad = 15,
      thickness = 20,
      line = dict(color = "black", width = 0.5),
      label = ["A1", "A2", "B1", "B2", "C1", "C2"],
      color = "blue"
    ),
    link = dict(
	  # indices correspond to labels, eg A1, A2, A1, B1, ...
      source = [0, 1, 0, 2, 3, 3], 
      target = [2, 3, 3, 4, 4, 5],
      value = [8, 4, 2, 8, 4, 2]
  ))])

fig.update_layout(title_text="Basic Sankey Diagram", font_size=10)
fig.show()
```

![[basic-sankey-diagram.png]]
# How to Read

Reading a Sankey diagram is relatively easy. They usually go from left to right with individual nodes in between. These are connected via a flow, its size representing how many connections there are for these two nodes. 
# Pitfalls
*some of these can be seen in the bad examples I’ve noted above, actually a lot of them*

- can get really complex and turn into a spaghettified mess
- bad node order
- improper use of color
- improper labeling
- sometimes get used just because they’re pretty
# Comparison to a Parallel Set

*Example parallel sets*
![[Pasted image 20261124203642.png|248]]

![[Pasted image 20261124203804.png|491]]

While Sankey’s are used to show flows or parts of a whole, parallel sets are used to compare between other variables. 

One example I’ve found is this titanic chart. Makes more sense to be a parallel set, as the order doesn’t really matter, while for a Sankey the order would matter. 

Similar to parallel coordinate plots, you can let the user move around the categories (which wouldn’t make any sense in a regular Sankey) to potentially discover new connections.

It is… pretty much the same though, and I wouldn’t fault anyone for confusing these two diagrams.

![[Pasted image 20261124204431.png|540]]

Quick comparison I made for myself. Not sure if it perfectly makes sense but it helps me understand it somewhat better.

**SANKEY — USE WHEN ORDER MATTERS AND STUFF IS FLOWING THROUGH OTHER STUFF**
**PARALLEL SET — USE WHEN COMPARING CATEGORIES BELONGING IN OTHER CATEGORIES**
# Sources

*Human sourced, added if I looked at it for my research.*
- [Sankey diagram – from Data to Viz](https://www.data-to-viz.com/graph/sankey.html)
- [Sankey Diagrams – A Sankey diagram says more than 1000 pie charts](https://www.sankey-diagrams.com/)
- [Sankey Diagrams And Alluvial Plots | by Isaac Neuhaus | Stackademic](https://blog.stackademic.com/sankey-diagrams-and-alluvial-plots-3a2ec6db1bd1)
- [Sankey diagram - Wikipedia](https://en.wikipedia.org/wiki/Sankey_diagram)
- [Energy Sankey - IEA](https://www.iea.org/data-and-statistics/data-tools/energy-sankey)
- [Sankey diagram in Python](https://plotly.com/python/sankey-diagram/)
- [Sankey diagram](https://python-graph-gallery.com/sankey-diagram/)
- [Sankey Diagrams have been the worst reporting visual I ever seen and have no idea why they are so popular recently : FPandA](https://www.reddit.com/r/FPandA/comments/1fcse9g/sankey_diagrams_have_been_the_worst_reporting/)
- [The 5 most common mistakes in designing a Sankey Diagram (and how to avoid them)](https://datasketch.blog/en/post/the-5-most-common-mistakes-in-designing-a-sankey-diagram-and-how-to-avoid-them/)
- [Tableau Chart Templates Part 1: Sankeys - The Flerlage Twins: Analytics, Data Visualization, and Tableau](https://www.flerlagetwins.com/2018/04/sankey-template.html)
- [Parallel Sets - Learn about this chart and tools to create it](https://datavizcatalogue.com/methods/parallel_sets.html)
- [Parallel coordinates - Wikipedia](https://en.wikipedia.org/wiki/Parallel_coordinates)
- [Parallel Sets | Data Viz Project](https://datavizproject.com/data-type/parallel-sets/)
