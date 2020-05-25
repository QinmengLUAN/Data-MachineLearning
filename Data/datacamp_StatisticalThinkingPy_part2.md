# [Datacamp Statistical Thinking in Python (Part 2)](https://learn.datacamp.com/courses/statistical-thinking-in-python-part-2)

> [A tidy github notebook](https://github.com/wblakecannon/DataCamp/tree/master/07-statistical-thinking-in-python-(part-2))

## Parameter estimation by optimization
### Optimal parameters
* Parameter values that bring the model in closest agreement with
the data

Example: Notice how the value of tau given by the mean matches the data best. In this way, tau is an optimal parameter
```
# Plot the theoretical CDFs
plt.plot(x_theor, y_theor)
plt.plot(x, y, marker='.', linestyle='none')
plt.margins(0.02)
plt.xlabel('Games between no-hitters')
plt.ylabel('CDF')

# Take samples with half tau: samples_half
samples_half = np.random.exponential(tau / 2, 10000)

# Take samples with double tau: samples_double
samples_double = np.random.exponential(tau * 2, 10000)

# Generate CDFs from these samples
x_half, y_half = ecdf(samples_half)
x_double, y_double = ecdf(samples_double)

# Plot these CDFs as lines
_ = plt.plot(x_half, y_half)
_ = plt.plot(x_double, y_double)

# Show the plot
plt.show()
```

### Linear regression by least squares
* residual
* **Least squares with np.polyt()**
    - The process of finding the parameters for which the sum of the squares of the residuals is minimal   
* Linear regression
* The importance of EDA: Anscombe's quartet

### Generating bootstrap replicates
* The use of resampled data to perform statistical inference
* Resampling engine: np.random.choice()
    - E.g. `np.random.choice([1,2,3,4,5], size=5)`

### Bootstrap confidence intervals
* Confidence intervals: you can take percentiles of your bootstrap replicates to get your confidence interval. `np.percentile()`
    - Example:
```
# Draw bootstrap replicates of the mean no-hitter time (equal to tau): bs_replicates
bs_replicates = draw_bs_reps(nohitter_times, np.mean, size = 10000)

# Compute the 95% confidence interval: conf_int
conf_int = np.percentile(bs_replicates, [2.5, 97.5])

# Print the confidence interval
print('95% confidence interval =', conf_int, 'games')
```

### Pairs bootstrap
* Nonparametric inference: make no assumptions about the model or probability distribution underlying the data
* Pairs bootstrap for linear regression
    - Resample data in pairs
    - Compute slope and intercept from resampled data
    - Each slope and intercept is a bootstrap replicate
    - Compute condence intervals from percentiles of bootstrap replicates

## Introduction to hypothesis testing
* **Hypothesis testing**: Assessment of how reasonable the observed data are assuming a hypothesis is true
* **Test statistic**: A single number that can be computed from observed data and from data you simulate under the null hypothesis. It serves as a basis of comparison between the two.

### Formulating and simulating a hypothesis: permutation

### Test statistics and p-values
* p-value
    - The probability of obtaining a value of your test statistic that is at least as extreme as what was observed, under the assumption the null hypothesis is true.
    - NOT the probability that the null hypothesis is true
* Quiz: What is a p-value?
    - The p-value is generally a measure of: the probability of observing a test statistic equally or more extreme than the one you observed, given that the null hypothesis is true.
* **Statistical signicance**:
    - Determined by the smallness of a p-value


