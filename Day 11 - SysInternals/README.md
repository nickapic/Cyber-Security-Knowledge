### Microsoft Sysinternals
 
Main resources used : https://tryhackme.com/room/btsysinternalssg

Sysinternals are basically a set of tools that are windows based and help us with the following utilities :

1. File and Disk Utilies
2. Networking Utilities 
3. Process Utilities
4. Security Utilities
5. System Utilities
6. Miscellaneous

These tools are used a lot by System admins and also Red Teamers to blend in quitely and do malicious activities.

To download Sysinternals we can use : 

https://docs.microsoft.com/en-us/sysinternals/downloads/ : To download only a few specific tools so to say.
If you wanna get the whole suite use this : https://docs.microsoft.com/en-us/sysinternals/downloads/sysinternals-suite

We can also use Sysinternals Live which is a alternative to downloading all the tools and just using their online version.

```
\\live.sysinternals.com\tools\procmon.exe 
```

**For this make sure you have the service webclient/webdav running also Also, Network Discovery needs to be enabled as well. This setting can be enabled in the Network and Sharing Center.**


##### File and Disk Utilities 

Sigchek : Shows File version number, timestamp information, digital signature detials . It can also check files status on virus total.

Syntax :
```
sigcheck -u -e C:\Windows\System32 
# -u means if virus total is enabled show files that are unkown by Virustotal or have a non-zero detection.
# -e scan executable images only
```

Streams : This tool helps us find alternate data streams in files and allow reading and writting in those streams  which can be created due to NTFS file systems. 
Alternate Data Streams (ADS) is a file attribute specific to Windows NTFS (New Technology File System). Every file has at least one data stream ($DATA) and ADS allows files to contain more than one stream of data.
This is often used in Malwares to hide data, but its not all malicious.

```
streams filepath # to reveal streams
notepad filepath:streamname # to open notepad with the stream data
```

SDelete :  This allows us to delete one or more files and/or directories to free space on a logic disk.

It implented the DOD 5220.22-M (Department of Defense clearing and sanitizing protocol).

Other good File and Disk Utilities : https://docs.microsoft.com/en-us/sysinternals/downloads/file-and-disk-utilities

##### Networking Utilities 

TCPview : This shows us all TCP and UDP endpoints on our system, including the local and remote addresses and state of TCP connections. It provides a more informative and conveniently presented subset of the Netstat program that ships with Windows also a cmd line version for it is TCPvcon.

##### Process Utilities

Autoruns : This has comprensive knowledge of auto-starting locations of any startup monitor and shows what tuff starts up when you bootup stuff , its a very powerfull tool to search for any malicious entries created in the local machine for persistence.
```
autoruns
```

Procdump : This for monitorinng an application for CPU spikes and generating crash dumps during a spike to determine the cause of spike. You can create dumps by right click on processes.*

Process Monitor : Advanced monitoring tool that shows real-time file system , registery, process/thread activity. Its very powerfull and can be very usefull for system troubleshooting and malware hunting.

```
procmon
```

it captures events that happen in the OS though you can turn it on and off in the File tab and there are a lot lot of them so to use it efficiently you gotta use filters which is fairly easy.
A good reosurce for it : https://adamtheautomator.com/procmon/

PSexec : This is a replacement for telnet , which lets you execute processes on other systems, like a cmd shell would basically. This utilised a lot in red teaming assesments as well.

##### Security Utilities.

Sysmon : This is a windows system service and device driver that remains across system reboots to monitor and log system activity to windows event log.

##### System Information 

WinObj : This tools uses the native windows api to access and display information on NT object manager's name space.

Session 0 is OS Session , Session 1 is User Session 
```
winobj
```

##### Miscellaneous 

Bginfo : Weird tool that allows you to display relevant information about a windows computer on desktop background.

RegJump : command-line applet takes a registry path and makes Regedit open to that path.