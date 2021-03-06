# Transition Probability Matrix 

The Cox Hazard model produces continuous hazard rates that will be discretized to produce
piecewise constant rates. 

The process is repeated for each of the 12 transition rates. The rates at each age are combined
into transition rate matrices for that age. Taking the matrix exponential changes that to 
transition probability matrices. 

!!! note
    The transition probability matrices are deterministic for each individual 
    with the Static and Trend models, but stochastic with the Frailty model.

---
### Get Transition Rates at a Certain Age

**health5_get_trans_rates(model, params, age, gender, i, latent)**

&nbsp;&nbsp; **Parameters**

&nbsp;&nbsp;&nbsp;&nbsp; model : character

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *'S' for static model, 'T' for trend model, 'F' for frailty model*

&nbsp;&nbsp;&nbsp;&nbsp; params : dataframe

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *Matrix of estimated parameters of the five-state static, trend, and frailty models. The rows are $\beta$, $\gamma^{\text{age}}$, $\gamma^{\text{f}}$, $\phi$, $\alpha$, and the columns are 1-12 transition types. (Generally, use params=US_HRS_5. Please refer to US_HRS_5 for the detailed construction of the matrix.)*

&nbsp;&nbsp;&nbsp;&nbsp; age : numeric

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *integer denoting the age*

&nbsp;&nbsp;&nbsp;&nbsp; gender : numeric

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *0 for male, 1 for female*

&nbsp;&nbsp;&nbsp;&nbsp; i : numeric

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *integer for the wave index = (interview year - 1998)/2 + 1*

&nbsp;&nbsp;&nbsp;&nbsp; latent : numeric

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *initial value of the latent factor, normally take the value 0*

&nbsp;&nbsp; **Returns:**

&nbsp;&nbsp;&nbsp;&nbsp; 5 by 5 matrix containing the transition probabilities between states at the input age, the states are H, M, D, MD, Dead for columns and rows.

&nbsp;&nbsp; **Usage:**

```r
# for male aged 65 at wave index i, using the frailty model with parameters 'param'
transition_rates=health5_get_trans_rates(model='F', params=US_HRS_5, age=65, gender=0, i=8, latent=0)
```

---
### Get a Transition Probability Matrix at a Certain Age

**health5_get_trans_probs(model, params, age, gender, i, latent)**

&nbsp;&nbsp; **Parameters**

&nbsp;&nbsp;&nbsp;&nbsp; model : character

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *'S' for static model, 'T' for trend model, 'F' for frailty model*

&nbsp;&nbsp;&nbsp;&nbsp; params : dataframe

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *Matrix of estimated parameters of the five-state static, trend, and frailty models. The rows are $\beta$, $\gamma^{\text{age}}$, $\gamma^{\text{f}}$, $\phi$, $\alpha$, and the columns are 1-12 transition types. (Generally, use params=US_HRS_5. Please refer to US_HRS_5 for the detailed construction of the matrix.)*

&nbsp;&nbsp;&nbsp;&nbsp; age : numeric

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *integer denoting the age*

&nbsp;&nbsp;&nbsp;&nbsp; gender : numeric

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *0 for male, 1 for female*

&nbsp;&nbsp;&nbsp;&nbsp; i : numeric

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *integer for the wave index = (interview year - 1998)/2 + 1*

&nbsp;&nbsp;&nbsp;&nbsp; latent : numeric

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *initial value of the latent factor, normally take the value 0*

&nbsp;&nbsp; **Returns:**

&nbsp;&nbsp;&nbsp;&nbsp; 12 by 1 vector containing the transition rates for transition types 1 to 12 at the input age

&nbsp;&nbsp; **Usage:**

```r
# for male aged 65 at wave index i, using the frailty model with parameters 'param'
transition_probabilities=health5_get_trans_probs(model='F', params=US_HRS_5, age=65, gender=0, i=8, latent=0)
```

---
### Get Full List of Transition Probability Matrices from Initial Age to Age 110

**health5_get_list_trans_prob_matrix(model, params, init_age, gender, i)**

&nbsp;&nbsp; **Parameters**

&nbsp;&nbsp;&nbsp;&nbsp; model : character

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *'S' for static model, 'T' for trend model, 'F' for frailty model*

&nbsp;&nbsp;&nbsp;&nbsp; params : dataframe

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *Matrix of estimated parameters of the five-state static, trend, and frailty models. The rows are $\beta$, $\gamma^{\text{age}}$, $\gamma^{\text{f}}$, $\phi$, $\alpha$, and the columns are 1-12 transition types. (Generally, use params=US_HRS_5. Please refer to US_HRS_5 for the detailed construction of the matrix.)*

&nbsp;&nbsp;&nbsp;&nbsp; init_age : numeric

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *integer denoting the initial age*

&nbsp;&nbsp;&nbsp;&nbsp; gender : numeric

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *0 for male, 1 for female*

&nbsp;&nbsp;&nbsp;&nbsp; i : numeric

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *integer for the wave index = (interview year - 1998)/2 + 1*

&nbsp;&nbsp; **Returns:**

&nbsp;&nbsp;&nbsp;&nbsp; List of 5x5 transition probability matrices, from the initial age to age 110.

&nbsp;&nbsp; **Usage:**

```r
# for male aged 65 at wave index i, using the frailty model with parameters 'param'
trans_prob_matrix_age65to110 <- health5_get_list_trans_prob_matrix(model='F', params=US_HRS_5, init_age=65, gender=0, i=8)
```


















