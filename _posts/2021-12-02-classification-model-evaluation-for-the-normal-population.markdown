---
layout: post
title:  "Classification model evaluation: an explainer for the normal population"
date:   2021-12-02
categories: for-the-normal-population statistics machine-learning
---

# Classification model evaluation: an explainer for the normal population

Even if you don’t build ML models from scratch, you may want to be able to evaluate them. Whether as an analyst using the output of models, decision-makers using them to inform their decisions, or stakeholders deciding to put them in production, having at least a basic understanding of how to evaluate ML models can be very useful - for at least 3 reasons:

1. Determining how well it performs for a particular purpose: are you comfortable enough implementing it or using it to make a decision or inform your analysis?
2. Deciding between different models: when presented with different options, which one do you choose?
3. (For classification models) deciding how to tweak the model’s sensitivity to signals that a case belongs to a particular class: what classification threshold do you use? This last one requires some further explanation, which I explain below under the heading *tradeoffs.*

I present here a high-level explainer of how to evaluate classification models. The purpose is to give you enough information to speak the same language as the ML model-builder so that you can understand how a model performs, without smothering you in technicalities. I will cover:

- Accuracy,
- Recall/sensitivity, precision, and specificity
- AUC
- F-score

As an example, let’s consider a [machine learning classification model for detecting Covid](https://www.nature.com/articles/s41598-021-95957-w). By the end of this article, we will understand what the performance metrics that they stated mean. The classification labels are:

- Positive: the patient is predicted to have Covid
- Negative: the patient is predicted to not have Covid

Keep in mind that for each of these 2 possible predictions, the model can be correct or incorrect (given whether the patient actually has Covid or not), which gives us 4 different possible outcomes for each patient.

In each case, the measure lies between 0 and 100%, with numbers closer to 100% being better, everything else equal.

### Accuracy

People often use the term “accuracy” colloquially to summarise a model’s performance. In the analytics world, this term has a precise meaning. It tells you what proportion of results the model correctly predicted overall. For example, an accuracy of 95% means that for every 100 predictions, the model got 95 right.

Accuracy can be a good average measure of model performance when your data is *balanced,* meaning that you have about the same number of one class as you do another. In cases where you have *imbalanced* data, it’s incredibly easy to get high accuracy even when a model performs poorly.

For example, in the Netherlands, where I live, [on 29 June about 3% of government-official tests resulted in a Covid-postive result](https://coronadashboard.government.nl/landelijk/positief-geteste-mensen). If the model *always* predicted that patients did not have Covid, ignoring all data, it would have an accuracy of 97% - because 97% of people don’t have Covid, it’ll be right that proportion of the time. So be cautious when using this metric.

### Recall/sensitivity, precision, and specificity

These are common metrics that give us a more nuanced view of model performance and aren’t sensitive to imbalance.

**Recall (AKA Sensitivity)**

I’ll use the term *recall* here but this is also called *sensitivity*. It indicates how well a model predicts the positive class. In our case, it tells us what proportion of patients who have Covid are detected as Covid-positve by the model. The higher this number, the fewer Covid-postive cases we’re missing.

We want to correctly identify a high proportion of Covid-positive patients because people who have Covid should take measures to prevent its spread, such as physically isolating, and the diagnosis would determine how health-care professionals treat the case.

**Precision**

This is an indication of how “truthful” the model is when it predicts that a case belongs to the positive class. It tells us what proportion of patients who the model predicts to have Covid actually do. Note the difference to recall. The former asks “what proportion of actually Covid-positive *patients* are correctly identified?” while precision asks “what proportion of Covid-postive *predictions* have Covid?”

Why is this important? If precision is low, a Covid-positive diagnosis can’t be trusted - even if recall is high. For example, a precision of 10% means that 90% of patients diagnosed with Covid are mis-diagnosed and potentially administered an inappropriate treatment. This could be ineffectual at best and even dangerous.

**Specificity**

This is the proportion of negative cases correctly labelled as such. In our example, it’s the proportion of patients who don’t have Covid who get a negative test result. Like recall, it gives the proportion of the class that’s correctly identified but while recall looks at the positive class, specificity looks at the negative class.

This becomes important when there is a high cost to incorrectly labelling the negative class as positive. In our Covid example, if we have a low specificity of 20%, that would mean that 80% of Covid-negtive patients would be incorrectly labelled as Covid-positive.

Individuals diagnosed with Covid may have to forgo income to isolate. They may not be able to fulfill responsibilities like grocery shopping. If they were getting tested for travelling, this would mean not getting on the plane. And it would not only affect them but anyone who came into contact with them who they would be obliged to notify. These are real costs, both subjective and economic, so we would want to keep such mis-labelling this low as well.

**Trade-offs: the classification threshold**

A classification model doesn’t just spit out a label. It predicts the probability of being classified one way or another (for example, Covid-positive or -negative). Engineers need to determine the cutoff above which the model assigns an observation to the positive class. This is called the *classification threshold*.

Commonly, a threshold prediction of 0.5 is used. In our example, that would mean that a probability of 0.5 or higher would lead to the model predicting the patient to have Covid. The actual level used will depend on the subjective weights that you assign to the correctly and incorrectly classifying individuals. For example, if we were giving the Covid test to doctors before they can go to work, we may be much more concerned with ensuring that we pick up any Covid if it exists (high recall) than making sure that all of the people who test positive actually are (high precision) - we may be comfortable telling some healthy doctors to stay at home in order to be extra sure that Covid-positive doctors don’t go to the hospital to see patients.

If we adjust the classification threshold, we will trade off recall on the one hand, and precision and specificity on the other:

- Increase the threshold → higher recall, lower precision / specificity
- Lower the threshold → lower recall, higher precision / specificity

**AUC**

The AUC is a way of combining recall and specificity. What it tells us is this: if we take a random positive and random negative case, the AUC is the probability that we correctly identify which is which. So an AUC of 50% would be no better than chance because I have a 50% probability of being correct if I take a random guess. The closer to 1 the better.

It nicely captures the overall performance of the model without being so susceptible to an imbalance dataset.

**F-score**

The F1-score combines recall and precision. It gives us a way to determine a single score - an average - that takes both measures into account, assuming if we weight each evenly. This is a difficult one to understand intuitively and I found [this article](https://towardsdatascience.com/an-intuitive-guide-to-the-f1-score-55fe8233c79e) very useful in demystifying it. It points out 2 properties of the the F1-score:

1. It can never be smaller than the smaller of precision and recall and it can never be greater than the greater of them. This makes it useful as an average since it’s bounded by the two.
2. It’s closer to the smaller of precision and recall than to the larger. This means that it’s smaller than their arithmetic average (the average that most people are used to). This is useful because it means that you can’t make the F1-score high by making only one of the measures high if the other is low.

There’s also a version of this that allows one to weight one of precision or recall higher than the other, the *generalised FB-score*. You can decide how much importance to give one relative to the other and it will depend on your particular problem. In our example, we’re probably more concerned with correctly identifying a positive Covid patient (recall) than we are about being right whenever we predict that someone has Covid (precision). So we’d assign a higher weight to recall.

### Bringing it all together

Now that we know what all these things are, we can understand what the performance metrics mentioned in the above paper mean:

- **Accuracy:** 93.98%[^1] of the predictions made by the model (positive and negative) were correct
- **Sensitivity:** (= recall): 66.3%[^1] of Covid-positve cases were correctly predicted as such.
- **Precision:** 85.66%[^1] of cases predicted to be Covid-positve actually were.
- **Specificity:** 81.98%[^2] of Covid-negative cases were correctly predicted as such.
- **AUC:** Taking a random positive and negative case from the sample, the model would be able to correctly distinguish which was positive and which negative 94.91%[^1] of the time.
- **F1-score:** if we give equal weight recall and specificity, our model gives an “average” performance of 73.33%[^1].

### Summary

I’ve tried to give you a basic understanding of how to evaluate classification models. There are, of course, many more things that can be said on the topic. But this will give you the fundamentals that you need to start having more informed discussions on the topic or as a springboard for future learning.

---

[^1]: Table 2: [https://www.nature.com/articles/s41598-021-95957-w/tables/2](https://www.nature.com/articles/s41598-021-95957-w/tables/2)

[^2]: Table 3: [https://www.nature.com/articles/s41598-021-95957-w/tables/3](https://www.nature.com/articles/s41598-021-95957-w/tables/3)