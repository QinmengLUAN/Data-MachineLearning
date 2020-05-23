# [Datacamp Statistical Thinking in Python (Part 2)](https://learn.datacamp.com/courses/statistical-thinking-in-python-part-2)

> [A tidy github notebook](https://github.com/wblakecannon/DataCamp/tree/master/04-statistical-thinking-in-python-(part2))

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
## Generating bootstrap replicates
## Bootstrap confidence intervals


