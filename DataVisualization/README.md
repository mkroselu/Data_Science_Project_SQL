**© 2022 Scott A. Bruce. Do not distribute.**

# Basic data visualization in Python

This notebook follows Chapter 4 in the Python Workshop textbook starting from the section on Plotting Techniques.  Herein, we will use the `matplotlib` and `seaborn` packages to create basic visualizations in Python.  Each visualization comes with strengths and weaknesses and choosing an appropriate visualization for the task at hand is often a source of frustration for content creators and readers alike.  Choose wisely!  Some general guidance is provided in what follows.


```python
%matplotlib inline
import matplotlib.pyplot as plt
import seaborn as sns
from IPython.core.interactiveshell import InteractiveShell
InteractiveShell.ast_node_interactivity = "all"
```

## 1. Exercise 62 (scatter plot)

Scatter plots are a great choice when visualizing a collection of independent data points.


```python
temperature = [14.2, 16.4, 11.9, 12.5, 18.9, 22.1, 19.4, 23.1, 25.4, 18.1, 22.6, 17.2]
sales = [215.20, 325.00, 185.20, 330.20, 418.60, 520.25, 412.20, 614.60, 544.80, 421.40, 445.50, 408.10]
```


```python
plt.scatter(temperature, sales, color='red')
plt.show()
plt.show

```
    
![png](output_4_1.png)
    

```python
plt.title('Ice-cream sales versus Temperature')
plt.xlabel('Temperature')
plt.ylabel('Sales')
plt.scatter(temperature, sales, color='red')
plt.show()
```

    
![png](output_5_4.png)
    


## 2. Exercise 63 (line plot)

Line plots are useful when visualizing data points that are connected in some meaningful way over the x-axis (e.g. repeated measurements over time).


```python
stock_price = [190.64, 190.09, 192.25, 191.79, 194.45, 196.45, 196.45, 196.42, 200.32, 200.32, 
               200.85, 199.2, 199.2, 199.2, 199.46, 201.46, 197.54, 201.12, 203.12, 203.12, 203.12, 202.83, 202.83, 203.36, 206.83, 204.9, 204.9, 204.9, 204.4, 204.06]
```


```python
plt.plot(stock_price)
plt.title('Opening Stock Prices')
plt.xlabel('Days')
plt.ylabel('$ USD')
plt.show()
```


    
![png](output_8_4.png)
    



```python
t = list(range(1, 31))
```


```python
plt.plot(t, stock_price, marker='.', color='red')
plt.xticks([1, 8, 15, 22, 28])
plt.show()
```

    
![png](output_10_2.png)
    


## 3. Exercise 64 (bar plot)

Bar plots are generally useful in portraying counts of items across categories (each with its own bar).


```python
grades = ['A', 'B', 'C', 'D', 'E', 'F']
students_count = [20, 30, 10, 5, 8, 2]
```


```python
plt.bar(grades, students_count, color=['green', 'gray', 'gray', 'gray', 'gray', 'red'])
```
    
![png](output_13_1.png)
    


```python
plt.title('Grades Bar Plot for Biology Class')
plt.xlabel('Grade')
plt.ylabel('Num Students')
plt.bar(grades, students_count, color=['green', 'gray', 'gray', 'gray', 'gray', 'red'])
plt.show()
```
    
![png](output_14_4.png)
    

```python
plt.barh(grades, students_count, color=['green', 'gray', 'gray', 'gray', 'gray', 'red']) # maybe used in long category names
```

    
![png](output_15_1.png)
    


## 4. Exercise 65 (pie chart)

Use sparingly.  Pie charts are useful for conveying fractional data and percentages.  However, pie charts make it difficult to compare the relative proportions across many categories.  Often times a bar chart or a table can be used in place of a pie chart, which can enable more accurate comparisons across categories.


```python
labels = ['Monica', 'Adrian', 'Jared']
num = [230, 100, 98] # Note that this does not need to be percentages
```


```python
plt.pie(num, labels=labels, autopct='%1.1f%%', colors=['lightblue', 'lightgreen', 'yellow'])
```
    
![png](output_18_1.png)
    

```python
plt.title('Voting Results: Club President', fontdict={'fontsize': 20})
plt.pie(num, labels=labels, autopct='%1.1f%%', colors=['lightblue', 'lightgreen', 'yellow'])
plt.show()
```

    
![png](output_19_2.png)
    


## 5. Exercise 66 (heat map)

Heat maps are an excellent visualization for illustrating the relationship between two categorical variables.  First, we will define a heatmap function.


```python
def heatmap(data, row_labels, col_labels, ax=None,
            cbar_kw={}, cbarlabel="", **kwargs):
    if not ax:
        ax = plt.gca()

    im = ax.imshow(data, **kwargs)
    cbar = ax.figure.colorbar(im, ax=ax, **cbar_kw)
    cbar.ax.set_ylabel(cbarlabel, rotation=-90, va="bottom")
    ax.set_xticks(np.arange(data.shape[1]))
    ax.set_yticks(np.arange(data.shape[0]))
    ax.set_xticklabels(col_labels)
    ax.set_yticklabels(row_labels)
    ax.tick_params(top=True, bottom=False, labeltop=True, labelbottom=False)
    plt.setp(ax.get_xticklabels(), rotation=-30, ha="right", rotation_mode="anchor")
    for edge, spine in ax.spines.items():
        spine.set_visible(False)
    ax.set_xticks(np.arange(data.shape[1]+1)-.5, minor=True)
    ax.set_yticks(np.arange(data.shape[0]+1)-.5, minor=True)
    ax.grid(which="minor", color="w", linestyle='-', linewidth=3)
    ax.tick_params(which="minor", bottom=False, left=False)
    return im, cbar
```


```python
import numpy as np
import matplotlib.pyplot as plt
data = np.array([
    [30, 20, 10,],
    [10, 40, 15],
    [12, 10, 20]
])

im, cbar = heatmap(data, ['Class-1', 'Class-2', 'Class-3'], ['A', 'B', 'C'], cmap='YlGn', cbarlabel='Number of Students')

```


    
![png](output_22_0.png)
    


Notice that we used a `numpy` array to store the data for the heatmap?  More on this later...

It would be helpful to be able to provide more information about the particular values corresponding to each color.  To accomplish this, we can define another function (below) that does this. 


```python
def annotate_heatmap(im, data=None, valfmt="{x:.2f}",
                     textcolors=["black", "white"],
                     threshold=None, **textkw):
    import matplotlib
    if not isinstance(data, (list, np.ndarray)):
        data = im.get_array()
    if threshold is not None:
        threshold = im.norm(threshold)
    else:
        threshold = im.norm(data.max())/2.
    kw = dict(horizontalalignment="center",
              verticalalignment="center")
    kw.update(textkw)
    if isinstance(valfmt, str):
        valfmt = matplotlib.ticker.StrMethodFormatter(valfmt)
    texts = []
    for i in range(data.shape[0]):
        for j in range(data.shape[1]):
            kw.update(color=textcolors[im.norm(data[i, j]) > threshold])
            text = im.axes.text(j, i, valfmt(data[i, j], None), **kw)
            texts.append(text)

    return texts
```


```python
im, cbar = heatmap(data, ['Class-1', 'Class-2', 'Class-3'], ['A', 'B', 'C'], cmap='YlGn', cbarlabel='Number of Students')
texts = annotate_heatmap(im, valfmt="{x}")
```

    /usr/local/lib/python3.7/site-packages/ipykernel_launcher.py:19: DeprecationWarning: In future, it will be an error for 'np.bool_' scalars to be interpreted as an index
    


    
![png](output_25_1.png)
    


## 6. Exercise 67 (histogram + density plot)

When visualizing the distribution of a continuous variable, histograms and density plots are a great option.


```python
data = [90, 80, 50, 42, 89, 78, 34, 70, 67, 73, 74, 80, 60, 90, 90]
sns.distplot(data)
```

    
![png](output_27_2.png)
    



```python
plt.title('Density Plot')
plt.xlabel('Score')
plt.ylabel('Density')
sns.distplot(data)
plt.show()
```

    
![png](output_28_5.png)
    


easily to identify clusters in the dataset that may not be visible in histogram or one-dimensional graph


```python
height=[85.08,79.25,85.38,82.64,80.51,77.48,79.25,78.75,77.21,73.11,82.03,82.54,
        74.62,79.82,79.78,77.94,83.43,73.71,80.23,78.27,78.25,80.00,76.21,86.65,
        78.22,78.51,79.60,83.88,77.68,78.92,79.06,85.30,82.41,79.70,80.16,81.11,
        79.58,77.42,75.82,74.09,78.31,83.17,75.20,76.14]
weight = list(range(1,45))
plt.title('Weight Dataset - Contour Plot')
plt.ylabel('height (cm)')
plt.xlabel('weight (kg)')
sns.kdeplot(weight, height, cmap="Reds", ) # cmap = color map 
```

    
![png](output_30_5.png)
    


## 7. More on visualizing univariate and bivariate distributions from the seaborn package documentation

For more details visit https://seaborn.pydata.org/tutorial/distributions.html


```python
penguins = sns.load_dataset("penguins")
sns.displot(penguins, x="flipper_length_mm")
```

    
![png](output_32_1.png)
    

```python
sns.displot(penguins, x="flipper_length_mm", hue="species")
```

    
![png](output_33_1.png)
    


```python
sns.displot(penguins, x="flipper_length_mm", hue="species", element="step")
```

    
![png](output_34_1.png)
    



```python
sns.displot(penguins, x="flipper_length_mm", hue="species", multiple="stack")
```


    
![png](output_35_1.png)
    



```python
sns.displot(penguins, x="flipper_length_mm", kind="kde")
```

    
![png](output_36_1.png)
    



```python
sns.displot(penguins, x="flipper_length_mm", kind="kde", bw_adjust=.25) # kde: kernel density estimate # bw: bandwidth 
```
    
![png](output_37_1.png)
    



```python
sns.displot(penguins, x="flipper_length_mm", hue="species", kind="kde")
```

    
![png](output_38_1.png)
    



```python
diamonds = sns.load_dataset("diamonds")
sns.displot(diamonds, x="carat", kde=True)
```
    
![png](output_39_1.png)
    



```python
sns.displot(penguins, x="bill_length_mm", y="bill_depth_mm") 
# dark blue corresponds to some numbers and want to compare to the numbers of lighter blue
# bc not able to perceive differences in colors and translate back to numerical differences easily, 
# this plot is not the best for making accurate comparisons about distribution 
# color is really helpful for identifying clusters. 
# so if your purpose is these three species are quite different and cluster together, this plot is fine. 
```
    
![png](output_40_1.png)
    



```python
# but if you want to make more accurate comparisons of distribution, you can add color bar helping a little but not tremendously. 
sns.displot(penguins, x="bill_length_mm", y="bill_depth_mm", binwidth=(2, .5), cbar=True)
```
    
![png](output_41_1.png)
    



```python
# or you can do a contour plot 
sns.displot(penguins, x="bill_length_mm", y="bill_depth_mm", kind="kde")
```

    
![png](output_42_1.png)
    



```python
# same code, use different colors 
sns.displot(penguins, x="bill_length_mm", y="bill_depth_mm", hue="species")
```

    
![png](output_43_1.png)
    



```python
sns.displot(penguins, x="bill_length_mm", y="bill_depth_mm", kind="kde", thresh=.2, levels=4)
```

    
![png](output_44_1.png)
    



```python
sns.displot(penguins, x="bill_length_mm", y="bill_depth_mm", hue="species", kind="kde")
```

    
![png](output_45_1.png)
    



```python
# jointplot let us get scatter plot and univariate distributions around axises 
# if you want all information in one plot, this will do the trick. 
sns.jointplot(data=penguins, x="bill_length_mm", y="bill_depth_mm")
```

    
![png](output_46_1.png)
    



```python
sns.jointplot(
    data=penguins,
    x="bill_length_mm", y="bill_depth_mm", hue="species",
    kind="kde"
)
```

![png](output_47_1.png)
    



```python
# boxplot with kde
g = sns.JointGrid(data=penguins, x="bill_length_mm", y="bill_depth_mm")
g.plot_joint(sns.histplot)
g.plot_marginals(sns.boxplot)
```

    
![png](output_48_1.png)
    



```python
sns.displot(
    penguins, x="bill_length_mm", y="bill_depth_mm",
    kind="kde", rug=True
)
```


```python
sns.relplot(data=penguins, x="bill_length_mm", y="bill_depth_mm")
sns.rugplot(data=penguins, x="bill_length_mm", y="bill_depth_mm")
```

    
![png](output_50_2.png)
    



```python
# pairplot: scatter plot metrics that you can build in R. 
# Univariate distributions along the diagonal and scatter plot of bivariate variable distributions for all variables. 
sns.pairplot(penguins)
```
    
![png](output_51_1.png)
    



```python
g = sns.PairGrid(penguins)
g.map_upper(sns.histplot)
g.map_lower(sns.kdeplot, fill=True)
g.map_diag(sns.histplot, kde=True)
```

    
![png](output_52_3.png)
    


## 8. Multiple charts in the same figure


```python
# Split the figure into 2 subplots
fig = plt.figure(figsize=(8,4))
ax1 = fig.add_subplot(121) # 121 means split into 1 row, 2 cols, and put in the 1st part
ax2 = fig.add_subplot(122) # 121 means split into 1 row, 2 cols, and put in the 2nd part

#first subplot
labels = ['Adrian','Monica','Jared']
num = [230, 100, 98]
ax1.pie(num, labels=labels, autopct='%1.1f%%', colors=['lightblue','lightgreen','yellow'])
ax1.set_title('Pie Chart (Subplot 1)')

#second subplot
plt.bar(labels, num, color=['lightblue','lightgreen','yellow'])
ax2.set_title('Bar Chart (Subplot 2)')
ax2.set_xlabel('Candidate')
ax2.set_ylabel('Votes')
fig.suptitle('Voting Results', size=14)
```

    
![png](output_54_7.png)
    


## 9. Exercise 69 (3-D plots)


```python
from mpl_toolkits.mplot3d import Axes3D
X = np.linspace(0, 10, 50)
Y = np.linspace(0, 10, 50)
X, Y = np.meshgrid(X, Y)
Z = (np.sin(X))

# Setup axis
fig = plt.figure(figsize=(7,5))
ax = fig.add_subplot(111, projection='3d')
ax.plot_surface(X, Y, Z)

# Add title and axes labels
ax.set_title("Demo of 3D Plot", size=13)
ax.set_xlabel('X')
ax.set_ylabel('Y')
ax.set_zlabel('Z')
```
    
![png](output_56_5.png)
    


For other fascinating 3-D plots available in matplotlib, visit https://matplotlib.org/2.0.2/mpl_toolkits/mplot3d/tutorial.html.

## 10. Dos and don'ts of data visualization

### 10.1 Design principles

**Design for the audience**

General audience considerations:
 - Language, knowledge, skills, expectation
 - Broad cultural representations and conventions
 - Discipline based conventions and preferences
 
Individual specific limitations and abilities
 - Visual acuity
 - Color blindness
 - Document accessibility regulations (https://www.epa.gov/accessibility/what-section-508)
 

**Design for the task**

Discovery and hypothesis generation
 - Comparative views: let the data speak, avoid confirmation bias
 - Alternative views: Change context, scale and visual perspective
 - Interactive views: sliders, dropdowns, filters
 
Analysis
 - Inferential graphics: hypothesis tests, error rate control confidence levels, ROC curves
 - Model graphics: structure, comparisons, residuals, criticism
 
Decision Making
 - Actionable graphics
 
Communication, teaching, learning
 - Summaries
 - Visual stories and explanations
 - Establish the problem, elaborate, and resolve
 - Verbal presentations with a single picture
 

 
 

### 10.2 Human perception and cognition

**Structuring input**
 - We continuously structure our sensory input on different scales.  
 - Many times we are unaware of this structuring.  For example, the blood supply of the retina is located in front of the retina, which means that light passes through the blood supply on its way to the photodetectors on the retina. We don't see the retinal blood supply because it doesn't change, and our eyes ignore unchanging images.
 - The mind is not a camera that stores everything we see exactly (remember - things get structured).
 
![image.png](image.png)

**Types of blindness in people with vision**
 - Color blindness
 - Inattentional blindness
 - Change blindness

how big is the difference visually and our ability to actually perceive and tell there is a difference here 

### 10.3 Weber's Law

**Just noticeable differences and the probability of detection**

 - Related to the ratios of stimuli magnitudes
 - Difference detection is more likely when ratios are further from 1
 
**Which line is longer?**
 
![image-2.png](image-2.png)

Line lengths have a ratio of 1.02.  

How about now.  Which line is longer?

![image-2.png](image-2.png)

Using the marker of equal lengths, our eyes can focus in on the line lengths after the marker.  This increases the ratio of the line lengths after the marker, which makes it easier to detect the difference.

![image.png](image.png)

Same idea with the equal sized boxes.  

**Grid lines** take advantage of this idea to help with comparisons between patterns. Consider comparing the x and y location of x and y minima without and with background grid lines.  Which of the below graphs has a smaller minima?

![image.png](image.png)

How about now.

![image.png](image.png)

Easier right?  Thank you grid lines!

### 10.4 Perceptual accuracy of extraction for continuous variable encodings (Cleveland and McGill, 1984)

1. Position along a scale (best)
2. Position along non-aligned scales
3. Length, direction, angle
4. Area
5. Volume
6. Shading (worse)

### 10.5 Superimposition vs. juxtaposition

When our eyes 'jump' from one image to another, there is no input.  The old image fades in about 1/5 of a second. A smaller part of the image under scrutiny is retained longer, but then we are blind to changes elsewhere in the image. Comparing juxtaposed images requires back and forth scrutiny.  This is tiring. Consider the comparisons needed to tell the story you want the readers to understand and choose the image accordingly.

![image.png](image.png)

![image-2.png](image-2.png)

Rapid alternation of almost identical images is often used to find changes between almost identical photos.
Changes appear as blinking. Inserting a blank image between images emulates change blindness

## 10.6 Comparing curves (we don't do this well)

![image.png](image.png)

If you want to emphasize the difference in curves, why not plot the difference?

### 10.7 Quantitative graphic guidelines

Four general goals under tension
 - Enable accurate comparisons
 - Simplify appearance
 - Provide context for interpretation
 - Engage the reader/analyst
 
Use visual encodings with high perceptual accuracy of extraction
 - Best encodings: Position along a scale or along identical scales
 - Grid lines increase decoding and shape comparison accuracy
 - Good examples: Dot plots, bar charts, scatter plots, time series plots
 
Comparison layouts
 - Superimpose
 - Juxtapose with comparable scales
 - Show differences explicitly
 
Comparison accuracy decreases with distance
 - Organize to make key items for comparison close together

Context is important in comparisons
 - Provide reference lines
 - Control extraneous sources of variation


