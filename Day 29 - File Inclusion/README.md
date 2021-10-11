```
Resource Used : https://tryhackme.com/room/fileinc
```
Local File inclusion (LFI), or simply File Inclusion, refers to an inclusion attack through which an attacker can trick the web application into including files on the web server by exploiting a functionality that dynamically includes local files or scripts. The consequences of a successful LFI attack include Directory Traversal and Information Disclosure as well as Remote Code Execution.

Typically, Local File Inclusion (LFI) occurs, when an application gets the path to the file that has to be included as an input without treating it as untrusted input. This would allow a local file to be supplied to the included statement.Local File Inclusion is very much like Remote File Inclusion (RFI), with the difference that with Local File Inclusion, an attacker can only include local files (not remote files like in the case of RFI).

You can do ?page=home.html

### Directory Traversal

A path traversal attack (also known as directory traversal) aims to access files and directories that are stored outside the web root folder. By manipulating variables that reference files with “dot-dot-slash (../)” sequences and its variations or by using absolute file paths, it may be possible to access arbitrary files and directories stored on file system including application source code or configuration and critical system files. It should be noted that access to files is limited by system operational access control (such as in the case of locked or in-use files on the Microsoft Windows operating system).

This attack is also known as “dot-dot-slash”, “directory traversal”, “directory climbing” and “backtracking”.

?page=../creditcard

You can do ?page=../../../etc/passwd or something like that

### Log Poinsoning

**In order for that to happen, the directory should have read and execute permissions.**

Applications typically use log files to store a history of events or transactions for later review, statistics gathering, or debugging. Depending on the nature of the application, the task of reviewing log files may be performed manually on an as-needed basis or automated with a tool that automatically culls logs for important events or trending information.

Writing invalidated user input to log files can allow an attacker to forge log entries or inject malicious content into the logs. This is called log injection.

Log injection vulnerabilities occur when:

1. Data enters an application from an untrusted source.
2. The data is written to an application or system log file.

Successful log injection attacks can cause:

1. Injection of new/bogus log events (log forging via log injection)
2. Injection of XSS attacks, hoping that the malicious log event isviewed in a vulnerable web application
3. Injection of commands that parsers (like PHP parsers) could execute

In order for that to happen, the directory should have read and execute permissions.

To read logs try this one →../../../../../var/log/apache2/access.log

RFI is remote file inclusions in this case what the user is able to do is basically plant any external resource in the ?extension thing here but . And this way we can include shells or something to get RCE. 

We can host our shell using a python server and then use that resource to execute what ever code we have specified by it but remember the file extensions in this if using php or something should be .txt in the rfi param as if its php it will be interpreted in our machine instead of our target.

Another uploading file uploading vulnerability could be unrestriced file uploading so like the file lets us upload .php, .aspx files or soemthing and get rev shell via it by traverssing the uploads.

This can be excuted if 2 conditions are met though : file cleaning sanitizing is not done and any extension is allowed and if the uploads directory is guessable or know to the users

To turn LFI to Log Poising and RCE we can do the following :

We will first need to pass a php snippet in the User-Agent field like so when we intercept a request to the php file thats vulnerable to the LFI

![[Pasted image 20211011233528.png]]

and then we can use 

```bash
/.././.././../log/apache2/access.log&cmd=id 
```

To run the command and to see the output of this just scroll down to the bottom of the logs and you will see the command reflected. 

![[Pasted image 20211011233544.png]]

### Base64 Filtering for LFI

For getting files using Base64 filtering LFI attacks we can use this command: 

```bash
?vulnparam=php://filter/convert.base64-encode/resource=index
```

### Use LFI Wordlist to get endpoints

If we have LFI and we dont know what to paths to go for my standards are to do /etc/passwd see the username and then try look for stuff like `.ssh/id_rsa` for that user and stuff like that so we can try bruteforcing the directories a good list we can use to bruteforce :

[https://github.com/carlospolop/Auto_Wordlists/blob/main/wordlists/file_inclusion_linux.txt](https://github.com/carlospolop/Auto_Wordlists/blob/main/wordlists/file_inclusion_linux.txt)