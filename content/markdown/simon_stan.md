---
title: Bayesian methods for Neighborhood Price Index
author: Simon Rimmele
date: 2018-06-28
category: stats
tags: stan, stats, pi
summary: Quick introduction to Bayesian methods and Stan programming language, which StreetEasy uses to estimate Price Index on neighborhood levels
output:
  revealjs::revealjs_presentation:
    theme: simple
    highlight: haddock
    center: TRUE
    incremental: TRUE
---
<style type="text/css">
  .reveal p {
    text-align: left;
  }
  .reveal ul {
    display: block;
  }
  .reveal ol {
    display: block;
  }  
</style>
### Obligatory Bayes Theorem 

$$P(\theta|x) = \frac{P(x|\theta)P(\theta)}{P(x)}$$

- $x$ is your observed data.  
- $\theta$ are your model parameters.

### Ignore the denominator
$$P(\theta|x) \propto {P(x|\theta)P(\theta)}$$
* $P(\theta|x)$: the posterior distribution (what we want)  
* $P(x|\theta)$: the likelihood (what frequentists see)  
* $P(\theta)$: the prior/s   

### Why Stan?
Calculating the posterior is difficult because it involves taking an integral over the right side of the equation; that is often intractable. Stan does it for you. 

### How does Stan do it?

[Check out this demo](https://chi-feng.github.io/mcmc-demo/app.html#EfficientNUTS,banana)

### Why not just do optimization ( SGD / Adam / your favorite algorithm? )

MCMC sampling estimates a distribution, which gives you access to **uncertainty**. Quantifying uncertainty is the goal of Bayesian statistics. 


### How do I do Bayesian statistics?
1. As an R User? You're in luck:   
    * `rstanarm`: package with a ton of pre-compiled model types, no need to write `Stan` code.  
    * `brms`: package with more esoteric models and flexibility, also no need to write `Stan`.  
    * `rstan`: R interface for `Stan`, if you want to write your own code. 
    * `shinystan`: Visual diagnostics for Stan, very practical for checking models. 
2. In Python? Not as easy right now:  
    * `pystan`: Python interface for `Stan`, you'll need to provide your own `Stan` code.  
    * `pymc3`: More pythonic probabalistic programming package, albeit without access to the excellent `Stan` algorithm. It can do a few things `Stan` cannot, like discrete mixture models. 

### Stan
##### Stan Syntax for Linear Regression 

$$ y = \alpha + \beta x $$
$$\alpha \sim N(0,10) $$
$$\beta \sim N(0,10) $$
$$\sigma \sim Cauchy(0,5) $$

```{}
data {
  int<lower=0> N;
  vector[N] y;
  vector[N] x;
}
parameters {
  real alpha;
  real beta;
  real<lower=0> sigma;
}
model {
  alpha ~ normal(0,10);    
  beta ~ normal(0,10);
  sigma ~ cauchy(0,5);
  for (n in 1:N)
    y[n] ~ normal(alpha + beta * x[n], sigma);
}
```
## How do I pick priors?  
* Remember: _Informative_ != _Subjective_

#### 1. Uninformative Priors
* `Uniform`
* `Jefferys` (don't bother)  

#### 2. Weakly Informative
* `Normal (0,[a large-ish number])`
* `Student-T (0,[a large-ish number], [fat tails?])`
* `LaPlace(0,[some number])`, this is locally equivalent to the L1 penalty (LASSO)
* Constrain a domain to possible values, variance is positive etc...

#### 3. Strongly Informative
* Anything where you are able to provide additional info.


## More Resources
* [Stan website](http://mc-stan.org/)
* [Stan: Guide to Priors](https://github.com/stan-dev/stan/wiki/Prior-Choice-Recommendations)
* [hierarchical models in pymc3](http://twiecki.github.io/blog/2017/02/08/bayesian-hierchical-non-centered/)