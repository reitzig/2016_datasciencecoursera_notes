## Introduction

Statistical inferences is the process of generating conclusions about a 
population from a noisy sample.
We try to create knowledge beyond the strict boundaries of the data at hand.

We will focus on the frequentist way of thinking about probability.

We want to be able to answer questions for *causalities* not only association.

## Probability

Probabilities are *population* properties. We will use data on *samples* to infer 
knowledge (using *estimators*) on these probabilities (*estimands*).

Since much of this is prior knowledge for me, I only list what is taught here 
without a lot of summary.

 * Axioms of probability measures.
 * Discrete and continuous random variables.
 * Densities and mass functions.
 * Cumulative distribution function.
 * Quantiles of distributions.
 * Conditional probability.
 * Bayes' rule.
 * Independence.
 * Expected values.
 * Bias.
 * Variance.
 * Standard error.
 
    Var(smean(X)) = sigma^2/n  
    Std(smean(X)) = sigma/sqrt(n) <- Standard error
 * Common distributions.
 * Law of large numbers.
 * Central limit theorem.
 
*Note:* In R, cumulative distribution functions are prefixed by `p-`m so e.g.
`pbeta()` for the beta distribution. Quantile functions are prefixed by `q-`
in a similar way. `_.test` methods, e.g. `binom.test()`, give you conservative
confidence intervals.
 
### Facts on the normal distribution

 1. About 68%/95%/99% lie within 1/2/3 standard deviations.
 2. -1.28/-1.645/-1.96/-2.33 are the 10/5/2.5/1 percentile of N(0,1).
 3. 1.28/1.645/1.96/2.33 are the 90/95/97.5/99 percentile of N(0,1).
 
Adjust 2./3. to N(µ,s) by x' = x*s +- µ.

### Approximation of confidence intervals using CLT 

(Wald intervals: smean +- qnorm(...)*stderror).

Say smean(X) is approximately normal with mean µ and std sigma/sqrt(n).
Then, smean(X) +- 2sigma/sqrt(n) is approximately a 95% interval for µ.

Hack for the Binomial case if n is not large enough: Agresti/Coull interval 
(add two successes and two failures).
