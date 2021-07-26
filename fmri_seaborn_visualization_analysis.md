# How to Use Seaborn in Python to Visualize the fMRI Dataset

## Visualization Technique

As an emerging Data Scientist in the neuroscience field, I will be using the fMRI data set included in the Seaborn sample data set library for this demonstration. Functional magnetic resonance imaging or functional MRI (fMRI) measures brain activity using a strong, static magnetic field to detect changes associated with blood flow. When an area of the brain is in use, blood flow to that region also increases. The increased blood flow is represented by a higher-amplitude signal seen as strong neural activity.
This particular library will be useful for visualizing fMRI data because it integrates closely with pandas data structures using data frames and arrays. I am planning to use multiple plots to show different visualization techniques for displaying the same data such as:
a line plot
a box plot
a swarm plot
a linear regression model

The Seaborn library focuses on different elements of the plot's mean and performs statistical aggregation to produce informative displays. They are best used to show relationships between two variables. In this case, we will be focusing on the variables Timepoint and Signal as related to the Event and Region variables.


## Visualization Library

“If Matplotlib “tries to make easy things easy and hard things possible”, Seaborn tries to make a well-defined set of hard things easy too.” — Michael Waskom
The library I am going to use is Michael Waskom’s Seaborn. It is a declarative open-source Python library built on top of Matplotlib. It is recommended to integrate the Seaborn library using the Jupyter/IPython interface in Matplotlib.

In comparison to Matplotlib, Seaborn’s functionality uses fewer syntax and specializes in statistics visualization beyond basic plotting. The Pandas library is readily integrated within Seaborn, allowing it to work with data frames and arrays more intuitively. It also provides default themes for visualization, which some would consider being a limitation in customization.

I decided to use this library because it is a good choice for built-in statistical tasks. It has specialized support for visualizing relationships between categorical and numeric variables which for the dataset I am using will help to narrate the results of the analysis.

Resource

### Seaborn Installation

The latest stable release can be installed from PyPI: pip install seaborn
To include the optional dependencies: pip install seaborn[all]
Developmental version from Github pip install git+https://github.com/mwaskom/seaborn.git
Install from Anaconda using conda: conda install seaborn
Required dependencies that are downloaded when you install seaborn: NumPy, scipy, pandas, matplotlib.

Resource

## Demonstration
### To Import Seaborn Library

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
# Importing searborn
import seaborn as sns
# Looking at the available sample datasets in the seaborn library
print(sns.get_dataset_names())
```

### Load the fMRI Data Set

To load the data, simply use the load_dataset() function to get quick access to the example dataset. The .head() function returns the first n rows of what this data frame looks like as shown below:

```python
# Loading the dataframe
fmri = sns.load_dataset('fmri')
# Returning the first n rows
fmri.head()
```

We can describe the data using the .describe() function which automatically computes basic statistics for all continuous variables. Any NaN values are automatically skipped in these statistics.

This will show: the count of that variable, the mean, the standard deviation (std), the minimum value, the IQR (Interquartile Range: 25%, 50%, and 75%), and the maximum value.
```python
# Describing the data
fmri.describe()
```

### Preprocessing and Cleaning

This sample data set does not require extensive processing. Instead, I will clean the data for aesthetic purposes. First, I am going to rename the columns so that each variable is capitalized. Then I will set the index to begin the data frame with the Subject variable. Lastly, I am going to rename all the string values, to begin with, a capital letter.
To review the changes made, I will use the .head() function to return the updated data frame. These changes will then reflect in my visualizations.

```python
# Renaming the columns
fmri.columns = ['Subject','Timepoint','Event','Region','Signal']
# Setting the index to begin with the variable Subject
fmri=fmri.set_index('Subject')
# Capitalizing string values in the dataframe
fmri.replace({'frontal': 'Frontal', 'parietal': 'Parietal', 'cue': 'Cue', 'stim': 'Stim'}, inplace=True)
# Returning the first n rows of the updated dataframe
fmri.head()
```

## Plotting the Data
### Line Plot

The relplot function has the ability to create a line chart commonly used in time series analysis. In this example, we analyze and compare two sets of observations, Timepoint and Signal by comparing Region and Event in the same plot.

Using kind="line" offers flexibility by transforming the data before plotting. Observations are sorted by their x value, and repeated observations are aggregated. By default, the resulting plot shows the mean and 95% confidence interval (CI) for each unit.

```python
sns.relplot(x="Timepoint", y="Signal", hue="Region", style="Event",
            palette="ch:rot=-.25,hue=1,light=.75",
            dashes=False, markers=True, kind="line", data=fmri);
```

### Box Plot

Box plots are based on percentiles and give a quick way to visualize the distribution of data. The box plot below is shown in two dimensions with a third dimension added, a hue parameter. The hue parameter can be incorporated into the plot by coloring the points according to a third variable. In this example, we used the Region variable as the hue parameter.

The top and bottom of the box are the 75th and 25th percentiles, respectively. The median is shown by the horizontal line in the box. The lines referred to as whiskers, extend from the top and bottom to indicate the range for the bulk of the data.

```python
sns.boxplot(x="Timepoint", y="Signal", hue="Region",
            data=fmri,palette="ch:rot=-.25,hue=1,light=.75")
```

### Swarm Plot

A swarm plot can be drawn on its own and is sometimes known as a “beeswarm”. The points on the plot have been grouped based on the category to represent the distribution of values for the Region variable. The plot points show a relationship between the parietal and frontal areas of the brain with the Signal variable over time.

```python
sns.set(style='whitegrid')
sns.swarmplot(x="Timepoint",y="Signal",hue="Region", 
                  data=fmri,palette="ch:rot=-.25,hue=1,light=.75")
```

### Linear Regression Model

Seaborn has two main functions, regplot() and lmplot(), for visualizing a linear relationships as determined through a regression, commonly used for predictive analysis. While regplot() always shows a single relationship, lmplot() combines regplot() with FacetGrid (a multi-plot grid for plotting conditional relationships) to provide an easy interface to show a linear regression on “faceted” plots that allow you to explore interactions with up to three additional categorical variables. For this example, we will use the lmplot() function to plot the relationship between Timepoint and Signal with Region and Event.

Resource

```python
sns.lmplot(x="Timepoint", y="Signal", hue="Region",
           col="Event", row="Region", data=fmri,
```

We can see there is a negative correlation between Signal and Timepoint in both the Parietal and Frontal region of the stim event but not in the cue event.

## Conclusion

Now that we’ve explored multiple plots from the fMRI sample data frame from the Seaborn library, we can understand what visualizations work best for this type of data. I would conclude that the line plot was most effective in representing the data fully with the inclusion of the Region and the Event variables without much confusion. The linear regression model gave us a deeper meaning of the analysis capabilities within Seaborn and would also be a preferred plot for visualizations. The swarm plot and box plot closely resembles the same pattern as the line plot but doesn't really narrate what is going on within the data since we are only representing two categories with similar values in the plot. The same information we extracted from the plots is just as easily represented with the .describe() function.

Hopefully, this demonstration was able to help you understand fMRI a bit more as well as what the Seaborn library can do to visualize this unique type of data.
