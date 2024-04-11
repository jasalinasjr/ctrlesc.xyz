---
layout: post
title: down with cve
categories: software
---

yeah, you know me.

### // sqli in the sky >

being woken up at 5 in the morning due to a security incident sucks. however, knowing that the incident was squashed before it became a huge issue is awesome.

last month some time i learned about `CVE-2023-48788` from a podcast i listen to regularly. i brought this to my team's attention at work, but as change management normally goes, it was decided that we would need to spin up a test server to ensure that updating the fortiems software wouldn't have an adverse effect on vpn users since fortiems was a few versions behind. while user impact that is something to keep in mind, the process was a bit too slow for my liking anyway.

yesterday, we had a visitor from the netherlands (not likely) say hello through blind sql injection to rce via `xp_cmdshell`. the malicious actor(s) attempted to download and execute malware to establish c2 channel via a curl request. the curl request (and prior commands) slipped by, but the attempt to execute the malicious msi package was caught by the falcon sensor installed on the host to which the falcon soc team immediately contained and began remediation.

this of course lit a fire under everyone's ass to fix the issue on our end.

### // analysis >

i was tasked with conducting an investigation on the facts of the incident, so i started my search via falcon's rtr to start collecting log data for analysis.

initially, i thought the attack was web-based, but i quickly learned that this was executed on the open port that was meant for the forticlient software to connect on for telemetry. the apache logs, while rife with malicious requests and probes, didn't yield any good information.

i was able to spot some indicators in the fortiems das logs of someone testing for sql injection using `OR 1=1 --`, but those resulted in failures. it did, however, give a timeline for when the attacks might have started. i then found when the actor(s) enabled `xp_cmdshell` in mssql. through the falcon sensor logs, i was also able to see that the actor(s) made a request using `invoke-webrequest` in powershell to url `https://forti.requestcatcher.com/$ENV:DOMAIN` to confirm they had rce on the host.

![Request Catcher](/images/request_catcher.png)

i feel that a tool like request catcher should be well known amongst cybersecurity companies, so why wouldn't this be included as an indicator?

Here's a good article on `CVE-2023-48788`:

[CVE-2023-48788](https://www.horizon3.ai/attack-research/attack-blogs/cve-2023-48788-fortinet-forticlientems-sql-injection-deep-dive/)

### // eof >

the moral of the story: patch your shit!