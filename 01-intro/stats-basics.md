# Statistics basics

## Dividing data

### Percentiles (100 regions)

The $k$th percentile is the number which has $k$ % of the values below it. The data must be ranked. The 50th percentile is the median.

### Deciles (10 regions)

The percentiles divide the data into 100 equal regions. The deciles divide the data into 10 equal regions. The instructions are the same for finding a percentile, except instead of dividing by 100 in step 2, divide by 10.

### Quartiles (4 regions)

The quartiles divide the data into 4 equal regions. Instead of dividing by 100 in step 2, divide by 4.

Note: The 2nd quartile is the same as the median. The 1st quartile is the 25th percentile, the 3rd quartile is the 75th percentile.

The quartiles are commonly used (much more so than the percentiles or deciles). The TI-82 calculator will find the quartiles for you. Some textbooks include the quartiles in the five number summary.

### Hinges

The lower hinge is the median of the lower half of the data up to and including the median. The upper hinge is the median of the upper half of the data up to and including the median.

The hinges are the same as the quartiles unless the remainder when dividing the sample size by four is three (like 39 / 4 = 9 R_3).

The statement about the lower half or upper half including the median tends to be confusing to some students. If the median is split between two values (which happens whenever the sample size is even), the median isn't included in either since the median isn't actually part of the data.

#### Example 1: sample size of 20

The median will be in position 10.5. The lower half is positions 1 - 10 and the upper half is positions 11 - 20. The lower hinge is the median of the lower half and would be in position 5.5. The upper hinge is the median of the upper half and would be in position 5.5 starting with original position 11 as position 1 -- this is the original position 15.5.

#### Example 2: sample size of 21

The median is in position 11. The lower half is positions 1 - 11 and the upper half is positions 11 - 21. The lower hinge is the median of the lower half and would be in position 6. The upper hinge is the median of the upper half and would be in position 6 when starting at position 11 -- this is original position 16.

## Five Number Summary

The five number summary consists of the minimum value, lower hinge, median, upper hinge, and maximum value. Some textbooks use the quartiles instead of the hinges.

## Box and Whiskers Plot

A graphical representation of the five number summary. A box is drawn between the lower and upper hinges with a line at the median. Whiskers (a single line, not a box) extend from the hinges to lines at the minimum and maximum values.

## Interquartile Range (IQR)

The interquartile range is the difference between the third and first quartiles. That's it: Q3 - Q1


## Outliers

Outliers are extreme values. There are mild outliers and extreme outliers. The Bluman text does not distinguish between mild outliers and extreme outliers and just treats either as an outlier.

### Definition of outliers

An _outlier_ is an observation that lies an abnormal distance from other values in a random sample from a population. In a sense, this definition leaves it up to the analyst (or a consensus process) to decide what will be considered abnormal. Before abnormal observations can be singled out, it is necessary to characterize normal observations.

### Ways to describe data 

Two activities are essential for characterizing a set of data:

1.  Examination of the overall shape of the graphed data for important features, including symmetry and departures from assumptions. The chapter on [Exploratory Data Analysis (EDA)](https://www.itl.nist.gov/div898/handbook/eda/section3/eda3.htm) discusses assumptions and summarization of data in detail.
2.  Examination of the data for unusual observations that are far removed from the mass of data. These points are often referred to as outliers. Two graphical techniques for identifying outliers, [scatter plots](https://www.itl.nist.gov/div898/handbook/eda/section3/scattera.htm) and [box plots](https://www.itl.nist.gov/div898/handbook/eda/section3/boxplot.htm), along with an analytic procedure for detecting outliers when the distribution is normal ([Grubbs' Test](https://www.itl.nist.gov/div898/handbook/eda/section3/eda35h.htm)), are also discussed in detail in the EDA chapter.


### Box plot construction

The box plot is a useful graphical display for describing the behavior of the data in the middle as well as at the ends of the distributions. The box plot uses the [median](https://www.itl.nist.gov/div898/handbook/eda/section3/eda351.htm) and the lower and upper quartiles (defined as the 25th and 75th [percentiles](https://www.itl.nist.gov/div898/handbook/prc/section2/prc252.htm)). If the lower quartile is Q1 and the upper quartile is Q3, then the difference (Q3 - Q1) is called the interquartile range or IQR.

### Box plots with fences 

A box plot is constructed by drawing a box between the upper and lower quartiles with a solid line drawn across the box to locate the median. The following quantities (called _fences_) are needed for identifying extreme values in the tails of the distribution:

1.  lower inner fence: $Q1 - 1.5 \ \text{IQR}$
2.  upper inner fence: $Q3 + 1.5 \ \text{IQR}$
3.  lower outer fence: $Q1 - 3 \ \text{IQR}$
4.  upper outer fence: $Q3 + 3 \ \text{IQR}$

### Outlier detection criteria

A point beyond an inner fence on either side is considered a **mild outlier**. A point beyond an outer fence is considered an **extreme outlier**.


#### Extreme Outliers

Extreme outliers are any data values which lie more than 3.0 times the interquartile range below the first quartile or above the third quartile. x is an extreme outlier if

$$x < Q1 - 3 \ \text{IQR}$$

or

$$x > Q3 + 3 \ \text{IQR}$$

#### Mild Outliers

Mild outliers are any data values which lie between 1.5 times and 3.0 times the interquartile range below the first quartile or above the third quartile. x is a mild outlier if ...

$$Q1 - 3 \ \text{IQR} \le x < Q1 - 1.5 \ \text{IQR}$$

or

$$Q1 + 1.5 \ \text{IQR} < x \le Q3 + 3 \ \text{IQR}$$


## Density plot

A density plot is a representation of the distribution of a numeric variable. It uses a kernel density estimate to show the probability density function of the variable. It is a smoothed version of the histogram and is used in the same concept.


## References

- https://people.richland.edu/james/lecture/m170/ch03-pos.html
- https://www.itl.nist.gov/div898/handbook/prc/section1/prc16.htm
- https://www.simplypsychology.org/boxplots.html