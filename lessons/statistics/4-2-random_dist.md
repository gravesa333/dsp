[Think Stats Chapter 4 Exercise 2](http://greenteapress.com/thinkstats2/html/thinkstats2005.html#toc41) (a random distribution)

### Chapter 4 Exercise 2
*The numbers generated by random.random are supposed to be uniform between 0 and 1; that is, every value in the range should have the same probability.
Generate 1000 numbers from random.random and plot their PMF and CDF. Is the distribution uniform?*

---

First, import the necessary modules. 

```
import random
import matplotlib.pyplot as plt
import pandas as pd
```

Next, generate 1000 random numbers.

```
series_1000 = pd.Series([])

#for each number in the range 0 to 999, generate a random number between 0 and 1 and add it to a series.

for i in range(1000):
    series_1000[i+1] = random.random()
```

Plot the PMF:

```
series_1000_pmf = series_1000.value_counts(1000)    #turns data into probabilities based on 1000 values
plt.figure(figsize = [15,7])
plt.bar(list(range(1000)), height = series_1000_pmf, color="blue", alpha=0.5)
plt.xlabel('Random Integers', fontsize = 15)
plt.ylabel('Probability', fontsize = 15);
plt.title('PMF for 1000 Random Integers between 0 and 1',fontsize = 20);
plt.bar
```

![Random Integers PMF](https://github.com/gravesa333/dsp/blob/master/lessons/statistics/random_integers_bar_pmf_0-1.png)

Plot the CDF:

```
plt.figure(figsize = [15,7])
plt.hist(series_1000, bins = 1000, density=True, cumulative=True,
         histtype='step', color='blue')
plt.xlabel('Random Integers', fontsize = 15)
plt.ylabel('Probability', fontsize = 15);
plt.title('CDF for 1000 Random Integers between 0 and 1',fontsize = 20)
plt.show()
```

![Random Integers CDF](https://github.com/gravesa333/dsp/blob/master/lessons/statistics/random_integers_cdf_0-1.png)


Both the PMF and CDF are approximately straight lines, which confirms that the distribution generated by random.random is uniform.
