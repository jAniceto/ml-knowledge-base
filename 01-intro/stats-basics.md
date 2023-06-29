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


## Density plot

A density plot is a representation of the distribution of a numeric variable. It uses a kernel density estimate to show the probability density function of the variable. It is a smoothed version of the histogram and is used in the same concept.


## References

- https://people.richland.edu/james/lecture/m170/ch03-pos.html
- https://www.itl.nist.gov/div898/handbook/prc/section1/prc16.htm
- https://www.simplypsychology.org/boxplots.html