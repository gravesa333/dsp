[Think Stats Chapter 3 Exercise 1](http://greenteapress.com/thinkstats2/html/thinkstats2004.html#toc31) (actual vs. biased)

### Chapter 3 Exercise 1
*Something like the class size paradox appears if you survey children and ask how many children are in their family. Families with many children are more likely to appear in your sample, and families with no children have no chance to be in the sample.*

*Use the NSFG respondent variable NUMKDHH to construct the actual distribution for the number of children under 18 in the household.*

*Now compute the biased distribution we would see if we surveyed the children and asked them how many children under 18 (including themselves) are in their household.*

*Plot the actual and biased distributions, and compute their means. As a starting place, you can use chap03ex.ipynb.*

---

First, import the dataframe from nsfg.py and the necessary modules. 

```
import nsfg
import matplotlib.pyplot as plt    #to plot histogram
import pandas as pd

df_resp = nsfg.ReadFemResp()    #create dataframe from data
```

Next, get the necessary bins and probabilities for children younger than 18 in the household.

```
child_num_series = df_resp['numkdhh']    #survey data

child_num_count = child_num_series.value_counts()    #counts occurrence of values in series
child_num_prob = child_num_series.value_counts(6)    #turns data into probabilities based on 6 bins
```

Using this data, create a bar graph displaying the probability mass function.

```
plt.bar(list(range(6)), height = child_num_prob, color="blue", alpha=0.5)
plt.xlabel('# of Child <18 in the Household', fontsize = 10)
plt.ylabel('Probability', fontsize = 10);
plt.title('PMF for # Children <18 in the Household',fontsize = 15);
plt.bar
```

![Actual PMF](https://github.com/gravesa333/dsp/blob/master/lessons/statistics/actual_pmf.png)

Finally, compute the mean for this data, i.e. the average number of children younger than 18 in the household.

```
child_num_prob_dict = dict(child_num_prob)

mean = 0
for x, p in child_num_prob_dict.items():    #iterate through pmf dictionary
    mean += x*p   #mean = sum[(# of children in household)*(probability)]
```

**The average number of children younger than 18 in a household (based on the actual distribution) is 1.02.**

---

Moving on to the biased distribution, create a dataframe of the bins and respective probabilities.

```
child_num_df = pd.DataFrame(child_num_count)    #add first row (data per bin)
child_num_df['child_num_pmf'] = child_num_prob    #add second row (probabilities)
```

| Index | numkdhh | child_num_pmf |
| --- | ------- | ------------- |
| 0 | 3563 | 0.466178 |
| 1 | 1636 | 0.214052 |
| 2 | 1500 | 0.196258 |
| 3 | 666 | 0.087139 |
| 4 | 196 | 0.025644 |
| 5 | 82 | 0.010729 |

Iterate through the dataframe and multiply the probabilities by the number of children who are in the respective bins.  Then normalize the data to create the biased distribution.

```
child_num_new_count = {}
child_num_new_prob = {}
child_num_new_sum = 0

for index, row in child_num_df.iterrows():    #iterate through dataframe

    #multiply the probabilities by the number of children who are in the respective bins
    
    child_num_new_count.update({index: index*row['numkdhh']*row['child_num_pmf']}) 
    child_num_new_sum += index*row['numkdhh']*row['child_num_pmf']

for index, row in child_num_new_count.items():
    new_row = row/child_num_new_sum    #normalize child_num_new_count to create biased pmf
    child_num_new_prob.update({index: new_row})
```

Using this data, plot both the actual and biased distributions on the same graph to compare them.

```
plt.bar(list(range(6)), height = child_num_prob, color="blue", alpha=0.5)
plt.bar(list(range(6)), height = child_num_new_prob.values(), color="red", alpha=0.5)
plt.xlabel('# of Child <18 in the Household', fontsize = 10)
plt.ylabel('Probability', fontsize = 10);
plt.title('PMF for # Children <18 in the Household',fontsize = 15);
plt.legend(['Actual Distribution','Biased Distribution'])
plt.bar
```

![Actual vs Biased PMF](https://github.com/gravesa333/dsp/blob/master/lessons/statistics/actual_vs_biased_pmf_fixed.png)

Finally, compute the mean for the biased distribution (same approach as for the actual distribution.

```
mean = 0
for x, p in child_num_new_prob.items():
    mean += x*p
```

**The average number of children younger than 18 in a household (based on our biased distribution) is 1.89.  This is greater than the average calculated from the actual distribution (1.02).**

