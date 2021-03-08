### YARA 
Main Resource Used : https://tryhackme.com/room/yara

###### Introduction

This fullform of YARA is Yet Another Ridiculous Acronym and its considered to be the pattern matching swiss knife for everyone.

It can identify info based on both binaru and text patterns such as hexadecomal and strings in a file.

To define these patterns we use Rules, these rules are often written to determine if a file is malicious or not.

Strings are fundamental component of programing langugaes, applications use strings to store data such as text. We can use Yara to search those strings in every program running in the OS. 

Malwares use strings to store textual data, like IP addresses or bitcoin addresses.

###### Downloading Yara

1. Apt-get installing it : `sudo apt install yara`
2. Installing from source : `sudo apt install automake libtool make gcc flex bison libssl-dev libjansson-dev libmagic-dev pkg-config`
3. Manually via the Github repo : `wget https://github.com/VirusTotal/yara/archive/v4.0.2.tar.gz` and then follow the steps there.

###### Rules in Yara 

The language used is preety basic in Yara and they are written in .yar extension files, The strenth of rules is basically determined by the patterns used to search for.
Yara command requires 2 arguements to be valid :
1. The rule file
2. Name of file, directory, or process ID to apply the rule.

To make this rule files we can start the file with   
```
rule examplerule {  
        condition: true  
}
```
The name in this case would be examplerule and the condition is condition. Every Rule requires a name and a condition to be valid.

This here checks if the files exist , if the file doesnt exist we get errors. And to run these rules we use yara like this : 
```
yara rule.yar filethattotallyexists
```
A lot of the conditions in Yara are documented here : 
https://yara.readthedocs.io/en/stable/writingrules.html

A few of them are : 

Meta : This is for descriptive information by the author of the rule. Like you can use desc to summarise what your rule checks for. Its basically like comments.

Strings : They can be used to search for specific text or hexadecimal in files or programs. If we wanted to know if the file contains a certain strings lets say we can use Yara rules.

We can define it like this : 
```
rule string_checker {
	strings: 
		$bitcoing_address = "Somerandomaddress"
		$MaliciousIP = "SomeguysIPs"
		
	condition:
		any of them
}
```

This rule will try to search all these strings in the files/paths we define. 

So for the conditions we can also use stuff like Operators <= , >= , != etc. 
For example to use these operators with strings :
```
rule string_checker {
	strings: 
		$bitcoing_address = "Somerandomaddress"
		$MaliciousIP = "SomeguysIPs"
		
	condition:
		$MaliciousIP  <= 10
}
```

So now it will match only if there are less then 10 or 10 occurances of the the Malicious IP , We also have other operators like and,not,or . Example 
```
rule string_checker {
	strings: 
		$bitcoing_address = "Somerandomaddress"
		$txt_file = ".txt"
		
	condition:
		$bitcoing_address and $txt_file
}
```
This rule will now only match with files that have the bitcoin address and have the .txt extension.

Super Handy Cheatsheet : https://medium.com/malware-buddy/security-infographics-9c4d3bd891ef#18dd

###### Modules 
We can use the modules to improve technicality of your yara rules.
Cuckoo : Automated Malware analysis enviornment, allows to generate yara rules on behaviours discovered in its sandbox. 

Python PE : This module allows to create rules from various secitons/ elements of Windows Portable Executable System. 

###### Loki 
This tool helps us to make many rules without building them from scratch.

Quite a lot of other tools can be found here :
https://github.com/InQuest/awesome-yara

This is basically a free Indicator of Compromise scanner. It can be used on Windows and Linux systems.

Many times you gotta research about the latest exploits and then use that information to create exploits, Loki already has a lot of rules that we can benefit from in scanning for evil on the endpoint security. We can use -p to define the path of the files we want Loki to scan.

```
python loki.py -p /path/tocheck
```

###### Thor Lite
This is a newer version of the tool and a multi platform IOC and Yara Scanner, It has scan throttling to limit exhausting CPU resources.

Some other tools are : Fenrir , YAYA (yet another yara automation)


###### Creating Yara Rules with yarGen

The main principle is the creation of yara rules from strings found in malware files while removing all strings that also appear in goodware files. Therefore yarGen includes a big goodware strings and opcode database as ZIP archives that have to be extracted before the first use.
