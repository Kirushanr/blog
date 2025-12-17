---
title: 'Knowing the unknown:  discovering devices connected to the internet'
category: network
---

![Discovering devices cover](https://user-images.githubusercontent.com/3458013/134617744-0f192a75-23b4-4be6-a8c9-0b37d47f2e77.png)

##  Introduction 

The present-day internet sees everything from smart toothbrushes, unsecured databases, CCTVs  and dirty dishes soaking in a [smart kitchen](https://mashable.com/article/iot-voice-control-delta-faucet-sink)  right now . 

By 2023 the number of devices connected to the internet will be approximately [29.3 billion](https://www.cisco.com/c/en/us/solutions/collateral/executive-perspectives/annual-internet-report/white-paper-c11-741490.html). More devices joining the internet with little or no security  have become a lucrative target for threat actors. 

This article describes a few tools we can use to identify and monitor our devices connected to the internet on a budget. 

> If you know the enemy and know yourself, you need not fear the result of a hundred battles. If you know yourself but not the enemy, for every victory gained, you will also suffer a defeat. If you know neither the enemy nor yourself, you will succumb in every battle. - [Sun Tzu](https://suntzusaid.com/book/3/18)

## Passive reconnaissance 

The first phase of hacking is reconnaissance (recon), where the attacker research and gathers information about a target.  Passive reconnaissance collects information about a target without raising alarms; this phase does not directly interact with a target or its network assets. 

There are SaaS (Software As A Service Platforms), which provide information gathered from actively scanning the internet (Active Reconnaissance). They are passive recon tools. 


### Shodan

[Shodan](https://www.shodan.io/) is the world's first search engine for Internet-connected devices and my favourite tool. 

![Shodan Dashboard of devices connected to the internet](https://user-images.githubusercontent.com/3458013/134498388-497fbab2-3a29-483d-9ff8-b6b4566d0c4e.png)

Shodan allows you to search devices based on various metadata, including but not limited to IP address, [CVE](https://www.redhat.com/en/topics/security/what-is-cve) (Common Vulnerability Exposure), HTTP banners, and the [list go on](https://github.com/jakejarvis/awesome-shodan-queries). 

In addition, they do provide network monitoring features where if something pops up in your given IP address space, the service will send you an alert. According to Shodan's [documentation](https://help.shodan.io/the-basics/on-demand-scanning), it scans the entire internet at least once a week. However, their API does provide on-demand scanning if you want to build continuous scanning and alerting.


<blockquote class="twitter-tweet"><p lang="en" dir="ltr">Quickly check if your IP is exposing any ports to the Internet by visiting <a href="https://t.co/suJA6fndpX">https://t.co/suJA6fndpX</a><br><br>If you see a 404 page then you don&#39;t have anything exposed!</p>&mdash; Shodan (@shodanhq) <a href="https://twitter.com/shodanhq/status/1308110245055660032?ref_src=twsrc%5Etfw">September 21, 2020</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script> 

Shodan pro subscriptions cost 49 $ a month, but they offer 5$/per month pro shodan accounts during the Black Friday sale. 
If you have a .edu email, they offer the pro account features for free. 

### Greynoise 

Greynoise comes in handy if we want to identify any compromised devices within a specified IP address range. 

![Greynoise bulk IP address analysis](https://user-images.githubusercontent.com/3458013/134498699-ee848d0e-87dd-4b51-8c89-f2fdd791dfec.png)

Greynoise does provide a free community subscription, allowing users to search IP addresses to identify whether they have been compromised and scanning the internet for other machines to infect. 

![Example Greynoise insights about an infected machine ](https://user-images.githubusercontent.com/3458013/134498543-a6a6cbfd-9c0f-4c87-b6c2-9dc1547d168f.png)

## Active Recon

### Nmap 

Nmap is the go-to opensource tool for network discovery and if you want to host your network scanning service. Nmap  comes in many flavours. 

- [Zenmap GUI version](https://nmap.org/zenmap/)
- [Dockerized Version](https://github.com/jonlabelle/docker-nmap)
- CLI

Nmap can be used to [find open-ports,  OS fingerprinting](https://nmap.org/book/port-scanning-tutorial.html) and [vulnerabilities](https://securitytrails.com/blog/nmap-vulnerability-scan) too. 


If you need an excellent place to start on continous network scanning Jeremy Gamblin has written a [simple guide](https://jerrygamblin.com/2016/11/05/continuous-network-monitoring-with-slack-alerting/) on that with alerting.


## Conclusion 

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">Attackers put in the time to know the network and the devices better than the defenders. That’s how they win.</p>&mdash; Rob Joyce (@NSA_CSDirector) <a href="https://twitter.com/NSA_CSDirector/status/1437233496351723523?ref_src=twsrc%5Etfw">September 13, 2021</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script> 


As defenders, we need to monitor the devices connected to the internet to prevent/identify a breach. We can only defend what we can see !.

Misconfigured and vulnerable devices are the root cause of some of the notable hacks in the past : 

- [Printers Were Exploited for PewDiePie Propaganda](https://www.wired.com/story/pewdiepie-printers-propaganda-hack-brief/)
- [An unsecured database exposed thousands of British passports](https://www.wired.co.uk/article/uk-passports-exposed-data-breach)
- [Hackers Enlisted Tesla's Public Cloud to Mine Cryptocurrency](https://www.wired.com/story/cryptojacking-tesla-amazon-cloud/)
- [How a Dorm Room Minecraft Scam Brought Down the Internet](https://www.wired.com/story/mirai-botnet-minecraft-scam-brought-down-the-internet/)
