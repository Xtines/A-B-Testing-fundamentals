

---
layout: post
title:  "A/B Testing - Udacity course notes"
date:   2017-03-13
categories: markdown notes
output: html_document

---


A/B Testing course from Udacity, built by Google
====================

Course Overview - 4 sections
------------------------------

### Syllabus

The syllabus for this online class can be found in this [Udacity course link][Udacity on A/B Test].
This course will cover the design and analysis of A/B tests, also known as split tests, which are online experiments used to test potential improvements to a website or mobile application. Two versions of the website are shown to different users - usually the existing website and a potential change. Then, the results are analyzed to determine whether the change is an improvement worth launching. This course will cover how to choose and characterize metrics to evaluate your experiments, how to design an experiment with enough statistical power, how to analyze the results and draw valid conclusions, and how to ensure that the the participants of your experiments are adequately protected.


### Overview of A/B Testing
**Overview of Business Example:** Audacity, an education provider, creates online finance courses. Considering their customer funnel, we'll consider a change in one of their website features, such as the "Start" button.

Steps in the evaluation process -

####Metric Choice
Two metrics:

* x **CTR%** = no. of clicks/no. of page views
* -> **Click-through-probability (CTP%)** = unique visitors who click/unique page visitors. Count at most one child-click per page view.


*Why is the *Click-through-probability* preferred?*
Because we do not want to count/consider the no. of times users double-clicked or reloaded a page, we want only want to understand the probability of users who would advance to the next level of the process.


1. **Which Distribution?**
We expect the Click-through probability to follow a *Binomial distribution*


2. **Confidence Intervals**
Calculating a CI:
	*  rule of thumb: if N * p-hat > 5, then it's safe to assume the distribution to be approx. normal
	*  margin of error = z * √(p-hat*(1 - p-hat)/n)
​

3. **Hypothesis Testing, or Establishing Statistical Signifiance** - A way to establish how likely it is your results occurred by chance.

*Hypothesis Testing steps*

* Null hypo Ho: p-control = p-exposed
* Alternative hypo Ha:   p-control - p-exposed not equal 0.
* Calculate Prob(p-exposed - p-control | Ho), which tells us the difference we observed could have occurred by chance, or if it would be very unlikely to have occurred if the null hypothesis were true.

* Choose a cut-off threshold, called Alpha, to determine...



#### **Comparing Two Samples**

**Pooled Standard Error -**

* **Pooled probability, phat-pool** of a click across groups= p-hat(pooled) = (Xcont + Xexp)/(Ncont + Nexp)

* **SE-pool** = √(phat-pool * (1-phat-pool)*(1/Ncont + 1/Nexp))

* **difference d**= phat-exp - phat-cont
d ~ N(0, SEpool)
Ho: d = 0
Ha:  reject null if d > 2 *SEpool, d < -2*SEpool


**Practically Significant, or Substantive**
What's next? Is the observed change in click-through probability practically significant?
What size change matters enough to us?

- Rule of thumb: a 1-2% change in click-through probability is quite large even at Google.
- What you want to observe: repeatability, which is what statistically significant is about. Want to size your experiment so that the stat significance bar is lower than the practical significance bar.
- Can be fair to assume from a business perspective, a 2% change in click-through probability would be practically significant.


#### Designing the experiment
This is where we need to figure out *how many* page views we need in order to get a statistically significant result to measure the change in CTP%.

**Statistical power** defined:

* power has an inverse trade-off with sample size - the smaller the change you want to measure, the larger the size needed (in other words, more page views in your control and exposed groups)


**How many page views or N?**

* alpha = P(rejecting null | Ho true); "he probability you are willing to accept for **incorrectly concluding that your improvement was successful**, even though it was not."

* beta B = P(fail to reject| Ho false), or the **probability of falsely failing to conclude there is a measured diff**, depends on how large your effect really was.
-> means you are likely to fail to design an experiment that actually did have a measured difference in response rates.

*ex)* larger measured differences will have a lower beta, that is, a lower chance of error.

* Confidence level = 1 - alpha
* 1-beta = statistical power/sensitivity (often ~80%) -> in general, you'd want your experiment to have a high level of sensitivity.

** the need to balance alpha & beta:**
why?



****

###Section title


#### Subsection title
Text text text
















[Udacity on A/B Test]: https://de.udacity.com/course/ab-testing--ud257/
