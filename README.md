# A/B TESTING: Adobe Creative Cloud
![header](https://user-images.githubusercontent.com/66885480/218327624-da289b3d-7f36-407d-b44d-5bef07f1c63f.jpg)

**Adobe Creative Cloud** is a set of applications and services from Adobe Corporation that gives subscribers access to a collection of software used for graphic design, video editing, web development, photography, along with a set of mobile applications and also some optional cloud services. In Creative Cloud, a monthly or annual subscription service is delivered over the Internet.

**Company approach:** Users can download and use ACC for one month absolutely free. However, once the free trial ends, they are required to pay for a license to continue using their services. The company is trying to encourage more people to purchase their subscription, regardless of its duration: monthly or annual. 

**Goal:** To increase the number of subscriptions, the company wants to try out some changes in the present layout of the homepage - they want to emphasize more on the free trial option. The hope is that this will encourage more people to download and use the software initially for free and when they find the usability of the product they might consider to acquire a subscription.

Through this notebook, I will perform an experiment to test whether the company should fully deploy this change or not: *Changes in the homepage will encourage more people to download the software and, eventually, acquiring a license (monthly/annual)*

<br/>

## USER FUNNEL

Before jumping straight into the code, it is important to understand how the user funnel works for Adobe Cloud Services. This means, in other words, to understand the steps you normally expect a user will take while visiting the website.

1. Access to the Adobe Creative Cloud homepage
2. Sign up / Sign in
3. Download ACC Services
4. Once the free trial expires, the software takes the user back to the subscription/license website.
5. User sucessfully subscribes to ACC

*Disclaimer:* There will be potential dropoffs in the users who move from one step to the next step in the funnel with only a few making it to the end. That means the probability decreases as you go down in the user funnel. Also, the above user funnel is not a unique set of paths/steps taken by every user visiting that page, e.g., a user might take some additional steps between any two consecutive steps shown above.

<br/>

## DEFINING METRICS: Invariant Metrics & North Star Metrics

Once we fully understand the business funnel, it is important to define one single metric or multiple metrics for the experiment. In the evaluation of the metric, we establish what change in that metric would be practically significant. These metrics will show us whether our experiment group is better than our control group or not.


1. **Invariant Metric or Sanity Check:** Metrics that should not change across your experiment or control group. This metric ensures that the two groups on which we are conducting the experiment are equivalent In this case I will check how similar the cookie distribution is between the control and experiment groups, ensuring that the observed difference in the behavior between the two groups are only due to the changes introduced.

<p align="center">$Number \ of \ Cookies$</p>


2. **North Star Metric:** These metrics will be used to evaluate the performance of the experiment. For the purpose of this experiment, I will be focusing on two: **Download Rate and Purchase Rate**. Calculations are presented below:




<p align="center">$Download \: Rate \: (DR) = \frac{Number \: of \: downloads}{Number \: of \: cookies}$

<p align="center">$Purchase \: Rate \: (PR) = \frac{Number \: of \: purchases}{Number \: of \: cookies}$</p>



*Disclaimer:* It is always a good idea to use ratios as metrics instead of absolute number. Even though we assume the number of cookies assigned to each group to be same, in reality there would be slight imbalance between the two groups.

<br/>

## Experiment Size - Confidence Level - Power of Test 

Before running the experiment, it is important to check the experiment size and to define the confidence level and test power expected. Based on historical data:

* **3250 avg visitors per day**
* **520 avg downloads per day (which represents a download rate equals to 0.16)**
* **65 licenses purchased on avg per day (which represents a purchase rate equals to 0.02)**


### GOAL 1: Increase 50 downloads per day

Let's suppose it is expected an increase of 50 downloads per day: a download rate of 0.175. What is the minimum runtime of the experiment to collect sufficient amount of data to detect a 1.5 pp increase in the DR? Let's define a **significance level of 0.05** (error type I) and a **power of the test of 0.80**

Experiment size calculation

$Experiment \: Size = \big[\big(\frac{Z_{1-\alpha}S_{0}-Z_{\beta}S_{1}}{P_1 - P_0}\big)\big]$

<br/>

$P_1 - P_0$ : Minimum detectable effect *eg.* increment in the download rate <br/>
$S_0$ : Standard Deviation of the control group  <br/>
$S_1$ : Standard Deviation of the experiment group  <br/>
$Z_{1-\alpha}$ : z-score of the control group at $1-\alpha$ <br/>
$Z_{\beta}$ : z-score of the experiment group at $\beta$ <br/>

Considering the significance level and power, let's proceed to the calculation

$P_1 - P_0$ = 0.175 - 0.16 = 0.015 <br/>

$\alpha \equiv$ type I error rate = 0.025 (since overall ${\alpha}$ should be 5%, applying Bonferroni correction to the type I error for both tests gives a value of 0.025) <br/>

$\beta$ = 1 - power = 1 - 0.8 = 0.2 <br/>

$S_0 = \sqrt{ \left( P_0 (1 - P_0) \right) +  \left( P_0 (1 - P_0) \right)}$ <br/>

$S_1 = \sqrt{ \left( P_0 (1 - P_0) \right) +  \left( P_1 (1 - P_1) \right)}$ <br/>

After running all calculations, the **minimum sample size $n = 9481$** is required for this effect to show up in the A/B Test. Based on historical data, there are on avg 3250 unique visitors per day, therefore 1625 unique visitors per group per day. Thus, in order to be able to collect the required amount of data, **we need to run our experiment to detect a 1.5 pp increase in the download rate for 9.481/1.625 = 6 days.**

### GOAL 2: Increase 10 subscriptions (purchase) per day

If the goal is to increase 10 subscriptions per day *(PR 0.020 to 0.023)*, let's define the **minimum size and runtime of the experiment** to collect sufficient amount of data to **detect 0.03 pp increase in the PR**, with ${\alpha}$ = 5% and 80% power?

Following the exact same steps as presented above, to detect a 0.03% increase in the purchase rate, the experiment needs to run for **34.930/1.625 = 21 days.**

*Disclaimer:* Users might not return immediately after the one month free trial period to purchase the software. So we will run our experiment for about one week longer.
