---
inFeed: true
hasPage: true
inNav: false
inLanguage: null
starred: false
keywords: []
description: Ran some tests regarding HTTP/2 being automatically enabled on Cloudflare CDN. Results were concerning.
datePublished: '2015-12-03T16:14:18.487Z'
dateModified: '2015-12-03T16:14:05.337Z'
title: Cloudflare HTTP/2 Benchmark Testing
author: []
sourcePath: _posts/2015-12-03-cloudflare-http2-benchmark-testing.md
published: true
authors: []
publisher:
  name: null
  domain: null
  url: null
  favicon: null
url: cloudflare-http2-benchmark-testing/index.html
_type: Article

---
Wes Bos over at [@wesbos][0] tweeted that Cloudflare sent him an email this morning on automatic upgrades to HTTP/2 with SPDY. After some checking around I learned a bit more an ran some tests with my coworker [@HTMelvis][1] to see what we came up with.
Making a rough draft to quickly post up here on my grid site, not sure how much HTML I can use here, so please forgive the lack of tables. I'll post a nice version on my main site when I can.

> Cloudflare just flipped on HTTP/2 for my site! 
> [https://t.co/pD8azXHhUN][2]--- Wes Bos (@wesbos) [December 3, 2015][3]

To begin we ran tests without changing anything under our Cloudflare account. It is worth noting [Cloudflare states][4] that "If you are a customer on the Free or Pro plan, there is no need to do anything at all. Both SPDY and HTTP/2 are already enabled for you." Even so, under the network tab in the dashboard the HTTP/2 + SPDY option was still set to SPDY only.

* First test was from [Pingdom Tools][5]. Numbers that recorded were done after a second test from the same location to ensure cacheing. 
Performance: 85/100 
* Requests: 90 
* Loadtime: 1.07s 
* Page Size: 1.2mb 

Second test was run using [WebPageTest.org][6]. First byte Time: B, Keep Alive Enabled: A, Compress Transfer: A, Compress Images: N/A, Cache static content: A, Effective use of CDN: Check. 

First view results as follows: 

* Document Complete: Load Time: 4.437s,
* Fully Loaded: 5.61s

Second view results: 

* Doc complete: Load Time: 3.032s 
* Fully Loaded: 3.736s

[Google Pagespeed Insights][7] test was next. Mobile 96/100 speed. Desktop 97/100\. 

It is interesting to note that **Chrome Dev Tools** is showing different numbers on this with Dom loading at _1.46_ seconds and Fully loaded at _1.9s_. This led @htmelvis to believe that these test sites might not be fetching with HTTP/2 enabled methods, and that our site indeed already had it turned on by Cloudflare. 

To test this I used [KeyCDN][8] to verify HTTP/2 support. This came back saying that out site supports HTTP/2\. Does this mean that it could be used or is being used? Not clear at this point. 

Next we ran the same sets of test after switching the HTTP/2 + SPDY option on manually under the Network tab in Cloudflare. Waited 5 minutes and manually flushed the CDN cache before these tests were ran.
 

**Pingdom:**

* Performance: 85/100 
* Requests: 90 
* Loadtime: 1.40s 
* Page Size: 1.2mb

* Document Complete: Load Time: 4.621s, 
* Fully Loaded: 6.257s 

Second view results: 

Chrome DevTools: 

* Dom load: 742ms
* Fully loaded: 890ms

[0]: https://twitter.com/wesbos
[1]: https://twitter.com/HTMelvis
[2]: https://t.co/pD8azXHhUN
[3]: https://twitter.com/wesbos/status/672418167994499072
[4]: https://blog.cloudflare.com/introducing-http2/
[5]: http://tools.pingdom.com/
[6]: http://www.webpagetest.org/
[7]: https://developers.google.com/speed/pagespeed/insights/
[8]: https://tools.keycdn.com/http2-test