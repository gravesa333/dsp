[Think Stats Chapter 2 Exercise 4](http://greenteapress.com/thinkstats2/html/thinkstats2003.html#toc24) (Cohen's d)

### Chapter 2 Exercise 4
*Using the variable **totalwgt_lb**, investigate whether first babies are lighter or heavier than others. Compute Cohen's **d** to quantify the difference between the groups. How does it compare to the difference in pregnancy length?*

First, we import the dataframe from nsfg.py. 

```
import nsfg
import matplotlib.pyplot as plt    #to plot histogram

df = nsfg.ReadFemPreg()    #create dataframe from data
```


The data has been cleaned in a number of ways, and the **totalwgt_lb** variable has been created. The relevant code is below.

```
# birthwgt_lb contains at least one bogus value (51 lbs)
# replace with NaN

df.loc[df.birthwgt_lb > 20, 'birthwgt_lb'] = np.nan
    
# replace 'not ascertained', 'refused', 'don't know' with NaN (represented as 97,98,99 respectively)

na_vals = [97, 98, 99]
df.birthwgt_lb.replace(na_vals, np.nan, inplace=True)
df.birthwgt_oz.replace(na_vals, np.nan, inplace=True)

# birthweight is stored in two columns, lbs and oz; convert to a single column in lb

df['totalwgt_lb'] = df.birthwgt_lb + df.birthwgt_oz / 16.0 
```

Next, we'll create a histogram for the weight of first babies compared to other babies:

```
live = df[df.outcome == 1]    #applied to dataframe so that we only look at live births
firsts = live[live.birthord == 1]
others = live[live.birthord != 1] #applied to dataframe so we can differentiate between first and other births

firsts_weight = firsts.totalwgt_lb    #series of total weight (lbs) of first babies
others_weight = others.totalwgt_lb    #series of total weight (lbs) of other babies
firsts_preg_length = firsts.prglngth    #series of pregnancy length (weeks) of first babies
others_preg_length = others.prglngth    #series of pregnancy length (weeks) of other babies

#plot histogram:

plt.figure(figsize = [18,8])
plt.hist(others_weight, bins = 16, rwidth = 0.35, color = 'blue')
plt.hist(firsts_weight, bins = 16, rwidth = 0.35, color = 'red')
plt.xticks(range(0,15,1), fontsize = 20)
plt.yticks(fontsize = 20)
plt.xlabel('Total Birth Weight (lbs + oz)', fontsize = 30)
plt.ylabel('Frequency', fontsize = 30)
plt.title('Histogram of Birth Weight',fontsize = 40)
plt.legend(['Firsts','Others'], fontsize = 20)
plt.tight_layout()
plt.show()
```

![Histogram of Birth Weight](https://github.com/gravesa333/dsp/blob/master/lessons/statistics/Histogram_of_birth_weight.png)

To calculate **Cohen's D**, we need to divide the difference of the means of the two weight groups (first babies and others) by the pooled_variance.  See below:

```
difference_weight = firsts_weight.mean() - others_weight.mean()
variance_1_weight, variance_2_weight = firsts_weight.var(), others_weight.var()
n1_weight, n2_weight = len(firsts_weight), len(others_weight)
pooled_variance_weight = (n1_weight*variance_1_weight + n2_weight*variance_2_weight)/(n1_weight+n2_weight)
d_weight = difference_weight/math.sqrt(pooled_variance_weight)
```
The difference in means is calculated to be **-0.089** standard deviations (first babies are lighter than other babies), which is insignificant.

Moving on to pregnancy length, we'll create a histogram of that data as well:

```
plt.figure(figsize = [18,8])
plt.hist(others_preg_length, bins = 20, range=[27, 46], rwidth = 0.45, color = 'blue')
plt.hist(firsts_preg_length, bins = 20, range=[27, 46], align = 'left', rwidth = 0.45, color = 'red')
plt.xticks(range(27,47,1), fontsize = 20)
plt.yticks(fontsize = 20)
plt.xlabel('Pregnancy Length (weeks)', fontsize = 30)
plt.ylabel('Frequency', fontsize = 30)
plt.title('Histogram of Pregnancy Length',fontsize = 40)
plt.legend(['Firsts','Others'], fontsize = 20)
plt.tight_layout()
plt.show()
```

![Histogram of Pregnancy_Length](https://github.com/gravesa333/dsp/blob/master/lessons/statistics/Histogram_of_pregnancy_length.png)

To calculate **Cohen's D**, we need to divide the difference of the means of the two pregnancy length groups (first babies and others) by the pooled_variance.  See below:

```
difference_preg_length = firsts_preg_length.mean() - others_preg_length.mean()
variance_1_preg_length, variance_2_preg_length = firsts_preg_length.var(), others_preg_length.var()
n1_preg_length, n2_preg_length = len(firsts_preg_length), len(others_preg_length)
pooled_variance_preg_length = (n1_preg_length*variance_1_preg_length + n2_preg_length*variance_2_preg_length)/(n1_preg_length+n2_preg_length)
d_preg_length = difference_preg_length/math.sqrt(pooled_variance_preg_length)
d_preg_length
```
The difference in means is calculated to be **0.029** standard deviations (first babies' pregnancy lengths are longer than other babies' pregnancy lengths), which is insignificant.

Although the two Cohen's d values do not reflect a large effect size, it can generally be concluded that first babies are lighter and have longer pregnancy lengths











