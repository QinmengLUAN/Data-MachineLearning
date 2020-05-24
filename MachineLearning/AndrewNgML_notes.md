# [Andrew Ng Machine Learning](https://www.coursera.org/learn/machine-learning/home/welcome) Notes

* Feature normalisation
    - How to do feature normalisation by using **feature scaling**(i.e. divided by the range) and **mean normalisation**(i.e minius the mean value)?
```
$x_{i}$: feature
$\mu_{i}$: mean (avarage) value of x (feature)
$s_{i}$: range of the feature (max - min) or you can set $s_{i}$ as the standard deviation of the feature.
**Normalisation**: $s_{i}$: = ($x_{i} - \mu_{i}$) / $s_{i}$
```
    - Reasons for using feature normalisation:
        + It speeds up gradient decent by making it require fewer iterations to get to a good solution.
