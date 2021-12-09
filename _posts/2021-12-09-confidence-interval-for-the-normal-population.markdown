---
layout: post
title: "The confidence interval: an explainer for the normal population"
date: 2021-12-08
categories: for-the-normal-population statistics ab-testing
---

# The confidence interval: an explainer for the normal population

After you run an experiment and measure a difference between your groups, what you get is a “point estimate” - one single value. It’s not necessarily the “true” difference between the groups because this is just one sample and we know that, due to randomness, any 2 groups can differ, even if we don’t do anything different to either one. We only have one realisation of multiple potential experiments we could run. The confidence interval is useful because it gives us a range in which we believe that our true value is likely to lie if we had to repeat the experiment many times.

If you construct a 90% confidence interval, it means that there’s a 90% chance that your particular experiment happens to be one in which you created a confidence interval that contains the true value. We say we’re “90% confident” that the true value lies within the interval.

There’s a nuanced point here. If you run the experiment many times and each time you construct a 90% confidence interval, 90% of the time the interval will contain the true parameter. This is illustrated in the image below:
- The orange dot represents the “true” value. 
- The blue and red dots are point estimates. 
- The horizontal lines are confidence intervals.
  - The blue confidence intervals contain the true value - this happens 9 out of 10 times (or 90% of the time)
  - The red confidence interval doesn’t contain the true value - this happens 10% of the time

![p-values]({{ BASE_PATH }}/assets/images/2021-12-09-confidence-interval-for-the-normal-population/confidence_interval.png)

This is what we mean when we say that 90% of the time, the true value will lie within our 90% confidence interval in repeated sampling. But in any one particular realisation of your experiment, you can’t say that there’s a 90% chance that the true value lies within your confidence interval. 

To illustrate this, imagine you had a confidence interval of (10, 100) and the true value was 20. It doesn’t make sense to say that 95% of the time 20 will lie in the range (10, 100). It clearly does lie within the range. The fact that in real life you don’t know what the true value is doesn’t change this. 

So the best we can say is that we’re probably in a world in which we’ve constructed a confidence interval that contains the true parameters - this happens 90% of the time after all. In reality, people often want more confidence than 90%, with 95% being a standard minimum threshold in many cases.

## Why report the confidence interval?

It’s good practice to give the confidence interval for at least 2 reasons:
We don’t know what the “true” value is so just reporting the point estimate is misleading. Although we don’t know if our realised confidence interval contains the true value, it likely does. So it gives a lower and upper bound to our estimate, which is a more truthful representation of what we believe the true uplift actually is.
The range itself gives us an idea of the certainty of our conclusions. Imagine you’re trying to estimate whether a particular change in your product increases conversion rates from an average of 2%. If your confidence interval of the increase were narrow, say (0.1%, 0.2%), that would mean you have a precise estimate of what the uplift is estimated to be. A wide confidence interval of (0.1%, 5%) would mean that you’re less sure about the impact - it’s likely to be as low as 0.1% or as high as 5%.

Couching your point estimates within a confidence interval is a useful way to convey the uncertainty of an estimate while representing the likely magnitude of your experiment.

