---
layout: post
title:  "10% of websites fail one or more times per three days"
date:   2015-06-19 01:45:50
---

This post is a result of me prototyping my business idea. At some point I changed my mind about how reasonable the idea is and how big its chances to develop into a successful product. But I did not want to have nothing at the end. So, I am going to try to disprove my conclusion and document it in a series of blog posts. This post is the first of them. 

The idea itself was not that new. It was website monitoring - checking periodically that a webserver answers successfully and the returned content is valid. In particular, the service was supposed to look for:

* connection timeout
* HTTP code
* HTML and Javascript errors by loading the whole webpage in headless browser - [PhantomJS](http://phantomjs.org/) or [SlimerJS](https://slimerjs.org/index.html) 

There are already lots of services doing more or less that. For instance, pingdom.com, uptreands.com, New Relic and others. As opposed to them, there would have been two major differences by my implementation.

First, the target market would be websites which do not have external monitoring yet. Owners of those have not heard about such tools yet or not convinced that the tools are beneficial enough to be paid for. It means that those will need to hear a strong argument to change their mind. The idea was to propose the service to potential customer when their website is down. In order to do that, it would require to monitor the state of websites without notifying their owners. Basically, it means that we would provide the service to the customers for free and try to convince them to pay for it.

 
