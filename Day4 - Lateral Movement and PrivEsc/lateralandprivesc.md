Somethings we should always check when we have some sort of intial access is always try to drop creds with Mimikatz for example or get the SAM/SYSTEM files to get credentials and try using them somewhere also we can utilize Powerview to basically ennumerate if we can move to some other machines with the current access we have :

```PowerShell
#Machines in current domain where the current user has local admin access
Find-LocalAdminAccess -Verbose
# We can also use other tools like : 
Find-WMILocalAdminAccess.ps1
# Find local admins on all machines
Invoke-EnumerateLocalAdmin -Verbose
# Check where the domain admin has sessions, confirm admin access 
Invoke-UserHunter -CheckAccess
Invoke-UserHunter -Stealth # To do it stealthly

```

Which such techniques and this ennumeration we just did above we can try moving around the network also another great method in my opinion is to go for ennumeration via Bloodhound which presents all this information in one integrated view and you can do queries to find out stuff you can do like paths to admin and so on.

This is a great room for learning more about how to use Bloodhound : https://tryhackme.com/room/postexploit

And for getting Local Admin rights we can use a bunch of tehcniques like automated scirpts like Winpeas.exe and PowerUp.

WinPeas.exe is a suite we can use to basically automate most of the local ennumerationn to find things we can abuse. 
PowerUp.ps1 is another great tool which can help us look/exploit Service Abuse, checking what we can do with a service and much more : https://github.com/PowerShellMafia/PowerSploit/tree/master/Privesc .

We can also use other tools / features to abuse like lets say we have something like SMB or something we can use default creds, the username as the password or so on.
P.S : Powersploit is a great tool in itself.

We can use Powershell Remoting to move to machines inside the network via just powershell also tools like psexec and smbexec exist as well 

```PowerShell
# Create a Session with a specified Port 
New-PSSession -ComputerName Server01 -Port 8081 -UseSSL -ConfigurationName E12 
# Is something we can use to get an interactive session 
Enter-PSSession -ComputerName Server01
```

Also an amaazing resource of Powershell scripts is Nishang : https://github.com/samratashok/nishang

There are scripts for : 
1. Scanning 
2. Bypassing Stuff
3. Escalataion
4. MITM
5. Pivotting
6. Shells
7. Escalation
8. and much more

You can read the ReadMe.md file 


Another thing we should look for is 
```cmd
whoami /priv
```

and then https://www.jaacostan.com/2020/09/printspoofer-windows-privilege.html this article points out that if we have SeImpersonatePrivilege is enabled we can use PrintSpoofer.exe : https://github.com/itm4n/PrintSpoofer


