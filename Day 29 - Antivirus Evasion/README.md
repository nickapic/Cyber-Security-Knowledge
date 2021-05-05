Main resources being used : https://tryhackme.com/room/splunk101
**Education Purposes Only**

So usually with many of the boxes that we do on THM and HTB there arent really antiviruses and stuff enabled on them so we have a easier time but for real life and also doing stuff like the pro labs we will need to do Antivirus Evasion and obfuscate our payloads or lets say for phising emails and so on we would need to make more advanced exe files and so on to bypass antivirus.

Lets talk basically about two of the main detection methods in AV's :

Static Detection : This is normally a signature based detection method where it can for example check the md5sum/hashsum of the malware and see if its malicious by comparing it to a database it has lets say. But other than this they also have a little harder bypassing technique called Byte/string matching. Byte matching is another form of signature detection which works by saeerching through the program and matches the byte sequences to the database of bad byte sequences.


Dynamic Detection : This sees how the file acts and there are a few ways on how it can check that like for example it can check the flow of the executable line by line and see what its trying to do. Or it could be executed in a sandbox under close supervision from the AV software and see if it does something suspicious.

So lets talk about some basic bypass for AV :

1. Payload Obfuscation :
So a very common way to bypass some basic AntiVirus is payload obfuscation and just basically making your payloads harder to detect with Static detection and so on for this we can use a lot of diffrent sites here is one that was mentioned in the Wreath lab by Tryhackme for php payload obfuscation : 
```
https://www.gaijin.at/en/tools/php-obfuscator
```

2. Executable and Compiling diffrent versions of tools :


3. Compiling your own Executables :
