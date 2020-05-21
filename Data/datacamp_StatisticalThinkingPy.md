# [Datacamp Statistical Thinking in Python (Part 1)](https://learn.datacamp.com/courses/statistical-thinking-in-python-part-1)

## Graphical exploratory data analysis (EDA)
### Plotting a histogram
- lable the axis
- set the bins
- **Seaborn**
    + sns.set(): use the default style
- Bins: **"square root rule"**:

    + is a commonly-used rule of thumb for choosing number of bins: choose the number of bins to be the square root of the number of samples.

Example:
```
# Import plotting modules
import matplotlib.pyplot as plt
import seaborn as sns

# Set default Seaborn style
sns.set()

# Compute number of data points: n_data
n_data = len(versicolor_petal_length)

# Number of bins is the square root of number of data points: n_bins
n_bins = np.sqrt(n_data)

# Convert number of bins to integer: n_bins
n_bins = int(n_bins)

# Plot the histogram
_ = plt.hist(versicolor_petal_length, bins = n_bins)

# Label axes
_ = plt.xlabel('petal length (cm)')
_ = plt.ylabel('count')

# Show histogram
plt.show()
```

### Plot all of your data: Bee swarm plots
* **Binning bias**: avoided by using Bee swarm plots
```
# Create bee swarm plot with Seaborn's default settings
df.head()
_ = sns.swarmplot(x='species', y='petal length (cm)', data=df)
# Label the axes
_ = plt.xlabel('species')
_ = plt.ylabel('sepal length (cm)')

# Show the plot
plt.show()
```
### Plot all of your data: Emperical cumulative distribution function (**ECDFs**)

> Edges have overlapping data points, using Bee swarm plots could cause obfuscating -> **ECDFs** could sove this problem.

```
def ecdf(data):
    """Compute ECDF for a one-dimensional array of measurements."""
    # Number of data points: n
    n = len(data)

    # x-data for the ECDF: x
    x = np.sort(data)

    # y-data for the ECDF: y
    y = np.arange(1, n + 1) / n

    return x, y
```

Plot an ECDF:
```
# Compute ECDF for versicolor data: x_vers, y_vers
x_vers, y_vers = ecdf(versicolor_petal_length)

# Generate plot
_ = plt.plot(x_vers, y_vers, marker='.', linestyle='none')

# Label the axes
_ = plt.xlabel('versicolor_petal_length')
_ = plt.ylabel('ECDF')

# Display the plot
plt.margins(0.02)
plt.show()
```

Plot comparison of ECDFs
```
# Compute ECDFs
x_set, y_set = ecdf(setosa_petal_length)
x_vers, y_vers = ecdf(versicolor_petal_length)
x_virg, y_virg = ecdf(virginica_petal_length)

# Plot all ECDFs on the same plot
plt.plot(x_set, y_set, marker='.', linestyle='none')
plt.plot(x_vers, y_vers, marker='.', linestyle='none')
plt.plot(x_virg, y_virg, marker='.', linestyle='none')

# Annotate the plot
plt.legend(('setosa', 'versicolor', 'virginica'), loc='lower right')
_ = plt.xlabel('petal length (cm)')
_ = plt.ylabel('ECDF')

# Display the plot
plt.show()
```

## Quantitative exploratory data analysis
* Mean, Median, IQR plot, outliers
* Variance, Standard deviation, **Covariance**, **Correlation**
    - Covariance: Cov(X,Y) = E[(X - E[X])(Y - E[Y])] = E[XY] - E[X]E[Y]
    - Pearson correlation coeffient: 
        - /rho = covariance / [std(x)std(y)] = variability due to codependence / independent variability
        - /rho ranges from -1 to 1
    * `np.cov(x,y)`: coverance matrix returns a 2D array where entries [0,1] and [1,0] are the covariances
    * `np.corrcoef(x,y)`
## Thinking probabilistically-- Discrete variables
* **Statistical inference**: involves taking your data to probabilistic conclusions about what you would expect if you took even more data, and you can make decisions based on these conclusions.
* Random number generators and hacker statistics
    * `np.random.random()`:  returns a random number between zero and one
* Probability distributions and stories: The Binomial distribution
    *  Proabability mass function (PMF): the set of probabilities of discrete outcomes.  
*  Binomial distribution
        -  The number r of successes in n Bernoulli trials with
probability p of success, is Binomially distributed.
        -  Example: The number r of heads in 4 coin flips with probability
0.5 of heads, is Binomially distributed
```
# Sampling out binomial distribution
# Take 10,000 samples out of the binomial distribution: n_defaults
n_defaults = np.random.binomial(100, 0.05, size = 10000)

# Compute CDF: x, y
x, y = ecdf(n_defaults) 

# Plot the CDF with axis labels
_ = plt.plot(x, y, marker = '.', linestyle = 'none')
plt.margins(0.02)
_ = plt.xlabel('number of defaults')
_ = plt.ylabel('CDF')

# Show the plot
plt.show()
```
+  Possion distribution
    * **Possion process**: The timing of the next event is completely independent of when the previous event happened
        * Examples of possion processes:
            + Natural births in a given hospital
            + Hit on a website during a given hour
            + Meteor strikes
            + Molecular collisions in a gas
            + Aviation incidents
            + Buses in Poissonville
    * **Poisson distribution**: The number r of arrivals of a Poisson process in a given time interval with average rate of λ arrivals per interval is Poisson distributed.
        * The number r of hits on a website in one hour with an average hit rate of 6 hits per hour is Poisson distributed.

> Relationship between Binomial and Poisson distributions:
> 
> You just heard that the Poisson distribution is **a limit of the Binomial distribution for rare events**. This makes sense if you think about the stories. 
> 
> Say we do a Bernoulli trial every minute for an hour, each with a success probability of 0.1. We would do 60 trials, and the number of successes is Binomially distributed, and we would expect to get about 6 successes. This is just like the Poisson story we discussed in the video, where we get on average 6 hits on a website per hour. So, **the Poisson distribution with arrival rate equal to np approximates a Binomial distribution for n Bernoulli trials with probability p of success (with n large and p small)**. Importantly, the Poisson distribution is often simpler to work with because it has only one parameter instead of two for the Binomial distribution.

> Example: When we have rare events (low p, high n), the Binomial distribution is Poisson. This has a single parameter, the mean number of successes per time interval, in our case the mean number of no-hitters per season.
## Thinking probabilistically -- Continuous variables
* Probability density functions (PDF)
    - Continous
* Normal distribution
    - Describes a continuous variable whose PDF has a single symmetric peak.
* Normal distribution: properties and warning