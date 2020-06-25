[Think Stats Chapter 2 Exercise 4](http://greenteapress.com/thinkstats2/html/thinkstats2003.html#toc24) (Cohen's d)


### Background

Cohen's D gives us the size of a variable's effect on outcomes. The difference in means between data, grouped by a variable, is represented in units of standard deviation. This deviation in means can be viewed as the impact the variable has had on the spreading of results.

### Prompt

Use the variable ```totalwgt_lb``` to investigate whether first babies are lighter or heavier than others. Compute Cohen's d to quantify the difference between the groups. Only live births have been kept in this dataset.

How does it compare to the difference in pregnancy length?

### Python Code
To calculate the Cohen's d
First we'll designate different groupings of live birth data based off of the order of birth.
```
firsts = live[live.birthord == 1]
others = live[live.birthord != 1]
```

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

```
CohenEffectSize(others.totalwgt_lb, firsts.totalwgt_lb)
```

#### Results

### Explanation
