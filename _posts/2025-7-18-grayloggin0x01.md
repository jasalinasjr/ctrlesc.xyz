---
layout: post
title: grayloggin0x01
categories: software, updates
---

i have been working with graylog as a siem on my home network recently and i am liking it so far. i was able to get my pfsense logs ingested and i was able to build some extractors for snort detections after a bit of research.

### // open sauce >

i recently deployed a socfortress container in my lab to start testing out the capabilities, but i realized that graylog alone is pretty cool. i have experience with elasticsearch and logscale as siem platforms already, so i figured this wouldn't be too different from either of those. i was able to get my pfsense logs ingested and was glad to see that there were extractors already available for pfsense. however, i decided to configure snort on my pfsense, and i was unable to find any readily available extractors and some of the extractors that i did find did not appear to be working properly. i ended up finding an article where someone posted regex for splunk to parse out the fields for snort detections in pfsense, so i took those and repurposed them for graylog.

- [Graylog Snort pfSense Extractor](https://github.com/jsalinas212/graylog_snort_pfsense_extractor)

### // eof >

i'll see if i can find more time to mess around with graylog and add more posts about it.

- an0malous
