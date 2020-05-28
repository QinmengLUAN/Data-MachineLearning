#  Datacamp [Time Series with Python](https://learn.datacamp.com/skill-tracks/time-series-with-python)

## Correlation and Autocorrelation
* Goals of Course
    - Learn about time series models
    - Fit data to a times series model
    - Use the models to make forecasts ofthe future
    - Learn how to use the relevant statistical packages in Python
    - Provide concrete examples of how these models are used
* A "Thin" Application of Time Series

Example 1_datetime:
```
df.index = pd.to_datetime(df.index) #Changing an index to datetime

df.plot() #Plotting data

df['2012'] #Slicing data
```

Example 2_plot:
```
# From previous step
diet.index = pd.to_datetime(diet.index)

# Plot the entire time series diet and show gridlines
diet.plot(grid=True)
plt.show()
```

Example 3_Slice:
```
# Slice the dataset to keep only 2012
diet2012 = diet['2012']
```

Example 4_Merge:
```
# Merge stocks and bonds DataFrames using join()
stocks_and_bonds = stocks.join(bonds, how='inner')
```
* Correlation of Two Time Series

### Whatis a Regression?
* i.e. Ordinary Least Squares (OLS) regression

In statsmodels:
import statsmodels.api as sm
`sm.OLS(y, x).fit()`

In numpy:
`np.polyfit(x, y, deg=1)`

In pandas:
`pd.ols(y, x)`

In scipy:
`from scipy import stats`
`stats.linregress(x, y)`

**Warning**: the order of `x` and `y` is not consistent across packages

### Relationship Between R-Squared and Correlation
* [corr(x, y)]^2 = R^2 (or R-squared)
* sign(corr) = sign(regression slope)
```
Example:
R-Squared = 0.753
Slope is positive
correlation = + sqrt(R-Squared)
```

### Autocorrelation 
* Correlation of a time series with a lagged copyof itself, i.e. **lag one correlation**, also called serial correlation
    - Negative autocorrelation: call it "mean reverting"
    - Positive autocorrelation: call it "trend following"
* Autocorrelation Function
    - Autocorrelation Function (ACF): The autocorrelation as a function of the lag
    - Equals one at lag-zero
    - Interesting information beyond lag-one

## Some Simple Time Series
### Autocorrelation Function (ACF)
* **ACF: the autocorrelation as a function of the lag**
* Confidence interval:
    * **Alpha**: sets the width of confidence interval, e.g., alpha = 0.5 means 5% chance that if true autocorrelation is zero.
    * Confidence bands are wider if
        * Alpha lower
        * Fewer observations
    + Under some simplified assumptions, 95% confidence bands are +-2/sqrt(N)
Random bands
    * If you want no confidence band, set `alpha=1`
* **White noise**:
    - White noise is a series with: constant mean, constant variance, zero autocorrelations at all lags.
    - Special case: if data has normal distribution, then Gaussian White Noise.
    - Generate White Noise: `returns = np.random.normal(loc=0.02, scale=0.05, size=1000)`

>Notice that for a white noise time series, all the autocorrelations are close to zero, so the past will not help you forecast the future.

### Random Walk
* Check whether a stock price is a random work

Step 1: Generate a Random Walk
```
# Generate 500 random steps with mean=0 and standard deviation=1
steps = np.random.normal(loc=0, scale=1, size=500)

# Set first element to 0 so that the first price will be the starting stock price
steps[0]=0

# Simulate stock prices, P with a starting price of 100
P = 100 + np.cumsum(steps)
```

Step 2: Get the Drift
```
# Generate 500 random steps
steps = np.random.normal(loc=0.001, scale=0.01, size=500) + 1

# Set first element to 1
steps[0]=1

# Simulate the stock price, P, by taking the cumulative product
P = 100 * np.cumprod(steps)

# Plot the simulated stock prices
plt.plot(P)
plt.title("Simulated Random Walk with Drift")
plt.show()
```

Step 3: Are Stock Prices a Random Walk?
```
# Import the adfuller module from statsmodels
from statsmodels.tsa.stattools import adfuller

# Run the ADF test on the price series and print out the results
results = adfuller(AMZN['Adj Close'])
print(results)

# Just print out the p-value, results[0] is the test statistic, and results[1] is the p-value
print('The p-value of the test on prices is: ' + str(results[1]))
```

Step 4: How About Stock Returns?-- not a random walk
```
# Import the adfuller module from statsmodels
from statsmodels.tsa.stattools import adfuller

# Create a DataFrame of AMZN returns
AMZN_ret = AMZN.pct_change()

# Eliminate the NaN in the first row of returns
AMZN_ret = AMZN_ret.dropna()

# Run the ADF test on the return series and print out the p-value
results = adfuller(AMZN_ret['Adj Close'])
print('The p-value of the test on returns is: ' + str(results[1]))
```
### Stationarity
* **Strong stationarity**: entire distribution of data is time-invariant
* **Weak stationarity**: mean, variance and autocorrelation are timeinvariant (i.e.,for autocorrelation, corr(X_t, X_(t-τ))is only a function
of τ).
* Why do we care stationarity?
    - If parameters vary with time,too many parameters to estimate
    - Can only estimate a parsimonious model with a few parameters
* Examples: random walk, Seasonality in series, Change in Mean or Standard Deviation over time
* Transforming Nonstationary Series Into Stationary Series:
    - e.g. take the first difference `SPY.diff()` for stock price
    - Take the seasonal difference (by taking the difference with lag of 4)`xxx.diff(4)`,
    - # Log to eliminate the exponential growth, then seasonal difference, `plt.plot(np.log(AMZN).diff(4))`

Example:
```
# Import the plot_acf module from statsmodels
from statsmodels.graphics.tsaplots import plot_acf

# Seasonally adjust quarterly earnings
HRBsa = HRB.diff(4)

# Print the first 10 rows of the seasonally adjusted series
print(HRBsa.head(10))

# Drop the NaN data in the first four rows
HRBsa = HRBsa.dropna()

# Plot the autocorrelation function of the seasonally adjusted series
plot_acf(HRBsa)
plt.show()
```
## Autoregressive (AR) Models
* Describe AR Model:
```
# Import the ARMA module from statsmodels
from statsmodels.tsa.arima_model import ARMA

# Fit an AR(1) model to the first simulated data
mod = ARMA(simulated_data_1, order=(1,0))
res = mod.fit()

# Print out summary information on the fit
print(res.summary())

# Print out the estimate for the constant and for phi
print("When the true phi=0.9, the estimate of phi (and the constant) are:")
print(res.params)
```
* Choosing the Right Model
    - Two techniques to determine order:
        + **Partial Autocorrelation Function (PACF)**
        + **Information Criteria**
* Information criteria: adjusts goodness-of-fit for number of parameters
    - Two popular adjusted goodness-of-fit measures
        + **AIC** (Akaike Information Criterion)
        + **BIC** (Bayesian Information Criterion)

Example:
```
# Import the modules for simulating data and for plotting the PACF
from statsmodels.tsa.arima_process import ArmaProcess
from statsmodels.graphics.tsaplots import plot_pacf

# Simulate AR(1) with phi=+0.6
ma = np.array([1])
ar = np.array([1, -0.6])
AR_object = ArmaProcess(ar, ma)
simulated_data_1 = AR_object.generate_sample(nsample=5000)

# Plot PACF for AR(1)
plot_pacf(simulated_data_1, lags=20)
plt.show()

# Simulate AR(2) with phi1=+0.6, phi2=+0.3
ma = np.array([1])
ar = np.array([1, -0.6, -0.3])
AR_object = ArmaProcess(ar, ma)
simulated_data_2 = AR_object.generate_sample(nsample=5000)

# Plot PACF for AR(2)
plot_pacf(simulated_data_2, lags=20)
plt.show()
```
Example: Estimate Order of Model: Information Criteria
```
# Import the module for estimating an ARMA model
from statsmodels.tsa.arima_model import ARMA

# Fit the data to an AR(p) for p = 0,...,6 , and save the BIC
BIC = np.zeros(7)
for p in range(7):
    mod = ARMA(simulated_data_2, order=(p,0))
    res = mod.fit()
# Save BIC for AR(p)    
    BIC[p] = res.bic
    
# Plot the BIC as a function of p
plt.plot(range(1,7), BIC[1:7], marker='o')
plt.xlabel('Order of AR Model')
plt.ylabel('Bayesian Information Criterion')
plt.show()
```
* Describe Model
## Estimation and Forecasting an MA Model
* Mathematical Description of MA(1) Model: R_t = μ + ϵ + θ ϵ
* Since only one lagged error on right hand side,this is called: MA model of order 1, or MA(1) model
* MA parameter is θ
* Stationary for all values of θ
    - Negative θ: One-Period Mean Reversion
    - Positive θ: One-Period Momentum
    - Note: One-period autocorrelation is θ/(1 + θ ), not θ
