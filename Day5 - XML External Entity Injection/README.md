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