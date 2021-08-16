# truncated-mvn-sampler

Efficient sampling from the truncated multivariate Normal distribution.

## Overview
Reimplementation using **Python** of the minimax tilting algorithm by [Botev (2016)](https://arxiv.org/pdf/1603.04166.pdf) for simulation and iid sampling of the truncated multivariate Normal distribution. The **original MATLAB implementation** by the author can be found [here](https://de.mathworks.com/matlabcentral/fileexchange/53792-truncated-multivariate-normal-generator).

The main features of this algorithm are:

- simulation from multivariate truncated Normal distribution using an accept-reject algorithm based on minimax exponential tilting
- (quasi) Monte-Carlo estimation of the distribution function using separation-of-variables together with exponential tilting for provable performances and theoretical upper bound on the error
- Cholesky decomposition using the reordering algorithm of Gibson, Glasbey and Elston (1994).

Feel free to use this code, but don't forget to cite [Botev (2016)](https://arxiv.org/pdf/1603.04166.pdf)!

## Main difference to the orignal MATLAB implementation
The Python implementation provided here uses a [modification of the Powell hybrid method implemented in SciPy (`method='hybr'`)](https://docs.scipy.org/doc/scipy/reference/generated/scipy.optimize.root.html#r9d4d7396324b-1) for finding the roots compared to the [Trust-Region-Dogleg Algorithm](https://de.mathworks.com/help/optim/ug/equation-solving-algorithms.html#f51887) used in the MATLAB implementation.


## Dependancies
- [numpy](https://numpy.org)
- [SciPy](https://docs.scipy.org/doc/scipy/index.html)

# Example
```python
d = 10  # dimensions

# random mu and cov
mu = np.random.rand(d)
cov = 0.5 - np.random.rand(d ** 2).reshape((d, d))
cov = np.triu(cov)
cov += cov.T - np.diag(cov.diagonal())
cov = np.dot(cov, cov)

# constraints
lb = np.zeros_like(mu) - 1
ub = np.ones_like(mu) * np.inf

# create truncated normal and sample from it
n_samples = 100000
tmvn = TruncatedMVN(mu, cov, lb, ub)
samples = tmvn.sample(n_samples)
```

Ploting the results of the first dimension and comparing it to the nontruncated normal distribution results in:

![tmvn_plot](https://user-images.githubusercontent.com/49341051/129542882-c83431dc-f47e-4a8d-bef8-f236e471c9f1.png)

*Disregard the scaling, as the normal and truncated normal are plotted on a different y-axis.*

# Reference
The implementation is based on the [MATLAB implemenation by author](https://de.mathworks.com/matlabcentral/fileexchange/53792-truncated-multivariate-normal-generator). An R implemetation by the author can be found [here](https://github.com/lbelzile/TruncatedNormal) or installed from **CRAN**  via

```R
install.packages("TruncatedNormal")
``` 

or from Github via

```R
devtools::install_github("lbelzile/TruncatedNormal")
```

Botev, Z. I., (2016), The normal law under linear restrictions: simulation and estimation via minimax tilting,
Journal of the Royal Statistical Society Series B, 79, issue 1, p. 125-148
