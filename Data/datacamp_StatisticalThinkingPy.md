# Datacamp [Statimastical Thinking in Python (Part 1)](https://learn.datacamp.com/courses/statistical-thinking-in-python-part-1)

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
## Thinking probabilistically-- Discrete variables
## Thinking probabilistically-- Continuous variables