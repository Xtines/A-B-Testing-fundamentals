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

Example of calculating CI of difference _d_ using the pooled SE:
![Lesson 1.25 example](/images/1-25diff_pooledSE.png)


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

**Why the need to balance _&alpha;_ & _&beta;_?**

	* in general, we want to have a high level of sensitivity at the practical significance boundary, often around 80%
	
	* As your sample size goes up with larger samples:
		* alpha remains the same
		* beta becomes lower, or the power goes up


#### Online sample size calculator

Use [this calculator](http://www.evanmiller.org/ab-testing/sample-size.html) to determine how many page views we'll need to collect in our experiment. Make sure to choose an absolute difference, not a relative difference.


## Lesson 3) Choosing and Characterizing Metrics


### Defining metrics with other techniques

- Experiments
- External data
- UER
- Focus Group
- Survey
- Retrospective analysis with past data: 
If you are running experiments, you must have logs or other data capture mechanisms to see what users do. Running analyses on this existing set of observational data withoutan experiment structure is called **retrospective analysis, or observational analysis**. These types of analyses are useful for generating ideas for A/B tests and for validating metrics. 
For example, if you observe something in a UER study, you could then go looking for that pattern in your logs to see if that observation bears out and is worth creating a metric about. You could look at the logs to see what the distribution of the latency of video loads might be and when the next user action is to see if that’s an interesting area for exploration. If you observe that a few students in a UER study are getting stuck on a particular quiz, you could analyze all interactions with that quiz to see if that is borne out at scale. Oftentimes, the usefulness of these retrospective analyses is
determined by how you frame the question and the analysis.


#### Comparing to external data
See this [pdf link](https://s3-us-west-2.amazonaws.com/gae-supplemental-media/additional-techniquespdf/additional_techniques.pdf) for Additional Techniques for Brainstorming and Validating Metrics
These techniques can help you get an understanding of your users, which you can use to come up with
ideas for metrics, validate your existing metrics, or even brainstorm ideas of what you might want to test in
your experiments in the first place.


#### Techniques to gather additional data
These methods vary across two dimensions of depth and no. of participants. 

* User Experience Research (UER)
* Focus groups
* Surveys from recruiting a group of users to ask them questions
	* useful for metrics you cannot directly measure
	* careful that answers may be dependent on how questions are phrased


### Clicks - metric definitions

#### Click metric definitions

Def #1 (Cookie probability): For each <time interval>, number of cookies that click divided by number of cookies

Def #2 (Pageview probability): Number of pageviews with a click within <time interval> divided by number of pageviews

Def #3 (Rate): Number of clicks divided by number of pageviews


#### Segmenting and filtering data

When we see an anamoly in a metric, it would be helpful to look at the metric performance across segments to find/isolate where that anamoly occurs. 


### Categories of Summary metrics

* Sums and Counts
* Distributional metrics: Means, medians, percentiles
* Probability and rates
* Ratios: more general than rates, can have any value


### 1. Distribution of the data (with retroactive analysis)

Common distributions - 
* Normal distribution: look at the shape of the data plot of frequency of events against all different values of the events 


#### Common distributions in online data
Let’s talk about some common distributions that come up when you look at real user data.

For example, let’s measure the rate at which users click on a result on our search page, analogously, we could measure the average staytime on the results page before traveling to a result. In this case, you’d probably see what we call a Poisson distribution, or that the stay times would be exponentially distributed.

Another common distribution of user data is a “power-law,” Zipfian or Pareto distribution. That basically means that the probability of a more extreme value, z, decreases like 1/z (or 1/z^exponent). This distribution also comes up in other rare events such as the frequency of words in a text (the most common word is really really common compared to the next word on the list). These types of heavy-tailed distributions are common in internet data.

Finally, you may have data that is a composition of different distributions - latency often has this characteristic because users on fast internet connection form one group and users on dial-up or cell phone networks form another. Even on mobile phones you may have differences between carriers, or newer cell phones vs. older text-based displays. This forms what is called a mixture distribution that can be hard to detect or characterize well.

The key here is not to necessarily come up with a distribution to match if the answer isn’t clear - that can be helpful - but to choose summary statistics that make the most sense for what you do have. If you have a distribution that is lopsided with a very long tail, choosing the mean probably doesn’t work for you very well - and in the case of something like the Pareto, the mean may be infinite!


### 2. Sensitivity and robustness

We want a metric that is both sensitive + robust. Idea is to pick a metric that would be able to measure changes you care about, but is robust against changes that you don't care about. 
For example, the mean of a load time is sensitive to out-liers, while the medium tends to be more robust to outliers. 

#### How to measure them?

* Use an experiment to observe changes in the metric with an introduced change. 
* an "a versus a" experiment
* (if cannot run experiments) a retro-active analysis of log data to observe whether metric(s) changed when you know change(s) were made to your site, then see if the metric(s) of interest moved in conjunction with those site changes. 


### Absolute vs. relative difference

**Absolute vs. relative difference**
Suppose you run an experiment where you measure the number of visits to your homepage, and you measure 5000 visits in the control and 7000 in the experiment. Then the absolute difference is the result of subtracting one from the other, that is, 2000. The relative difference is the absolute difference divided by the control metric, that is, 40%.

**Relative differences in probabilities**
For probability metrics, people often use percentage points to refer to absolute differences and percentages to refer to relative differences. 

For example, if your control click-through-probability (CTP) were 5%, and your experiment click-through-probability were 7%, the absolute difference would be 2 percentage points, and the relative difference would be 40 percent. 

However, sometimes people will refer to the absolute difference as a 2 percent change, so if someone gives you a percentage, it's important to clarify whether they mean a relative or absolute difference!


#### Why Relative differences might be preferred

:thumbsup: By using the relative difference as a metric, you can stick with one practical significance boundary over time in spite of the below changes: 
- When you are running many tests/changes 
- When your system is changing over time
- Seasonality factors present

:warning: Downside for using relative difference is the variability; often good to start with the absolute difference then work your way up to relative differences
- studying a new feature/change


### Variability (measured by variance)


#### Calculating variability by estimating variance 

**The mean and est. variance** depends on the assumed distribution. 

Summary of type of metric / distribution / estimated variance: 

![alt text](/images/3-22calcvariance.png)


#### Example) Difference between Poisson variables

The difference in two Poisson means is not described by a simple distribution the way the difference in two Binomial probabilities is. If your sample size becomes very large, and your rate is not infinitesimally small, sometimes you can use a Normal confidence interval by the law of large numbers. But usually you have to do something a little more complex. For some options, see [here (for a simple summary)](http://ncss.wpengine.netdna-cdn.com/wp-content/themes/ncss/pdf/Procedures/PASS/Tests_for_Two_Poisson_Means.pdf), here [section 9.5 (for a full summary)](http://www.stat.wisc.edu/~wardrop/courses/371chapter9b.pdf) and [here (for one free online calculation)](http://www.evanmiller.org/ab-testing/poisson-means.html). If you have access to some statistical software such as R (free distribution), this is a good time to use it because most programs will have an implementation of these tests you can use.

If you aren’t confident in the Poisson assumption, or if you just want something more practical - and frankly, more common in engineering, see the Empirical Variability section of this lesson.


#### Empirical Variability

Text 



## Lesson 4) Designing an Experiment: Target population, sizing, duration vs. exposure


### Section title


#### Subsection title
Text 



_Markdown math notation resource:_
* https://www.w3schools.com/html/html_entities.asp
* http://sites.psu.edu/symbolcodes/codehtml/#math


[Udacity on A/B Test]: https://de.udacity.com/course/ab-testing--ud257/
