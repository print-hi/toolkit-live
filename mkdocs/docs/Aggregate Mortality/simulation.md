# Simulating Life Paths

One-year death probabilities derived from previous sections can also be used to simulate life paths
for a single individual or a cohort of individuals. Each individual or cohort will be alive at a specified initial age, and we sample
deaths according to the one-year death probabilities given at each age.

When simulating for an individual, we keep track of their dead or alive status at each age.
When simulating for a cohort, we keep track of the number of people still alive at each age.

---

### Simulate Individual Life Path

**sim_indiv_path(init_age, sex = "F", death_probs = NULL, closure_age = 130, n_sim = 10000)**

&nbsp;&nbsp; **Parameters:**

&nbsp;&nbsp;&nbsp;&nbsp; init_age : numeric

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *integer denoting initial age of individuals*

&nbsp;&nbsp;&nbsp;&nbsp; sex : character

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *character denoting the gender of individuals, "F" for female and
"M" for male*

&nbsp;&nbsp;&nbsp;&nbsp; death_probs : vector

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *vector of 1-year death probabilities. If not supplied, an M7
age-period-cohort model will be* 

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *fitted on `mortality_AUS_data` to produce forecasted
death probabilities for individuals* 

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *starting at `init_age` in 2022*

&nbsp;&nbsp;&nbsp;&nbsp; closure_age : numeric

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *maximum age*

&nbsp;&nbsp;&nbsp;&nbsp; n_sim : numeric

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *integer denoting number of path simulations*

&nbsp;&nbsp; **Returns:**

&nbsp;&nbsp;&nbsp;&nbsp; a matrix where each row represents an individual's dead (-1) or alive (0) status
across the 

&nbsp;&nbsp;&nbsp;&nbsp; years

An example looks like: 

\begin{pmatrix}
0 & -1 & -1 & -1 & \ldots & -1 \\
0 & 0 & 0 & -1 & \ldots & -1 \\
 &  &  \vdots & & & \\
0 & 0 & 0 & 0 & \ldots & -1 \\
0 & 0 & -1 & -1 & \ldots & -1
\end{pmatrix}

Note that the first column of the matrix will always be 0 as everyone is alive at the initial age,
and the last column will always be -1, signifying everyone will be dead at age 131 (if we assume 130 is the
maximum age one can be). 

&nbsp;&nbsp; **Usage:**

```r
# Simulate life paths for females starting at age 60
sim_indiv_path(init_age = 60, sex = "F")

# Suppose we want to use period 1-yr death probabilities instead
AUS_male_rates <- mortality_AUS_data$rate$male
ages <- mortality_AUS_data$age # 0:110
old_ages <- 91:130
AUS_male_qx <- rate2rate(AUS_male_rates, from = "central", to = "prob")
kannisto_q <- complete_old_age(AUS_male_qx, ages, old_ages, method = "kannisto",
                               type = "prob", fitted_ages = 80:90)

# Consider males aged 55 in the year 2018
qx_55_2018 <- kannisto_q[as.character(55:130), "2018"]
sim_indiv_path(init_age = 55, sex = "M", death_probs = qx_55_2018)                         
```

---

### Simulate Cohort Life Path

**sim_cohort_path_realised(init_age, sex = "F", death_probs = NULL, closure_age = 130,
cohort = 1000, n_sim = 10000)**

&nbsp;&nbsp; **Parameters:**

&nbsp;&nbsp;&nbsp;&nbsp; init_age : numeric

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *integer denoting initial age of individuals*

&nbsp;&nbsp;&nbsp;&nbsp; sex : character

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *character denoting the gender of individuals, "F" for female and
"M" for male*

&nbsp;&nbsp;&nbsp;&nbsp; death_probs : vector

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *vector of 1-year death probabilities. If not supplied, an M7
age-period-cohort model will be* 

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *fitted on `mortality_AUS_data` to produce forecasted
death probabilities for individuals* 

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *starting at `init_age` in 2022*

&nbsp;&nbsp;&nbsp;&nbsp; closure_age : numeric

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *maximum age*

&nbsp;&nbsp;&nbsp;&nbsp; cohort : numeric

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *integer denoting initial cohort size*

&nbsp;&nbsp;&nbsp;&nbsp; n_sim : numeric

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *integer denoting number of path simulations*

&nbsp;&nbsp; **Returns:**

&nbsp;&nbsp;&nbsp;&nbsp; a matrix where each row represents the number of individuals still alive
from a given cohort 

&nbsp;&nbsp;&nbsp;&nbsp; at each age

An example looks like: 

\begin{pmatrix}
1000 & 996 & 991 & 985 & \ldots & 0 \\
1000 & 998 & 993 & 986 & \ldots & 0 \\
 &  &  \vdots & & & \\
1000 & 997 & 994 & 986 & \ldots & 0 \\
1000 & 997 & 992 & 987 & \ldots & 0
\end{pmatrix}

Note that the first column of the matrix will always be the initial cohort size as everyone is alive at the initial age,
and the last column will always be 0, signifying everyone will be dead at age 131 (if we assume 130 is the
maximum age one can be). 

&nbsp;&nbsp; **Usage:**

```r
# Simulate life paths for cohort of 1000 females starting at age 60
sim_cohort_path_realised(init_age = 60, sex = "F")

# Suppose we want to use period 1-yr death probabilities instead
AUS_male_rates <- mortality_AUS_data$rate$male
ages <- mortality_AUS_data$age # 0:110
old_ages <- 91:130
AUS_male_qx <- rate2rate(AUS_male_rates, from = "central", to = "prob")
kannisto_q <- complete_old_age(AUS_male_qx, ages, old_ages, method = "kannisto",
                               type = "prob", fitted_ages = 80:90)

# Consider 100 males aged 55 in the year 2018
qx_55_2018 <- kannisto_q[as.character(55:130), "2018"]
sim_cohort_path_realised(init_age = 55, sex = "M",
                         death_probs = qx_55_2018, cohort = 100)                         
```

---

### Simulate Expected Cohort Life Path

**sim_cohort_path_expected(init_age, sex = "F", death_probs = NULL, closure_age = 130,
cohort = 1000)**

&nbsp;&nbsp; **Parameters:**

&nbsp;&nbsp;&nbsp;&nbsp; init_age : numeric

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *integer denoting initial age of individuals*

&nbsp;&nbsp;&nbsp;&nbsp; sex : character

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *character denoting the gender of individuals, "F" for female and
"M" for male*

&nbsp;&nbsp;&nbsp;&nbsp; death_probs : vector

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *vector of 1-year death probabilities. If not supplied, an M7
age-period-cohort model will be* 

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *fitted on `mortality_AUS_data` to produce forecasted
death probabilities for individuals* 

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *starting at `init_age` in 2022*

&nbsp;&nbsp;&nbsp;&nbsp; closure_age : numeric

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *maximum age*

&nbsp;&nbsp;&nbsp;&nbsp; cohort : numeric

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *integer denoting initial cohort size*

&nbsp;&nbsp; **Returns:**

&nbsp;&nbsp;&nbsp;&nbsp; vector of expected number of individuals still alive 
from a given cohort at each age

An example looks like: 

\begin{pmatrix}
1000 & 995 & 989 & 981 & \ldots & 0
\end{pmatrix}

Note that the first entry of the matrix will always be the initial cohort size as everyone is alive at the initial age,
and the last entry will always be 0, signifying everyone will be dead at age 131 (if we assume 130 is the
maximum age one can be). 

&nbsp;&nbsp; **Usage:**

```r
# Simulate expected life path for cohort of 1000 females starting at age 60
sim_cohort_path_expected(init_age = 60, sex = "F")

# Suppose we want to use period 1-yr death probabilities instead
AUS_male_rates <- mortality_AUS_data$rate$male
ages <- mortality_AUS_data$age # 0:110
old_ages <- 91:130
AUS_male_qx <- rate2rate(AUS_male_rates, from = "central", to = "prob")
kannisto_q <- complete_old_age(AUS_male_qx, ages, old_ages, method = "kannisto",
                               type = "prob", fitted_ages = 80:90)

# Consider 100 males aged 55 in the year 2018
qx_55_2018 <- kannisto_q[as.character(55:130), "2018"]
sim_cohort_path_expected(init_age = 55, sex = "M",
                         death_probs = qx_55_2018, cohort = 100)
```