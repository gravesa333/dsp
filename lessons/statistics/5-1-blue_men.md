[Think Stats Chapter 5 Exercise 1](http://greenteapress.com/thinkstats2/html/thinkstats2006.html#toc50) (blue men)

### Chapter 5 Exercise 1

*In the BRFSS (see Section 5.4), the distribution of heights is roughly normal with parameters μ = 178 cm and σ = 7.7 cm for men, and μ = 163 cm and σ = 7.3 cm for women.*

*In order to join Blue Man Group, you have to be male between 5’10” and 6’1” (see http://bluemancasting.com). What percentage of the U.S. male population is in this range? Hint: use scipy.stats.norm.cdf.*

---


First, import the necessary module.

```
import scipy.stats
```

Create the variables for mean, standard deviation, and the normal distribution.

```
mu = 178 #cm
sigma = 7.7 #cm
dist = scipy.stats.norm(loc=mu, scale=sigma)
```

Looking at the height range of 5'10" to 6'1", convert to cm to match the units of the mean and standard deviation.

```
minimum_height = 70*2.54 #cm
maximum_height = 73*2.54 #cm
```

The percentage of the US male population within this range will be determined using the CDF.

```
percent_male = dist.cdf(maximum_height) - dist.cdf(minimum_height)
```

From this calculation, it is determined that **34.3%** of the US male population is between 5'10" and 6'1".