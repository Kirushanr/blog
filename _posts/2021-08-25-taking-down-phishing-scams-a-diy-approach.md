---
category: phishing
title: 'Taking down phishing scams : a DIY approach'
---

# Stop and Think 
If you have **lost money or data to a phishing scam**, please stop reading this article and contact law enforcement or get help. 
* Australia:  https://www.scamwatch.gov.au/get-help/where-to-get-help
* USA: https://www.usa.gov/stop-scams-frauds

# Synopsis

In this article, I revisit a DIY approach I took to take down a scammer's infrastructure during the last holiday season. <br />



# The Link  

You may have received one or more links from scammers with the description of "You have won an iPhone" or "A parcel is waiting for clearance" click here to claim it. Once you click the link, the link may direct you to a website, which could deceive you into entering PII ([Personally Identifiable Information](https://www.oaic.gov.au/privacy/guidance-and-advice/what-is-personal-information/)). 

This attack is identified as phishing but based on the threat vector; it can be classified differently.  For example, **[Smishing](https://www.kaspersky.com.au/resource-center/threats/what-is-smishing-and-how-to-defend-against-it)**  uses SMS as a medium to target users.  <br />



# Analysis

The first step of the analysis begins with the link I received from the scammer. The link is the most important clue, as it may reveal the phishing infrastructures behind the scam. 

I primarily used URLSCAN.io for my investigation . It is a web-based sandbox environment that scans and analyzes a given link. It provides detailed information about the link you were about to click. 


![URLSCAN.IO HOME PAGE](https://user-images.githubusercontent.com/3458013/130765438-1b5d3c09-93ac-4d83-84b8-58393bf5713b.JPG)



An initial scan of that particular URL rendered an empty page. 
As web users, we know that websites may behave differently based on the device/browser. One of the techniques servers use to identify the browser is through the[ User-Agent](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/User-Agent), sent with the request to the server.




The above theory prompted me to try something with URLSCAN.io. I switched and tried multiple User-Agents [4] with different Operating systems.  But I finally got lucky with the mobile user agent iOS Safari,  a nicely designed web app was prompting me to enter my email to claim an iPhone.

![iPhone scam page ](https://user-images.githubusercontent.com/3458013/130765737-a6e923ee-32e1-40ff-83b2-8f0a730e027c.png)








The scammers have hosted the primary user interface, the data collection user interface, on two separate hosting infrastructures. The reason could be that if someone takes down the primary domain and its infrastructure, the data is stored securely in the secondary hosting infrastructure.  


![Secondary Phishing site collecting Credit Card Information ](https://user-images.githubusercontent.com/3458013/130766052-bb1091b0-f0af-4c73-9291-eab386688a3e.png)
<br />




# Reporting

The key to taking down phishing infrastructure is to reporting it to hosting providers. In addition, reporting it to browser makers and antivirus companies could mark the domain as malicious/suspicious.  The below image shows an example of URLSCAN.io results. 

![Identifying hosting infrastructure using URLSCAN.io](https://user-images.githubusercontent.com/3458013/130767394-bad89935-ccc3-4787-b22a-ca9d9c06ca1b.png)



[SwiftOnSecurity](https://twitter.com/SwiftOnSecurity) has written a comprehensive How-To at [Decent Security](https://decentsecurity.com/#/malware-web-and-phishing-investigation/) on how to do this. Let me you take through the list of steps I used. 


- Reporting it to [Google Safe Browsing team](https://safebrowsing.google.com/safebrowsing/report_general/) was the initial step . Google shares this data with third parties, including browsers makers who use Google's Safe Browsing API to block phishing and malware.


- Using the hosting information I gathered in the analysis phase, I reported it to [Amazon Web Services](https://support.aws.amazon.com/#/contacts/report-abuse)  and[ Digital Ocean](https://www.digitalocean.com/company/contact/)  through their abuse forms.  The key is to provide as much information as possible, including a screenshot and links where appropriate. 



-  Since this scam targeted users primarily based in Australia, I  also sent the report to ScamWatch Australia


<br />
<br />


# The waiting game 

Hosting providers get tons of abuse reports a day. The average turnaround time could be days, weeks,  or may be months.

- AWS got back to me after three days, and they took down the primary domain I reported. 


![Response from AWS Support](https://user-images.githubusercontent.com/3458013/130764842-e3fc3fad-5446-49ee-a0f6-162781f528e3.png)



- DigitalOcean did get back to me after two days. However, the site was still active during their investigation even after that.

![Response from Digital Ocean Support](https://user-images.githubusercontent.com/3458013/130764858-a3f80a34-6967-4d5e-b03b-6fccf892c227.png)



- As of this writing none of the reported domains are active.


<br />
# Conclusion 

Taking down phishing scams are similar to the whack a mole game; there will be another  when you take down one. However, reporting them is the key to prevent others from falling into the scam. 

If you need to read the more comprehensive guide, please bookmark  [Decent Security](https://decentsecurity.com/#/malware-web-and-phishing-investigation/) and revisit it when you get another Smishing/Phishing scam.

Happy Hunting !!!.
