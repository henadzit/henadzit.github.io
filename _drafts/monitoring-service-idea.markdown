---
layout: post
title:  "Monitoring service idea"
---

First, the target market would be websites which do not have external monitoring yet. Owners of those have not heard about such tools yet or not convinced that the tools are beneficial enough to be paid for. It means that those will need to hear a strong argument to change their mind. The idea was to propose the service to potential customer when their website is down. Something like <span style="font-style: italic">"Hey, your website is down! Do you know about that? If you used our awesome service, you would know that"</span>. In order to do that, it would require to monitor the state of websites without notifying their owners. Basically, it means that we would provide the service to the customers for free and try to convince them to pay for it.

The idea itself was not that new. It was website monitoring - checking periodically that a webserver answers successfully and the returned content is valid. In particular, the service was supposed to look for:

* connection timeout
* HTTP code
* HTML and Javascript errors by loading the whole webpage in headless browser - [PhantomJS](http://phantomjs.org/) or [SlimerJS](https://slimerjs.org/index.html) 

There are already lots of services doing more or less that. For instance, pingdom.com, uptreands.com, New Relic and others. As opposed to them, there would have been two major differences by my implementation.

This would require from us to know the list of websites to monitor. Apparently, this information is not that easy to get. I do not have a solution for this problem at the moment. For my research I used [website ranks from SimilarWeb](http://www.similarweb.com/global) and grabbed around 1k sites. The list can be found in [my GitHub repository](https://github.com/henadzit/webhealth/blob/master/data/similarweb-sites.txt).

This would require from us to know the list of websites to monitor. Apparently, this information is not that easy to get. I do not have a solution for this problem at the moment. For my research I used [website ranks from SimilarWeb](http://www.similarweb.com/global) and grabbed around 1k sites. The list can be found in [my GitHub repository](https://github.com/henadzit/webhealth/blob/master/data/similarweb-sites.txt).

 
