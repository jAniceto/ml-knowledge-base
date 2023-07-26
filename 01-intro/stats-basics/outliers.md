# Outliers

Outliers are extreme values. There are mild outliers and extreme outliers. The Bluman text does not distinguish between mild outliers and extreme outliers and just treats either as an outlier.

## Definition of outliers

An _outlier_ is an observation that lies an abnormal distance from other values in a random sample from a population. In a sense, this definition leaves it up to the analyst (or a consensus process) to decide what will be considered abnormal. Before abnormal observations can be singled out, it is necessary to characterize normal observations.

## Ways to describe data 

Two activities are essential for characterizing a set of data:

1) Examination of the overall shape of the graphed data for important features, including symmetry and departures from assumptions. The chapter on [Exploratory Data Analysis (EDA)](https://www.itl.nist.gov/div898/handbook/eda/section3/eda3.htm) discusses assumptions and summarization of data in detail.

2) Examination of the data for unusual observations that are far removed from the mass of data. These points are often referred to as outliers. Two graphical techniques for identifying outliers, [scatter plots](https://www.itl.nist.gov/div898/handbook/eda/section3/scattera.htm) and [box plots](https://www.itl.nist.gov/div898/handbook/eda/section3/boxplot.htm), along with an analytic procedure for detecting outliers when the distribution is normal ([Grubbs' Test](https://www.itl.nist.gov/div898/handbook/eda/section3/eda35h.htm)), are also discussed in detail in the EDA chapter.


## Box plot construction

The box plot is a useful graphical display for describing the behavior of the data in the middle as well as at the ends of the distributions. The box plot uses the [median](https://www.itl.nist.gov/div898/handbook/eda/section3/eda351.htm) and the lower and upper quartiles (defined as the 25th and 75th [percentiles](https://www.itl.nist.gov/div898/handbook/prc/section2/prc252.htm)). If the lower quartile is Q1 and the upper quartile is Q3, then the difference (Q3 - Q1) is called the interquartile range or IQR.

## Box plots with fences 

A box plot is constructed by drawing a box between the upper and lower quartiles with a solid line drawn across the box to locate the median. The following quantities (called _fences_) are needed for identifying extreme values in the tails of the distribution:

1.  lower inner fence: $Q1 - 1.5 \ \text{IQR}$
2.  upper inner fence: $Q3 + 1.5 \ \text{IQR}$
3.  lower outer fence: $Q1 - 3 \ \text{IQR}$
4.  upper outer fence: $Q3 + 3 \ \text{IQR}$

More on boxplots [here](boxplot.md).

## Outlier detection criteria

A point beyond an inner fence on either side is considered a **mild outlier**. A point beyond an outer fence is considered an **extreme outlier**.


### Extreme Outliers

Extreme outliers are any data values which lie more than 3.0 times the interquartile range below the first quartile or above the third quartile. x is an extreme outlier if

$$x < Q1 - 3 \ \text{IQR}$$

or

$$x > Q3 + 3 \ \text{IQR}$$

### Mild Outliers

Mild outliers are any data values which lie between 1.5 times and 3.0 times the interquartile range below the first quartile or above the third quartile. x is a mild outlier if ...

$$Q1 - 3 \ \text{IQR} \le x < Q1 - 1.5 \ \text{IQR}$$

or

$$Q1 + 1.5 \ \text{IQR} < x \le Q3 + 3 \ \text{IQR}$$