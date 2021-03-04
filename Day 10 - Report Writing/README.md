
### Report Writting
For Report Writing first of all we gotta choose a template i chose the template from this repository here :

[](https://github.com/noraj/OSCP-Exam-Report-Template-Markdown)[https://github.com/noraj/OSCP-Exam-Report-Template-Markdown](https://github.com/noraj/OSCP-Exam-Report-Template-Markdown)

and this will help us write Report in Markdown and to set this up i highly recommend watching the Video from John Hammond who explains the note taking and using this template in a very nice way:

[](https://www.youtube.com/watch?v=MQGozZzHUwQ&t=411s)[https://www.youtube.com/watch?v=MQGozZzHUwQ&t=411s](https://www.youtube.com/watch?v=MQGozZzHUwQ&t=411s)

And now lets look at the composition of a Report :

1.  Introduction
2.  Executive Summary
3.  Reporting Summary (Technical)
4.  Remediation Summary

### Introduction

For the Introduction Part you can use examples from a few templates and edit them This Section Basically covers information and summary of what task you are given, the Confidentiality Statement so stuff like who has access to this, A disclaimer, and then an Assessment Overview where we basically define the date from which we will be doing the assessment, the activities in a very General Form like Planning, Discovery, Attack, Reporting.

Next in this section, we define the criteria we will be evaluating the system by for example if you are giving a Risk Value for each Vulnerability you can write how you would evaluate that like what is Low Risk, What is a medium risk, and so on and how are you classifying so like are you using a generic low → Medium → High scale or something else you can also use a standard like CVE-Scores to define your Risk Values and state them here.

For getting an example on this i would highly recommend checking out this repository from The Cyber Mentor who has a sample report again that report will also give you a very good idea of what a report should consist of.

Then a small summary of the scope we were allocated and our task should also be included in a section called scope.

Resources :

Template → [](https://github.com/hmaverickadams/TCM-Security-Sample-Pentest-Report)[https://github.com/hmaverickadams/TCM-Security-Sample-Pentest-Report](https://github.com/hmaverickadams/TCM-Security-Sample-Pentest-Report)

### Executive Summary

This part is what consists of us basically summarizing the whole Assessment for the higher ups they don't really care about the specifics of the exploits but they want to know for example lets say how is the overall security looking like, what would be the cost to fix everything up and the number of vulnerabilities (We will have graphs and stuff for that) and now lets try to break this down in sections :

1.  Introduction and Summary of the Report : This is a short summary basically summarizing stuff like since when and what time Time Period the assessment was carried and if we were able to compromise the machines and if we have completed the targets that were set in the Rules of Engagement.
2.  Attack Summary : Here we have the basic summary of the attack and how we managed to compromise the Machines but more on a basic /simple level as we don't wanna have too put too many details for the executives we do a more detailed and through report in the Vulnerability assessment.
3.  Vulnerabilites Found Graph : Here we provide the employers with a graph that can summarize the vulnerabilites in each category like the diagrams like this :

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/15c8c253-0f46-4321-82b0-0a64813927a9/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/15c8c253-0f46-4321-82b0-0a64813927a9/Untitled.png)

Resources that can be used to make graphs :

[](https://www.rapidtables.com/tools/bar-graph.html)[https://www.rapidtables.com/tools/bar-graph.html](https://www.rapidtables.com/tools/bar-graph.html)

[](https://spark.adobe.com/make/charts/bar-graph/)[https://spark.adobe.com/make/charts/bar-graph/](https://spark.adobe.com/make/charts/bar-graph/)

[](https://www.visme.co/graph-maker/)[https://www.visme.co/graph-maker/](https://www.visme.co/graph-maker/)

4.  Security Strengths and Weaknesses : Then in these two sections we basically summarize the Security Strengths of the Organization like if they caught one of your attack, or maybe there is a firewall in place which made the process hard you can compliment annd metnion them here and then in the weaknesses it could be that no one in the company found out there was an attack being carried out , nno two factor authentication in place , etc.

You can also use a diffrent approach and grade your clients Security out of a scale like maybe out of 1-10 or a scale you define.

### Vulnerability Report :

This is the report we go into more technical details about all the vulnerabilities we gathered and this sections we try to divide by hosts/network sections of the organisation as well like for example and first we can organise and write all the vulnerabilities about machines divided into their own sections. So lets say for example we can have it like so :

In our example case lets assume we have two network sections( CORP, ACME.LOCAL) and in those we have two machines ( Machine 1 and Machine2 ) then we can classify it like so :

**CORP Network**

General Idea about how you got into the network

Machine 1 :

Vulnerabilities in this system .

**ACME.LOCAL**

Machine 2 :

Vulnerabilities in this system.

Now let's see how we can describe these vulnerabilities and provide specifications about them for example lets choose :

Input Validation Error in the login form which causes SQLi

Now to Describe this would be great if you can provide as much technical information about this vulnerability as possible and where its caused and if you have access to the source code you can point out the the errors directly in the source code.

For our Example :

---

**SQLi attack due to poor input validation in login form →**

This error happens due to no sanitisation done on the server side and even client side which leads the login form being a place where malicious actors can place malicious payloads and dump the organisation's database and get sensitive information.

Risk : Possible Risk of Sensitive Data Disclosure due to SQL Injection attack which can lead to GDPR fines causing a huge financial impact on the organisation.

Impact Level : Very High

Likelihood : Very Likely

Vulnerable Piece of Code :

/\*Here you can show the Vulnerable code if you have access to the source code \*/

---

### Remediation Summary :

This is a place where you can give an overall summary of the actions needed to fix for the system at first like if there is a weak password policy in place, no anit viruses in place etc they can be addressed at first, and then we can go into more about fixes about each and every vulnerability in the system i personally go over them in the same order i defined the sections in the Vulnerability Section for this , if you have compromised something using a Metasploit module you can look up patches on that exploit, module or you can search the name of the vulnerability and look for patches from the vendor.

If the vulnerability is more about Web exploitation you can use OWASP Guides to make suggestions for the organization and tell them to treat this as a guide for development to avoid mistakes in development, also if its stuff that is popular like shellshock you can look up patches and so on and in some cases you can try providing improved code snippets of the vulnerable piece of code in the system and the same can be done for Buffer Overflows and fixing the vulnerable/defected part of the web app or app in general.

You can also provide soluitions / applications that help such issues like in case of a node js application you can recommend some packages to help fix issues in the webapp.

In this stage you can also provide an estimated budget the company would need to fix these issues.