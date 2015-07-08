---
layout: post
title:  "3.5% of websites fail one or more times per day"
date:   2015-07-07 01:02:03
---

### Assumptions

* 1000 websites selected from [various similarweb tops](http://www.similarweb.com/global);
* 2 monitoring nodes, one in Europe and one in the US;
* a failure is when a website is considered as failing by both nodes for 2 or more data points (one data point is one minute);
* a failure is any connection failure or an error HTTP code; 
* the results were filtered out to exclude obviosly irrelevant data like websites failing more than 20% of checks.

### Preamble

This post is a result of me prototyping my business idea. At some point I changed my mind about how reasonable the idea is and how big its chances to develop into a successful product. But I did not want to have nothing at the end. So, I am going to try to disprove my conclusion and document it in a series of blog posts. This post is the first of them. 

The idea itself was not that new. It was about website monitoring - checking periodically that a webserver answers successfully and the returned content is valid and notify the admin if something is wrong. I am going to elaborate on differences of my idea from existing services in a separate blog post. For now I can say that success of the service would depend on how often websites are down besides other factors.

The data was collected and analyzed with a bunch of Python scripts I wrote for this occasion. They all are on [GitHub](https://github.com/henadzit/webhealth). The graphs are done with iPython notebook and [pandas](http://pandas.pydata.org/).

### Results sanity

To show that there is no noise introduced by monitoring nodes themselves I graphed aggregated failures and processing times from both nodes. The failure rate on both nodes was about 60 during the period. That means that ~6% of the sample websites were down in average. After digging into that I found that some of them are actually dead but some of them have anti-bot alghorithms implemented.

The average processing time does not show anything too concerning. The US node shows slightly better latency though which probably can be explained by the fact that most of the examined websites are hosted in the US.

![European node](/images/node_overal_europe.png)

![US node](/images/node_overal_us.png)

### Looking for failures

The assumptions have been already mentioned in the first paragraph of the post. Basically, the algorithm can be described with the following steps:

* find failures of websites which have two consecutive failing data points for each monitoring node separately;
* remove websites which have more than 20% failure rate;
* find websites which have matching failures on all nodes (failing data points at the same time).

The algorithm implementation is in [the analysis.py module](https://github.com/henadzit/webhealth/blob/master/webhealth/analysis.py#L42).

### Results

The numbers of failed at least once websites for two consecutive days after filtering irrelevant data out:

* 5 Jun, 2015 - 33 
* 6 Jun, 2015 - 38

That is about 3.5% in average. 19 websites among those failed on both days which is nearly half. It makes me think that they may have systematic problems.

Initially I planned to have 3 days but my EC2 instance got a bad neighbor or something and produced lots of false positives on the third day so I had to exclude that data.

Another interesting metric I got is the fraction of failures over all checks. It is 6.4%. Basically, it means that if your Internet surfing interests are equally distributed across 1000 websites, 6.4% of your requests will fail. **You will have to at least refresh every twentieth cat picture**. That's brutal.

Let's examine some of the "winners".

*Indiegogo.com* is the most famous one among the found. It got 2 hour downtime in the European morning. 

![indiegogo.com downtime](/images/website_indiegogo.com.png)

If my understanding is correct, *megafilmeshd.net* is a video streaming service used mostly in Brazil. It seems to have scalling problems since it failed during the peak time (the time is UTC on the graph, Rio is 3 hours ahead of UTC).

![megafilmeshd.net downtime](/images/website_megafilmeshd.net.png)

*Goldsgym.com* seems to run some sort of 2 hour maintenance every day which puts their website down so the only thing you can learn about fitness during this time is "Error establishing a database connection".

![goldsgym.com downtime](/images/website_goldsgym.com.png)


### Conclusion

Websites are down sometimes. Sometimes is quite often. As the trend of API-fication gets [stronger](http://techcrunch.com/2015/03/18/battle-for-the-building-blocks/), there will be more dependencies on external services. There will be more small API providers too which will not be able to provide the same level of availability as big companies do. This may make the Internet more fragile unless there are good practices and technologies out there to make the integration robust and failure tolerant.
