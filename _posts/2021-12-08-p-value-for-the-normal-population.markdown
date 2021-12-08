---
layout: post
title: "The p-value: an explainer for the normal population"
date: 2021-12-08
categories: for-the-normal-population statistics ab-testing
---

# The p-value: an explainer for the normal population

You’ve run an experiment and measured some difference between your groups. That’s dandy but how confident can you be that this difference reveals some change that you made as opposed to blind chance? This is where the p-value comes in. It tells you how likely you are to get a value as, or more, extreme than the one you measure, if there really is no difference. If it’s super unlikely to measure such a change under the assumption that there’s no change then we can be more confident that this assumption is false - that there really is a difference.

If the p-value is small, say 0.01, meaning that you’d only expect to see a result at least that extreme 1% of the time, it’s unlikely that the difference is a result of chance - even though it’s possible. If the p-value is large, let’s say 30%, then it’s quite likely that the result was only due to chance, since almost one third of the time you’d get a result at least that extreme.

So the smaller the p-value, the more confident you can be that the difference you’re observing is due to the change you made in your experiment. It therefore provides a measure of risk associated with taking action on the assumption that your experiment had an effect. The risk here being that you’re wrong. The lower the p-value, the less risk there is of you assuming that there is a real difference when there really isn’t one.

## An example

Let’s say you’re interested in seeing whether caffeine affects sleep quality - as measured by the number of hours slept. So you run an experiment with 200 people - you randomly assign half (group A) regular coffee and the other half (group B) decaffeinated coffee that’s otherwise identical to the regular version. And you find that group B sleeps 7 minutes more than group A. Nice! But wait a minute, how confident can you be that this difference was caused by the caffeine? 

If you take any 2 random groups of 100 people, and don’t do anything different to either one, you’ll probably measure some difference in just about anything you want to measure - because of randomness. As an extreme example, if you took any 2 random people and measured their difference in weight, you’re very likely to get a difference. The more people you put in either group, the less the difference is likely to be but it’s unlikely to ever go away.

Let’s go back to the caffeine example. Imagine that instead of a 7 minute difference in time slept, you’d found a difference of 20 minutes. After which experiment would you be more confident to say that caffeine affects sleep quality? Intuitively, the second one right? Because it’s less likely that you’d get a difference of 20 due to chance compared to just 7. 

The p-value gives us a way to quantify how likely the results we got - 7 and 20 in our example - were just due to chance. How do we do this?

## Getting the distribution of differences due to chance

Theoretically, you could actually take 2 random groups of 100 people each and measure the difference it takes for each to fall asleep. In the product experimentation world, this sort of test is known as an  “A/A test” (because each group is exactly the same). Repeating this exercise many times, you’d get a distribution of differences only due to chance.

We can simulate what this would look like. According to this paper, adults aged 26-40 from the Netherlands, UK, and USA, sleep on average 8 hours with a standard deviation of 0.9. Converting this to minutes, we can simulate the process of randomly selecting 100 people into 2 groups. We can repeating the process 100,000 times, recording the mean each time:

```python
groups = ['a', 'b']
n = int(10e4)

means = {}
samples = {}
for g in groups:
    samples[g] = []
    means[g] = []
for s in range(0, n):
    for g in groups:
        new_sample = np.random.normal(8*60, 0.9*60, 100)
        samples[g].append(new_sample)
        means[g].append(new_sample.mean())
```

This allows us to find the differences between the means from each sample and construct the following histogram:

```python
diffs = pd.DataFrame(np.subtract(means[groups[0]], means[groups[1]]))
fig, ax = plt.subplots(figsize=(15,10))
sns.histplot(diffs, kde=False, ax=ax, legend=False)
```
![simulated_frequencies]({{ BASE_PATH }}/assets/images/2021-12-08-p-value-for-the-normal-population/simulated_frequencies.png)


Using this, we can tell what proportion of the time we got a result at least as extreme as the ones from our example assuming that there’s no real difference between the groups. This is also the distribution of the difference under the null hypothesis.

In the real world it’s often not feasible to do this exercise, and nor is it necessary, because we actually know what the distribution of the differences due to chance would be theoretically, by the magic of the Central Limit Theorem (which I won’t go into right now).

## Enter the real world

We know that a sample statistic tends to be normally distributed. If we assume that there’s no difference between our groups, all we need is to know the standard deviation of the difference (which we now call the “standard error”). 

Without getting too technical, knowing the standard deviation from our one realised experiment, there are some neat formulas we can plug in to get the standard error of the distribution of differences that we would get due to chance. We can use the following code to pick out at random one of the samples from each group and theoretical, inferred, distribution, based on the formulas, and superimpose this ontop of what we got from our empirical, simulated, distribution from above :

```python
fix, ax = plt.subplots(figsize=(15,10))

sample_a = samples['a'][np.random.randint(0, len(samples['a']))]
mean_a = sample_a.mean()
var_a = sample_a.var()
size_a = len(sample_a)

sample_b = samples['b'][np.random.randint(0, len(samples['b']))]   
mean_b = sample_b.mean()
var_b = sample_b.var()
size_b = len(sample_b)

null_mean = 0
null_se = math.sqrt(var_b/size_b + var_a/size_b)

# Create our x-axis: 100 evenly-spaced values between extreme lower and upper values of the distribution
x = np.linspace(
        stats.norm.ppf(0.00001, null_mean, null_se),
        stats.norm.ppf(0.99999, null_mean, null_se), 
        100
    )
# Transform the values on our x-axis to values in the our normal distribution
y = stats.norm.pdf(x, null_mean, null_se)

# Plot what we just inferred
sns.lineplot(x=x, y=y, ax=ax, label='Inferred distribution')

# Plot the values above (from the simulation) but convert frequencies to density
sns.histplot(diffs, kde=False, stat='density', ax=ax, label='Empirical distribution')
ax.legend()
```
![inferred_vs_simluated_densities]({{ BASE_PATH }}/assets/images/2021-12-08-p-value-for-the-normal-population/inferred_vs_simluated_densities.png)


(I converted the frequencies from the simulated distribution into densities so that the area under the graph equals 1, which allows us to compare it to the theoretical distribution, which has the same property).

I find it amazing that the results from only one experiment can give us the same distribution of the null hypothesis that you’d get from running a simulation 100,000 times! Such is the power of statistics.

## How to practically calculate the p-value from the distribution under the null

So back to our example, how likely are we to get a difference of 7 as extreme or more? You can get that by finding the area under the curves above 7 and below -7 (assuming we’re running a 2-tailed test because a difference of 7 could be due to group A having 7 minutes more or less than B). We can find that by plugging into the CDF. Let’s do the same for 20 minutes:

```python
pval_7 = (1-stats.norm.cdf(7, null_mean, null_se))*2
print(f'p-value of a 7 minute difference: {pval_7:.2%}')

pval_20 = (1-stats.norm.cdf(20, null_mean, null_se))*2
print(f'p-value of a 20 minute difference: {pval_20:.2%}')
```
![p-values]({{ BASE_PATH }}/assets/images/2021-12-08-p-value-for-the-normal-population/p-values.png)


This tells us that about 33% of the time we’d get a difference of 7 or more but only 0.6% of the time we’d get a difference of 20 or more. So our intuition above was indeed correct.

## How big is “enough”?

There’s ultimately no “correct” p-value but commonly thresholds of 5% or 1% are used. These correspond to confidence levels of 95% and 99% respectively.

## Summary

The p-value is one important measure used to understand how confident we can be that the differences in our experiment groups are due to chance or some change that we enacted.
