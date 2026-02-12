---
title: 'Famous Chollima and Dragon Sickness'
category: threat intelligence
---

![Famous Chollima and Dragon Sickness Cover](https://user-images.githubusercontent.com/3458013/140440412-420cf2e9-8326-4298-b2d7-c1a6df2c101c.png)


> There is nothing like looking, if you want to find something. You certainly usually find something, if you look, but it is not always quite the something you were after.
― J.R.R. Tolkien, The Hobbit, or There and Back Again 


## Disclaimer 

I am not a malware researcher or a reverse engineer. I poke at things when I get bored (Which is pretty often, now more often!).
The samples and behavior of malware that is discussed here are my understanding, expert opinion may be needed. I relied on public information, intelligence reports, IOCs, and observing malware execution in sandbox environments to write this analysis.

## Job Hugging, Economy and DPRK enters the hiring market 

> Job Hugging : "A quiet but widespread pattern: employees clinging to roles that no longer inspire them. After years of pandemic-era disruption and economic uncertainty, many people sought safety over ambition" - [Australian HR Institute](https://www.ahri.com.au/articles/why-job-hugging-is-more-than-just-another-fleeting-work-trend).

There are three patterns (I left the fourth one out, guess it !) that you can observe all across the world
 - Mass layoffs 
 - Prolonged unemployment 
 - Increasing cost of living
 
 Everyone is clutching their pearls !,  especially in our industry because allegedly AI is coming for our jobs(I refer you to Corey Doctorow's [masterpiece](https://www.theguardian.com/us-news/ng-interactive/2026/jan/18/tech-ai-bubble-burst-reverse-centaur)). This domino effect has created a new opportunity for DPRK also known as Democratic Peoples Republic of Korea, a debt-stricken nation with economic sanctions and they are known for getting around this by committing [cybercrime](https://msmt.info/Publications/detail/MSMT%20Report/4221).

North Koreans took this opportunity to target the current job market, especially the IT sector employees who are "Job hugging" or affected by layoffs.
This is dubbed as contagious interview, where the threat actor is luring these employees into taking fake interview assessments that are laced with malware.

The main motivation behind this appears to be targeting crypto-currency wallets, however after analyzing the payload it may also include harvesting credentials that are commonly available on developer workstations. These credentials may be used for further intrusions or pivot to other environments targeted individuals may have access to.


## Contagious Interview & Visual Studio Code : A Better Love Story than Twilight

The term contagious interview is coined by [Unit42 Research Team](https://unit42.paloaltonetworks.com/two-campaigns-by-north-korea-bad-actors-target-job-hunters/). 
In the initial Contagious interview campaign highlights that the threat actors were posing as interviewers and directing people to install malicious NPM packages. But post [shai-hulud](https://unit42.paloaltonetworks.com/npm-supply-chain-attack/), we have lots of eyes on  NPM and other package ecosystem. So spreading malware via this method seem less ROI (Return on Investment) compared to previous years because it can get reported or picked up by a commercial tool, that flags the installation or removed by the package managers.

But we imagine the following scenario where :
 Threat actors at a roundtable, and they are thinking "what can we do next ... ?", they understand the need to pivot. They may have read the Microsoft article published on May 2025 titled [Celebrating 50 Million Developers: The Journey of Visual Studio and Visual Studio Code](https://developer.microsoft.com/blog/celebrating-50-million-developers-the-journey-of-visual-studio-and-visual-studio-code). So the Chief at [Cholima](https://www.crowdstrike.com/adversaries/famous-chollima/) (an adversary associated with DPRK) suggests: "Let's target 50 million developers, shall we?" Kim approves! 

The adversary chose Visual Studio Code because they can easily tap into a software's existing feature that has a large footprint across the globe, at no cost to DPRK.

On November 2025 Paul McCarty (the creator of [OpenSourceMalware](https://opensourcemalware.com/)) published an article titled [Latest Contagious Interview malware campaign abuses Microsoft VSCode Tasks](https://opensourcemalware.com/blog/contagious-interview-vscode), this technique that was observed by Paul(see the diagram below), uses a feature that is built-in Visual Studio Code. When a user trusts the workspace, the tasks.json file in .vscode directory will execute.

![Overview of Malicious Tasks.json execution](https://gist.github.com/user-attachments/assets/6ab623a1-232f-4cf0-8171-0e4e1fa28876)

Now, what is the catch ? The catch is that unsuspecting users will Trust the Workspace, because they are required to complete an interview assessment ([Pretext](https://en.wikipedia.org/wiki/Pretexting)), to prove their worth to their potential employer so that they can get hired. They need money, they need to support their families, most importantly they need to survive. 

The only thing standing between them and malware is this blue prompt shown in the image below. 

![Trust Workspace Prompt shown by VSCode](https://gist.github.com/user-attachments/assets/28f192a4-9b30-4d84-9a5c-bb8fcef15403)

Oscar Wilde packs a punch for this exact situation 
> Whenever a man does a thoroughly stupid thing, it is always from the noblest motives.

DPRK is leveraging current global economic conditions to their advantage. This is  important because threat actors always improve their [tactics and techniques](https://attack.mitre.org/groups/G1052/) based on what is happening around us.


## GitHub & Me 


On Tuesday, 27th January 2026, I read another article published on OpenSourceMalware blog titled [ Small Open-Source Maintainers Targeted by VS Code Tasks Malware ](https://opensourcemalware.com/blog/oss-maintainters-vscode-tasks-compromised).
After reading this, I thought I could look for these malicious patterns in GitHub. Then I remembered that couple of years back, GitHub did improve it's code [search](https://github.com/search), allowing you to use custom filters to search and identify certain code patterns. (Also very useful if you want to identify insecure code patterns across all public repositories, if you are into vulnerability research).

So I started with a simple pattern that looks for code that is located in .vscode/tasks.json and has curl.

```
path : ./vscode/tasks.json "curl"
```

The last part is common, where a malicious script is fetched using curl from a hosting platform then piped (Pipe operator) to shell as input.
The first iteration returned a lot of results, including users making legitimate requests to localhost and 127.0.0.1. 

On my second iteration, I excluded localhost from my search. Then I found the malicious patterns (GitHub search and its indexing methods are a very [entertaining read](https://github.blog/engineering/architecture-optimization/the-technology-behind-githubs-new-code-search/)).

```
path:.vscode/tasks.json "curl http" NOT 127.0.0.1 NOT 0.0.0.0 NOT localhost
```

Something I want to highlight here is that there are ways that would allow threat actors to hide their malicious tasks.json from appearing in search results.
At the time of this writing, threat actors are pivoting to host the initial code within the repository to evade this type of detection. 
In the images shown below, attackers have removed their payload from tasks.json.

***Before***  
![Attacker hosting first payload in tasks.json](https://gist.github.com/user-attachments/assets/7bbb4bf5-237e-4c54-b651-2309210ad4e2)



***After*** 
![Attacker removing the payloads from tasks.json and hosting it part of the code](https://gist.github.com/user-attachments/assets/f62d4e9e-5c21-48a2-83b8-f9de4ffdc0cb)


This small finding later turned into a small hobby project, where I built a small python script ( AI assisted coding). 
This script runs on GitHub action using CRON job trigger. This python script looks for these patterns using GitHub Search API on regular interval. When the findings are reported, The code keeps track of it in a JSON file. When the scan runs it checks if there are any new findings after comparing it to historical findings. Then a GitHub issue, is created,  which I review and report to OpenSourceMalware. 


As I am writing this, there are more than 200+ true positive findings has been flagged (Some of them I have chosen directly report the infrastructure to avoid duplicates).

![GitHub issues Created by the my scanner, when github repositories with the pattern are spotted](https://gist.github.com/user-attachments/assets/12f78f19-e04f-418d-8a33-e04e400b7b95)


Also, note that the final step is manual, because human intervention is required to ensure that it is indeed malicious, this includes reading what the code does, testing the code in sandbox environment, then finally, coming up with a report. 


For anyone reading this I know some of this can be automated by an AI agent, but the important fact is, I will not be learning anything from it. This allowed me to learn a bit about how JavaScript obfuscation works, how these malicious payloads are evading detection and observing these payloads in a sandbox environment. Once we gain understanding of these fundamental behaviours, we can then feed that knowledge to AI (aka. Skill).


## Analyzing and Reporting 


### tasks.JSON execution lifecycle

As we saw earlier in the lifecycle

1) Payload is fetched from a remote source (e.g. Trusted Hosting provider)

2) Payload is then piped to bash or cmd (depends on the OS)

3) Initial payload may make further calls to remote resources to fetch the final payload 

4) Final payload executes on user device

In ***step one*** attackers are using Trusted Hosting providers such as GitHub, Vercel, OnRender because
most organisations may choose to allowlist them in their networks to avoid the constant requests for change and to avoid the backlog of IT tickets filled with requests to allowlisting domains. In addition, most EDRs will not block these platforms for same reasons (Unless they are explictly flagged as malicious). Abuse of hosting platforms is not a new thing, it has been happening for a very long time. 

***Step 2-4*** further payloads are fetched from remote resources, and POST requests are also made to attacker controlled infrastructure.
Final goal of Step 4 exfilteration of credentials from the infected devices. 


### Final Payload Classification 

Based on my observations, I have classifed the final payloads into three categories.
They are : 

1) ***Pure NodeJS exfiltration***

2) ***BeaverTail***

3) ***Etherhiding***


Which final payload drops is controlled by a parameter flag.
This flag controls which final payload lands on the affected user's device.
For example: test-9 flag leveraged [Etherhiding](https://cloud.google.com/blog/topics/threat-intelligence/dprk-adopts-etherhiding) 


#### Pure NodeJS Exfiltration

![Environment variables are sent as POST](https://gist.github.com/user-attachments/assets/46280962-31a1-49c3-a2ea-5de2176ea9c5)

The final JS payload shown in the image above executes on the user device, sends a POST request to attacker controlled API endpoint with a body.
The body of the POST request contains [process.env](https://nodejs.org/en/learn/command-line/how-to-read-environment-variables-from-nodejs). In this particular instance if you have secrets (e.g. GITHUB PAT, AWS Keys, Azure Secrets) configured  as environment variables in your bash/zsh shell, It will be harvested by the attacker. I also wanted to note that, if the user device does not have NodeJS runtime environment installed, it is installed prior to the execution of the final payload!. 

So if you wonder why secrets are in environment variables, I direct you to [12 Factors App](https://12factor.net/config), and also a very known example of configuring [AWS CLI](https://docs.aws.amazon.com/cli/v1/userguide/cli-configure-envvars.html). When developers export the secrets, as part of their existing shell to save some time, the secrets sits there waiting to be stolen.  


#### BeaverTail 

In this particular instance the final payload is BeaverTail malware. This is a fun one because when the final payload was uploaded to VirusTotal only one EDR platform actually detected the payload as malicious.

![Obfuscated BeaverTail payload output in VirusTotal](https://gist.github.com/user-attachments/assets/c39d958d-7102-4898-adb7-d09dc74a0d86)

Now if you want to understand how BeaverTail works, please read  the [MITRE Article](https://attack.mitre.org/software/S1246/) for techniques and references.


#### Etherhiding

The trick with this payload is that it executes scripts fetched from Blockchain, where threat actors can modify them as needed.
There were two addresses I was able to identify in the deobfuscated JS file,

```
0xbe037400670fbf1c32364f762975908dc43eeb38759263e7dfcdabc76380811e
0x3f0e5781d0855fb460661ac63257376db1941b2bb522499e4757ecb3ebd5dce3
```

Both of theses addresses are known as part of Etherhiding techniques used by DPRK.
If you interested in read more about indepth analysis of how this works, I recommend you reading this [article](https://ransom-isac.org/blog/cross-chain-txdatahiding-crypto-heist/) by RANSOM ISAC.


### Evasion techniques and Juliette Clarke 

Threat actors use several techniques to evade both human and security software detection :

1) ***Visual Evasion***:
They are using tabs to right indent the payload so that if you are looking at a tasks.json file you will miss it. 

![Payload is indented to far far from the right](https://gist.github.com/user-attachments/assets/dfe1a270-d2a4-4461-838b-b6727f418136)

2) ***Base64 encoding the payloads*** : 
Most of the payload I observed utlised base64 encoding. Again a known evasion method.

![Example of Base64 encoded payload](https://gist.github.com/user-attachments/assets/e12dfd75-b1b1-46e4-8611-c490e1cf586a)

3) ***Obfuscating JavaScript***  : 
Not all EDRs are created equally, as I mentioned earlier some of these obfuscated payloads are not detected by most AV vendors.
Also, if an untrained user is executing it, they may not understand what the script does.

4) ***Using Trusted hosting platforms to bypass Web Filtering***

5) ***User agent detection to stop abuse teams investigating the initial payload sources***: 
This is quite fun one, and very known behaviour if you have hunted phishing pages. This is a primary defense against online scanners such as Virustotal, if you scan the initial URL, it will return a benign response that is not going to be flagged. Furthermore, it is also a technique to bypass platforms that automate absuse verification, where if they are scanning the affected URL, their scanner may also fail to notice it. Unless, a human explicitly reads the report and pokes around the URL to confirm that it is indeed malicious.

6) ***JWT Use***: This is a new technique I have not observed before. In this particular instance, the JWT is not protected by an algorithm; it is simply a base64-encoded payload with expiration time and a step count (not sure what that means). I was able to bypass this by running the payload in my sandbox environment and observing the entire behavior. I suspect this technique is also has the same reasons as using user agent detection.

![Example of JWT usage](https://gist.github.com/user-attachments/assets/52e4fc77-0ac8-4809-befc-be5a0e99c0d6)

![JWT Decoded](https://gist.github.com/user-attachments/assets/af658c2d-8b40-48ce-937b-c0bb26fc75ff)


I mentioned the name Juliette Clarke in the title because some of the malicious tasks.json files reference this name with the title of Lead Engineer. Of course, it is fiction!. But they do this to make it look more legitimate.


## Detection and Finale

I want to suggest the following approaches for detection and prevention: 

1) ***Developer awareness***:
Please make sure your developers are aware that these threats exist. Trusting public repositories using Visual Studio Code is a really bad idea. 
So always encourage them to inspect the code in ReadOnly mode, before trusting the workspace. Specially, the tasks.json file and language specific execution definitions (e.g. package.json files).
At the time of this writing Microsoft made changes to the Trust Workspace Dialog see the [GitHub issue](https://github.com/microsoft/vscode/issues/287073).

2) ***Using CanaryTokens for Detection*** :
This is my favorite method, if you do not have a large dedicated detection engineering team or SOC (even if you have one).
CanaryTokens are best option for detecting intrustions, that involves credentials harvesting. Please take a look at [Thinkist Canary Tokens project](https://canarytokens.org/nest/) (It is free, also I am not affiliated with this vendor). For example, you can create AWS Keys as canary tokens and export it to your environment variables and patiently wait for them to trip on your wire!.

3) ***Eliminate the usage of long-lived credentials***: I cannot stress this enough.
Please do not create privileged long-lived credentials that gives access to your Cloud environment (e.g. AWS Access Keys) or GitHub PAT. Always use ephemeral credentials where possible. This gives attackers a small time window to act.

4) ***Monitor your environment for anomalies*** : Most stolen credentials especially cookies, secrets should trigger alerts when new sessions/actions are created or performed, if you have established a baseline behaviour of your environment. These alerts are biggest giveaway that someone in your organistion is compromised.
Also, I want to highlight something here: users can access your organization's SaaS platforms on their personal devices, especially if BYOD is allowed (mostly startups fall into this category). If you are relying solely on corporate device detection techniques, you will struggle to detect these threats.

Another notable difficulty small organizations face when using SaaS platforms is the lack of logs unless they are signed up for an Enterprise plan. This is a big problem because attackers can use stolen credentials for reconnaissance and persistence without getting detected. We are entering a phase where SaaS platforms need to proactively alert users if they observe anomalies when tokens or APIs are used. 
***Also, If SaaS  platforms are already collecting the logs why aren't they giving it for free ? ***


> Gandalf: [Dragon sickness](https://pjhobbitlotr.fandom.com/wiki/Dragon_sickness) is a malady that affects all of us.
> [looks at Bilbo]
> Gandalf: Well, almost all of us.


Thank you for reading this post. Hopefully you learned something today!
I will see you in my next post....  

![skeletor-running-away](https://gist.github.com/user-attachments/assets/5d569b75-da5b-46f5-b3ee-82c155b658ea)