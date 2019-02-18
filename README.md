---
layout: post
title:  "A/B Testing - Udacity course notes"
date:   2017-03-13
categories: markdown notes
output: html_document

---


A/B Testing course by Udacity, built by Google
====================

Course Overview - 4 sections
------------------------------

### Syllabus

The syllabus for this online class can be found in this [Udacity course link][Udacity on A/B Test].
This course will cover the design and analysis of A/B tests, also known as split tests, which are online experiments used to test potential improvements to a website or mobile application. Two versions of the website are shown to different users - usually the existing website and a potential change. Then, the results are analyzed to determine whether the change is an improvement worth launching. This course will cover how to choose and characterize metrics to evaluate your experiments, how to design an experiment with enough statistical power, how to analyze the results and draw valid conclusions, and how to ensure that the the participants of your experiments are adequately protected.


## Lesson 1) Overview of A/B Testing

**Overview of Business Example:** Audacity, an education provider, creates online finance courses. Considering their customer funnel, we'll consider a change in one of their website features, such as the "Start" button.

Steps in the evaluation process -

### Metric Choice

Two metrics:

* :warning: **CTR%** = no. of clicks/no. of page views
* :thumbsup: **Click-through-probability (CTP%)** = unique visitors who click/unique page visitors. Count at most one child-click per page view.


*Why is the *Click-through-probability* preferred?*
	* Because we do not want to count/consider the no. of times users double-clicked or reloaded a page, we want only want to understand the probability of users who would advance to the next level of the process.
	* "Rates are often better than probabilities for measuring the usability of a feature such as a button, and increasing the size of the "Start Now" button is probably an attempt to increase the usability. Thus, click-through-rate might have been a better choice. However, click-through-probability is a good choice as well."
	

1. **Which Distribution?**
We expect the Click-through-probability to follow a *Binomial distribution*. 



2. **Confidence Intervals**
Calculating a CI:
	*  rule of thumb: if N * p&#770; > 5, then it's safe to assume the distribution to be approx. normal
	*  margin of error = z * SE = z * √(  p&#770; * (1 - p&#770; ) /n)

3. **Hypothesis Testing, or Establishing Statistical Signifiance** - A way to establish how likely it is your results occurred by chance.


### Hypothesis Testing steps

* Null hypothesis H<sub>o</sub>: p<sub>control</sub> = p<sub>exposed</sub>
* Alternative hypothesis H<sub>a</sub>:   p<sub>control</sub> - p<sub>exposed</sub> not equal 0.
* Calculate Prob(p<sub>exposed</sub> - p<sub>control</sub> | Ho), which tells us the difference we observed could have occurred by chance, or if it would be very unlikely to have occurred if the null hypothesis were true.

* Choose a cut-off threshold, called _alpha_, to determine...

* Reject Null hypothesis if Prob(p<sub>exposed</sub> - p<sub>control</sub> | Ho) < _alpha_


### Comparing Two Samples


* **Pooled probability, p&#770;<sub>pool</sub> of a click across groups** (p-hat:an estimated p) = p&#770;<sub>pool</sub> = (X<sub>cont</sub> + X<sub>exp</sub>)/(N<sub>cont</sub> + N<sub>exp</sub>)

**Pooled Standard Error**
* **SE<sub>pool</sub>** = √( p&#770; <sub>pool</sub> * (1- p&#770;<sub>pool</sub> )*(1/N<sub>cont</sub> + 1/N<sub>exp</sub>))

* **difference _d_**= p&#770;<sub>exp</sub> - p&#770;<sub>cont</sub> = X<sub>exp</sub> / N<sub>exp</sub> - X<sub>cont</sub> / N<sub>cont</sub>

Expect d&#770; ~ Normal(0, SE<sub>pool</sub>)

**H<sub>o</sub>**: d&#770; = 0

**H<sub>a</sub>**:  reject null if
d&#770; > 1.96 * SE<sub>pool</sub>, d&#770; or, 
d&#770; < -1.96 * SE<sub>pool</sub>

Example:
![alt text](/images/1-25calcresults.png)


**Practically Significant, or Substantive**
What's next? Is the observed change in click-through probability practically significant?
What size change matters enough to us?

- Rule of thumb: a 1-2% change in click-through probability is quite large even at Google.
- What you want to observe: repeatability, which is what statistically significant is about. Want to size your experiment so that the stat significance bar is lower than the practical significance bar.
- Can be fair to assume from a business perspective, a 2% change in click-through probability would be practically significant.


### Designing the experiment
This is where we need to figure out *how many* page views we need in order to get a statistically significant result to measure the change in CTP%.

**Statistical power**

**Power**
Statistics textbooks frequently define power to mean the same thing as sensitivity, that is, 1 - beta. However, conversationally **power often means the probability that your test draws the correct conclusions, and this probability depends on both alpha and beta.**
In this course, we'll use the second definition, and we'll use sensitivity to refer to 1 - beta.

It has an inverse trade-off with sample size - the smaller the change you want to measure, the larger the size needed (in other words, more page views in your control and exposed groups)


**How many page views or N?**

* alpha (_&alpha;_) = P(rejecting null | Ho true): The probability you are willing to accept for **incorrectly concluding that there was a measured effect**, even though there was not. Plainly speaking, it occurs when we are observing a difference when in truth there is none (or more specifically - no statistically significant difference). 

* beta (_&beta;_) = P(fail to reject null| Ho false), or the **probability of falsely failing to conclude there is a measured diff**, depends on how large your effect really was. 

	* the error of not rejecting a null hypothesis when the alternative hypothesis is the true state of nature. In other
words, this is the error of failing to accept an alternative hypothesis when you don't have adequate power. Plainly speaking, it occurs when we are failing to observe a difference when in truth there is one. 
	* means you are likely to fail to design an experiment that actually did have a measured difference in response rates.
	* larger measured differences will have a lower beta, that is, a lower chance of error.

* **Confidence level** = 1 - _&alpha;_
* 1 - _&beta;_ = **sensitivity** (often ~80%) -> in general, you'd want your experiment to have a high level of sensitivity.

** Why the need to balance &alpha; & &beta
	* in general, we want to have a high level of sensitivity at the practical significance boundary, often around 80%
	* As your sample size goes up with larger samples:
		* &alpha; remains the same
		* &beta; is lower, or the power goes up


#### Online sample size calculator

Use [this calculator](http://www.evanmiller.org/ab-testing/sample-size.html) to determine how many page views we'll need to collect in our experiment. Make sure to choose an absolute difference, not a relative difference.


## Lesson 3) Choosing and Characterizing Metrics


### Defining metrics with other techniques

See this [pdf link](https://s3-us-west-2.amazonaws.com/gae-supplemental-media/additional-techniquespdf/additional_techniques.pdf) for Additional Techniques for Brainstorming and Validating Metrics
These techniques can help you get an understanding of your users, which you can use to come up with
ideas for metrics, validate your existing metrics, or even brainstorm ideas of what you might want to test in
your experiments in the first place.


#### Subsection title
Text 


### Section title


#### Subsection title
Text 


### Section title


#### Subsection title
Text 




## Lesson 4) 


### Section title


#### Subsection title
Text 



Math notation resource:
* https://www.w3schools.com/html/html_entities.asp

* http://sites.psu.edu/symbolcodes/codehtml/#math



[Udacity on A/B Test]: https://de.udacity.com/course/ab-testing--ud257/
