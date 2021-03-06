# Discrete-Time Generator

The discrete-time economic scenario generator simulates the trajectories of 11 Australian economic and financial variables: 

(1) 3-month zero-coupon bond yields,

(2) 10-year zero-coupon bond spread,

(3) NSW home value index,

(4) NSW home rental yields,

(5) Australia GDP,

(6) Australia CPI,

(7) S&P/ASX200 closing price,

(8) Australian dollar trade-weighted index,

(9) Australia mortgage rate,

(10) NSW unemployment rate,

(11) Stochastic discount factors (pricing kernels).

Factors (3)(5)-(8) were transformed to continuously compounded growth rates for consistency. Factors (1)-(8) (in rates) were fitted using a Vector Autoregressive model (VAR), factors (9)-(10) were respectively expressed as a fixed margin over factors (1)-(2) due to strong correlations, while factor (11) is derived from the fitted VAR model with arbitrage-free assumptions. Further details on model parameter estimation and forecasts can be found in note (a) below. 

Vector Autoregression (VAR) is a regression of a time series where the ouput depends linearly on the past values of itself, and the past values of other variables, up to some specfied order: 

![](https://latex.codecogs.com/svg.image?\mathbf{z}_{t}&space;=&space;\mathbf{\mu}&space;&plus;&space;\Phi_{1}\mathbf{z}_{t-1}&plus;&space;\Phi_{2}\mathbf{z}_{t-2}&plus;\cdots&plus;&space;\Phi_{p}\mathbf{z}_{t-p}&space;&plus;&space;\mathbf{\epsilon},)

where  

* $\mathbf{z}_{t}$ is the vector of economic variables,

* $\boldsymbol{\mu}$ is the vector of intercepts,

* $\Phi_{i}$, for $i=1,\cdots, p$ are coefficient matrices of size $n \times n$ with $n$ being the number of economic variables and $p$ the lags. 

* $\boldsymbol{\epsilon}$ is a vector of white noises. 

The stochastic discount factor is defined as: 

![](https://latex.codecogs.com/svg.image?s_{t&plus;1}&space;=&space;\exp&space;\left(-&space;\mathbf{e}_1&space;^\top&space;\mathbf{z}_t&space;-&space;\frac{1}{2}&space;\mathbf{\lambda}_t^\top&space;\mathbf{\lambda}_t&space;-&space;\mathbf{\lambda}_t^\top&space;\mathbf{\epsilon}_{t&plus;1}&space;\right),)

where ![](https://latex.codecogs.com/svg.image?\mathbf{e}_1^\top&space;\mathbf{z}_t) and ![](https://latex.codecogs.com/svg.image?\mathbf{\epsilon}_t) respectively denote the 3-month zero-coupon bond rates and  the white noises from the fitted VAR model, and ![](https://latex.codecogs.com/svg.image?\mathbf{\lambda}_t) is the market price of risk process which is assumed to be affine over factors (1)-(8). 


---


**esg_var_simulations(num_years = 5, num_paths = 10, frequency = 'quarter', perc_change = FALSE, return_sdf = TRUE)**

&nbsp;&nbsp; **Parameters:**

&nbsp;&nbsp;&nbsp;&nbsp; num_years : numeric

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *integer denoting number of years to forecast from 01-01-2021*

&nbsp;&nbsp;&nbsp;&nbsp; num_paths : numeric

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *integer denoting number of simulations to make for each variable*

&nbsp;&nbsp;&nbsp;&nbsp; frequency : character

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *'year', 'quarter' or 'month' denoting the simulation frequency*

&nbsp;&nbsp;&nbsp;&nbsp; perc_change : logical

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *set TRUE for outputs to be expressed as period-by-period percent change. The reference level, i.e., the original values in the first output period, will be appended above the percentage changes for each variable and each trajectory. See note (b) below.*

&nbsp;&nbsp;&nbsp;&nbsp; return_sdf : logical 

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *set TRUE to return the stochastic discount factors*

&nbsp;&nbsp; **Returns:**

&nbsp;&nbsp;&nbsp;&nbsp; A list of 10 dataframes containing simulated trajectories of the 10 variables, or a list of 11 dataframes including the simulated stochastic discount factors if return_sdf is set TRUE from 01-01-2021. See note (c) for explanations on the negativity of output values. 

&nbsp;&nbsp; **Usage:**

```r
# simulate 10 years of data
sim <- esg_var_simulations(num_years = 10, num_paths = 10000, frequency = 'year')

# suppose we wish to look at the 3 months zero coupon bonds
sim$zcp3m_yield

# if we wanted a specific trajectory, say 103
sim$zcp3m_yield$trajectory_103
```

&nbsp;&nbsp; **Notes:**

  (a) Procedure for parameter estimation: 
  
  * Transformed all inputs to continuously compounded rates, tested for correlation, causality, and stationarity. 
        
  * Found the optimal lag order for Vector Autoregression using AIC, SIC, HQC. 
        
  * Fitted the VAR model using ordinary least squares. This was followed by evaluation. 
        
  * Associated the stochastic discount factors with VAR factors and nominal bond prices, estimated the market price of risk parameters by minimising the sum of squared error. 
        
   Procedure for forecasts: Generated factors (1)-(8) using Vector Autoregression formula. From the generated paths and noises, derived stochastic discount factors. 
  
  (b) Large values of percentage change can appear if the original forecasts are near-zero, or if the Gaussian noise is large, though with low probabilities. This happens especially for interest rates in the first few periods due to historical-low rates in 2021. 

  (c) Negative values for rate factors i.e., factors (1)(2)(4)(9)(10) are theoretically allowed as Vector Autoregression models assume that the noise follow a Gaussian distribution. Index factors, i.e., factors (3)(5)-(8), on the other hand, are all positive. 


&nbsp;&nbsp; **References:**

Daniel H Alai, Hua Chen, Daniel Cho, Katja Hanewald, and Michael Sherris. Developing equity release markets: Risk analysis for reverse mortgages and home reversions. _North American Actuarial Journal_, 18(1):217???241, 2014.

Andrew Ang and Monika Piazzesi. A no-arbitrage vector autoregression of term structure dynamics with macroeconomic and latent variables. _Journal of Monetary economics_, 50(4):745???787, 2003.

Andrew Ang, Monika Piazzesi, and Min Wei. What does the yield curve tell us about gdp growth? _Journal of econometrics_, 131(1-2):359???403, 2006.




















