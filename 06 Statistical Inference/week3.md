## T Confidence Intervals

So far, we created confidence intervals using the CLT; they took the form
estimate +- quantile of N(0,1) * estimate standard error of the estimate.
This doesn't work well for small sample sizes.

We will derive quantiles of the student-t distribution and use those instead.
For large samples, they converge towards the normal quantiles.

---

The student-t distribution comes from the observation that normalizing an 
average of n i.i.d. Gaussians with the *sample* standard error s does not yield 
a normal distribution (using the true standard error would!); this is the 
definition of student-t with n-1 degrees of freedom, it is the distribution of

(mean(X) - mu) / (s / sqrt(n)).

The student-t distribution is centered around zero, looks similar to the normal
distribution but has a thicker tail.

The intervals we use are then

mean(X) +- quantile of t(n-1) * s/sqrt(n).

*Notes:*

 * The t-intervall assumes iid normal data, but it works fairly well whenever
   data are roughly symmetric and mound-shaped.
   
 * Paired observations are often analyzed using the t-interval by taking
    differences, maybe on the log scale.
    
 * Since s converges to the standard deviation, the student-t distribution
    converges to the standard-normal. As a consequence, the confidences intervals
    converge as well. You can therefore always use the t-intervals.
    
 * *Skewed* distributions violate the assumptions. Plus, it doesn't make much
    sense to center skewed distributions at the mean.
    
    Taking logarithms can help, or using a different summary statistic 
    as e.g. the median.
    
 * For highly discrete data, e.g. binary, other intervals are available.
    You can also look at relative risk, risk difference, odd ratio, chi-squared
    tests, normal approximations, and exact tests.
    
    More on that in the regressions class and the Biostat bootcamp 2.
 
 * In R, `qt()` gives the quantiles of the student-t distribution.
 
 * `t.test()` provides ready-made implementations. Options `paired` and `var.equal` 
    tests control which assumptions you assert to hold.
 

## Independent group t-intervals

If the two groups are independent, i.e. no pairs can be formed,
we can use these *independent group intervals*:

mean(Y) - mean(X) +- t(n_x + n_y - 2, 1 - alpha/2) * s_p * (1/n_x + 1/n_y)^1/2

is the (1 - alpha) * 100% confidence interval for muy - mux. Here, s²p is
the *pooled variance*;

s²_p = ( (n_x - 1)s²_x + (n_y - 1)s²_y ) / (n_x + n_y - 2),

which is the weighted average of the variances of the two groups. This assumes
that the population of the two groups have the same variance. If that is not
the case, we "add in" the variances differently:

mean(Y) - mean(X) +- t(df, 1 - alpha/2) * (s²_x/n_x + s²_y/n_y)^1/2

where df is rather complicated. If both X and Y contain i.i.d. normal random
variables, df is already rather complicated. The main point here is that in
the case of unequal variances, the difference of means is *not* t-distributed,
so you *have* to use the unequal-variance interval.

*Note:* Always pair if you can, it reduces variance resp. shrinks the confidence
intervals!


## Hypothesis testing

We have the *null hypothesis* H_0 (status quo, assumed true) and seek for
statistical evidence to reject it in favor of the *research/alternative hypothesis* H_a.

There are two types of errors that can happen: we reject H_0 even though it is
true (type 1 error), or we do not reject it event though it is false (type 2 error).

What is our standard for rejecting H_0? If low, the rate of type 1 error increases
but the rate of type 2 error decreases; if high, the other way around.
Note that more/better data can lower both simultatneously!

We call the probability of a type 1 error alpha (in English *significance*), and
the probability of a type 2 error beta (1-beta is called *power*). Our target
significance drives the test-cut-off while our target power drives the sample size.


### One-sided testing

We typically mandate the probability of type 1 errors alpha to be small,
say 0.05. Say we want to reject H_0 if mean(X) > C -- how do we choose C?  
(Using > here; < works as well.)

Depending on the model we assume as H_0 (e.g. N(µ, s)), we can set up
P(mean(X) > C | H_0) <= alpha and solve for C.

We can also use the Z-score = (mean(X) - µ_0) / standard error as test statistic, 
which is standard normal under normal H_0.

*Note:* For large sample sizes, we often employ the CLT and use normal hypotheses 
and therewith normal quantiles. For smaller sample sizes, we may have to fall back
to the t-distribution; we can still use the Z-score but with t-quantiles.

### Two-sided testing

Two-sided tests work similarly, but we have to split alpha in half for upper
and lower tail. Note that an alpha-two-sided hypothesis test is exactly the
same as checking the (1-alpha)-confidence interval.

### Two groups t-test

Testing H_0 : µ_1 = µ_2 follows from the above, specifically from group t-intervals:
transform to H_0 : µ_1 - µ_2 = 0 and apply the same mathematics.

### In R

Functions `_.test`, e.g. `t.test()` perform these tests for us.


## P-values

Ubiquituous, but often misinterpreted. Take care!

Assume we are interested in summary statistic f(X).

 1. Determine the distribution F of f(X) under H_0.
 2. Calculate the test statistic f(data). 
 3. The p-value is the probability of values >= f(data) under F.  
    (For the one-sided, great-than variant; others are similar).
    
All hypothesis tests with alpha >= p-value would reject the hypothesis.
Hence, the p-value is also called *attained significance level*.

*Note:* Software often assumes a two-sided test, so check before reporting.
