[Think Stats Chapter 2 Exercise 4](http://greenteapress.com/thinkstats2/html/thinkstats2003.html#toc24) (Cohen's d)


### Background

Cohen's D gives us the size of a variable's effect on outcomes. A variable's impact is viewed as the difference in means between distinct variable groupings. The difference is measured in units of standard deviation from the mean. 

### Prompt

Use the variable ```totalwgt_lb``` to investigate whether first babies are lighter or heavier than others. Compute Cohen's d to quantify the difference between the groups. Only live births have been kept in this dataset.

How does it compare to the difference in pregnancy length?

### Python Code
This code is ran with numpy, as well as nsfg (National Survey of Family Growth) a package made available by Allen Downey in Think Stats.

```
import numpy as np
import nsfg
```

Within the NSFG data available the live birth subset is of use to us.

```
preg = nsfg.ReadFemPreg()
#getting a subset dataframe
live = preg[preg.outcome == 1]
```
To calculate the Cohen's d, designated groupings must be made. One is of first births & the other is later births. These subsets will still have all the same features inherit to their observations, which will be used later.

```
firsts = live[live.birthord == 1]
others = live[live.birthord != 1]
```

The function used here, from Think Stats, gives us the Cohen's d of two groups. Specifically, it will take the difference in mean divided by a square root of the combined variation of the two groups.

```
def CohenEffectSize(group1, group2):
    """Computes Cohen's effect size for two groups.
    
    group1: Series or DataFrame
    group2: Series or DataFrame
    
    returns: float if the arguments are Series;
             Series if the arguments are DataFrames
    """
    diff = group1.mean() - group2.mean()

    var1 = group1.var()
    var2 = group2.var()
    n1, n2 = len(group1), len(group2)

    pooled_var = (n1 * var1 + n2 * var2) / (n1 + n2)
    d = diff / np.sqrt(pooled_var)
    return d
    
```
The variable we're accessing here is to see whether there's any difference between weights of babies when born first or later.

```
CohenEffectSize(others.totalwgt_lb, firsts.totalwgt_lb)
```
To compare it with pregnancy length we'll be calling the function once more.

```
CohenEffectSize(firsts.prglngth, others.prglngth)
```

#### Results

Cohen's d of birth order on total birth weight was determined to be -0.0889. 
For birth order on pregnancy length this effect was 0.0289

Birth order was seen to have a small, positive influence on pregnancy length and a small--but slightly larger--negative influence on birth weight. Note that the statistical signifiance of these numbers has not yet been validated.

### Explanation

The negative value of Cohen's d, when comparing subsequent birth babies to first birth babies, indicates that initial births' total weight were slightly less than later births. Although not great, this is a slight, notable correlation of born weights and birth order. A smaller signifance is that of birth order on pregnancy lengths. The positive value represents a small likelihood that birth length may be longer for first born babies.
