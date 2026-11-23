#data_visualization 

![[HW_YcKGa4AAMmxt.jpg|260]]

# Data Visualization

Why?

**Easier to use the power of eyeballs to see:**
- patterns
- clusters
- holes
- outliers

Exploration <-> Explanation

>*The visual representation and presentation of data to facilitate understanding*
>
>- Andy Kirk (2016)
## Stages of understanding

1. Perceiving
2. Interpreting
3. Comprehending
## Needs-Driven Workflow Design

![[Pasted image 20261121150301.png]]
# Design Triangle Framework

![[21min.png|235]]
![[Pasted image 20261121150413.png]]
## Who

- who are the users
- what do they do
- their goals
- work context

have to determine if your users **novices** or **experts** etc.
## Why

- What’s the task
	- Domain specific - US Presidential vote
	- Abstrakt - comparison of two values

![[Pasted image 20261121150640.png|476]]

### Task Abstraction 

- Categorizing & Clustering
	- ![[Pasted image 20261121150816.png|225]]
- Trends
	- ![[Pasted image 20261121150823.png|224]]
- Distribution
	- ![[Pasted image 20261121150835.png|256]]
- Composition
	- ![[Pasted image 20261121150846.png|241]]
- Ordering, Ranking, Sorting
	- ![[Pasted image 20261121151058.png|267]]
- Comparison
	- ![[Pasted image 20261121151102.png|295]]
- Geospatial Location
	- ![[Pasted image 20261121151110.png|255]]
- Correlations and Relationships
	- ![[Pasted image 20261121151116.png|251]]
## What
### Data Abstraction

- Cardinality and Dimension of Data
	- Entities and Attributes
	- How many categories, etc for each attribute
### Attribute Types

![[Pasted image 20261121151314.png|422]]

![[Pasted image 20261121151359.png]]

![[Pasted image 20261121151408.png]]
### Data Abstraction

![[Pasted image 20261121151517.png|351]]

![[Pasted image 20261121151655.png]]
# Data Transformation
## Data Wrangling

The process to prepare raw data and structure, clean and validate it.

- Data Usability — can the data be read?
- Credibility — is the data representative of real phenomenon?
- Usefulness — can I learn something out of this data?
## Data Cleaning

Process of correcting or removing wrong or incomplete data.

Possible Problems:
- missing data
- duplicates
- unrealistic values
- wrong data
- unclear data
- combination of multiple sources
## Data Analysis

- combination of task and data abstraction
- conversions
	- to user goals
- granularity
	- low detail → get an overview of data
	- high detail → get to see detailed stuff
- data of interest
	- projection of attributres
	- selection of elements
## Data Transformation

- Deriving new data from existing data

![[Pasted image 20261121152519.png]]

- converting into new data-formats 
	- e.g. for better readability 

- consolidate
	- adding the ability to add new variables or elements
# Components of a Visualization

![[MinaIdle.gif|382]]

- Reference
- Data Overlay
- Context

![[Pasted image 20261121152923.png]]

- Visual elements:
	- points
	- lines
	- areas
	- ![[Pasted image 20261121153011.png|422]]

![[Pasted image 20261121153031.png]]

![[Pasted image 20261121153101.png]]
# Expressivity and Effectivity 

![[MinaFloatGlasses.gif|487]]

![[Pasted image 20261121153346.png]]

- Precision should be determined by importance of the attribute

![[Pasted image 20261121153552.png]]

![[Pasted image 20261121153807.png]]
## Effectivity of visual Attributes

is determined by:
- Precision
- how many values can we differentiate
- differentiability
- pop-out
## The 3 Model

![[Pasted image 20261121154022.png]]
## Pre attentive thingies

- fast, parallel 
- no need to focus
- fast
- can be used to make relevant information fast to notice

![[Pasted image 20261121154224.png]]

In this case, I’d prefer a)

![[Pasted image 20261121154241.png]]

Color or size are a much better thingy to use compared to just shape.

![[Pasted image 20261121154320.png]]

Combining just makes it worse…

![[Pasted image 20261121154418.png]]
# Visualization for static Analysis

![[MinaFall.gif]]

![[Pasted image 20261121154825.png]]
## Scatterplot

![[Pasted image 20261121154912.png]]
### Correlation

![[Pasted image 20261121155001.png]]

> [!IMPORTANT]
> Correlation is not Causation
> 
> \- Someone smart probably
### Colored-dot Scatterplot

![[Pasted image 20261121155214.png|513]]

![[Pasted image 20261121155222.png|540]]
### Bubble chart

![[Pasted image 20261121155238.png]]
## Correlation Matrix

![[Pasted image 20261121155331.png]]
## Heatmap

![[Pasted image 20261121155450.png]]
## Correlation Matrix vs Heatmap

![[Pasted image 20261121155614.png]]
## Parallel Coordinates

![[Pasted image 20261121155642.png]]

![[Pasted image 20261121155831.png]]
## Radar Chart

![[Pasted image 20261121155941.png|669]]

![[Pasted image 20261121155958.png]]

![[Pasted image 20261121160132.png]]

![[Pasted image 20261121160220.png]]
## Bar Chart

![[Pasted image 20261121160235.png]]
### Lollipop Chart

![[Semester 3/Data Visualization/Exams/Kurz Test/attachments/Mina1_glasses - Copy.png|304]]
*take her pants off if you wanna see lollipop*

![[Pasted image 20261121160411.png]]
### Helps

![[Pasted image 20261121160433.png]]

![[Pasted image 20261121160509.png]]

![[Pasted image 20261121160524.png]]

![[Pasted image 20261121160619.png]]

![[Pasted image 20261121160625.png]]
### Bar Chart Variants

![[Pasted image 20261121160657.png]]

![[Pasted image 20261121160729.png]]

![[Pasted image 20261121160806.png]]
## Dot Plot

![[Pasted image 20261121160852.png]]
## Distribution of values over an area

![[Pasted image 20261121161049.png]]
## Strip Plot

![[Pasted image 20261121161156.png|230]]

![[Pasted image 20261121161206.png]]
## Jittered Strip Plot

![[Pasted image 20261121161343.png]]
## Swarm Plot

![[Pasted image 20261121161419.png]]
## Histogram

![[Pasted image 20261121161503.png]]

![[Pasted image 20261121161517.png]]
## Boxplot

![[Pasted image 20261121161620.png]]

![[Pasted image 20261121161638.png]]
## Violin Plot

![[Pasted image 20261121161756.png]]

![[Pasted image 20261121161843.png]]
## More groups and stuff

![[Pasted image 20261121162029.png]]

![[Pasted image 20261121162048.png]]

![[Pasted image 20261121162056.png]]
## Additions to Distribution Charts

![[Pasted image 20261121162155.png]]

![[Pasted image 20261121162202.png]]
# Visualization for semantic data structures
![[t_pirate_alt1.png|412]]
## Pie Chart

![[Pasted image 20261123125222.png]]
## Donut Chart

![[Pasted image 20261123125408.png]]
## Waffle Chart

![[Pasted image 20261123125840.png]]
## Stacked Bar Chart

![[Pasted image 20261123125934.png]]
## Absolute values vs percentages

![[Pasted image 20261123130402.png]]
## Totals with a large number of parts

Pie charts and such become too busy looking with more six or seven (laugh) parts. Bar charts are better for larger parts.
## Treemap

![[Pasted image 20261123130553.png]]

![[Pasted image 20261123130630.png]]
## Sunburst Chart

![[Pasted image 20261123130906.png]]

![[Pasted image 20261123130930.png]]
# Showing Data over Time

**Data over time**

![[Pasted image 20261123131050.png]]

**Change between two time periods**

![[Pasted image 20261123131218.png]]

**breakdowns of totals changing over time**

![[Pasted image 20261123131233.png]]
## Line Chart

![[Pasted image 20261123131303.png]]

**NEVER ESTIMATE MISSING VALUES JUST LEAVE A BLANK SPACE**

![[Pasted image 20261123131356.png]]

If many values are missing: use a dot plot.

![[Pasted image 20261123131440.png]]
## Avoid a second Y-Axis

- not easily noticeable which axis the data belongs to
- even when using different chart types
- using different colors makes it easier, but correlation is just coincidence
## Circular Line Chart

![[Pasted image 20261123131914.png]]
## Line Charts with many Categories

![[Pasted image 20261123132023.png]]

![[Pasted image 20261123132157.png]]
## Heatmap

![[Pasted image 20261123132237.png]]
## Warming Stripes

![[Pasted image 20261123132333.png]]
## Bar Chart

![[Pasted image 20261123132429.png]]
## Bar vs Line Chart

![[Pasted image 20261123132639.png]]
## Y-Axis

Always start at 0. Only allowed to zoom in if the changes are too hard to see otherwise.

Maximum value can also lead to wrong interpretations.

![[Pasted image 20261123133024.png]]
## Bar / Line Combination

![[Pasted image 20261123133116.png]]
## Bump Chart

![[Pasted image 20261123133244.png]]
## Slope Chart

![[Pasted image 20261123133406.png]]

![[Pasted image 20261123133518.png]]
## Arrow Chart

![[Pasted image 20261123133637.png]]
## Area Chart

![[Pasted image 20261123133806.png]]
## Small Multiples

![[Pasted image 20261123133950.png|347]]

![[Pasted image 20261123134011.png]]
## Stacked Area Chart

![[Pasted image 20261123134112.png]]

![[Pasted image 20261123134152.png]]

![[Pasted image 20261123134258.png]]

![[Pasted image 20261123134443.png]]
## When are Maps useful?

- geographic distribution of data
- drawback: less good at quantitive comparison 

Maps are projections of a sphere and can distort data.
### Mercator Projection

![[Pasted image 20261123134813.png]]
### Equal Area Projection

![[Pasted image 20261123134858.png]]
### Robinson-Projection

![[Pasted image 20261123134927.png]]
### Europe Projections

![[Pasted image 20261123135004.png]]
## Choropleth Map

![[Pasted image 20261123135041.png]]

![[Pasted image 20261123135453.png]]
## Colored dots on regions

![[Pasted image 20261123135629.png]]
## Tile Map

![[Pasted image 20261123135942.png]]
## Symbol Map

![[Pasted image 20261123140158.png]]

![[Pasted image 20261123140313.png]]

![[Pasted image 20261123140500.png]]
## absolute vs relative values

![[Pasted image 20261123140755.png]]
## Dot Density Map

![[Pasted image 20261123141009.png]]
## Flow Map

![[Pasted image 20261123141437.png]]

![[HUMauezW0AEBNrX.jpg]]