### Volatility

Resources used : https://tryhackme.com/room/bpvolatility 

Volatility is a free memory forensics tool developed and maintained by Volatility labs. Regarded as the gold standard for memory forensics in incident response, Volatility is wildly expandable via a plugins system and is an invaluable tool for any Blue Teamer.

Obtaining a memory capture from machines can be done in numerous ways, however, the easiest method will often vary depending on what you're working with. For example, live machines (turned on) can have their memory captured with one of the following tools:

FTK Imager - Link Redline - Link \*Requires registration but Redline has a very nice GUI DumpIt.exe win32dd.exe / win64dd.exe - \*Has fantastic psexec support, great for IT departments if your EDR solution doesn't support this

These tools will typically output a .raw file which contains an image of the system memory. The .raw format is one of the most common memory file types you will see in the wild.

Offline machines, however, can have their memory pulled relatively easily as long as their drives aren't encrypted. For Windows systems, this can be done via pulling the following file:

%SystemDrive%/hiberfil.sys

hiberfil.sys, better known as the Windows hibernation file contains a compressed memory image from the previous boot. Microsoft Windows systems use this in order to provide faster boot-up times, however, we can use this file in our case for some memory forensics!

Things get even more exciting when we start to talk about virtual machines and memory captures. Here's a quick sampling of the memory capture process/file containing a memory image for different virtual machine hypervisors:

```
VMware - .vmem file
Hyper-V - .bin file
Parallels - .mem file
VirtualBox - .sav file *This is only a partial memory file. You'll need to dump memory like a normal bare-metal system for this hypervisor
```

Now that we've collected our memory image let's dig into it!First, let's figure out what profile we need to use. Profiles determine how Volatility treats our memory image since every version of Windows is a little bit different. Let's see our options now with the command

```bash
volatility -f MEMORY_FILE.raw imageinfo
```

In addition to viewing active processes, we can also view active network connections at the time of image creation! Let's do this now with the command

```bash
volatility -f MEMORY_FILE.raw --profile=PROFILE netscan. 
```

Unfortunately, something not great is going to happen here due to the sheer age of the target operating system as the command netscan doesn't support it. It's fairly common for malware to attempt to hide itself and the process associated with it. That being said, we can view intentionally hidden processes via the command `psxview`

In addition to viewing hidden processes via psxview, we can also check this with a greater focus via the command 'ldrmodules'. Three columns will appear here in the middle, InLoad, InInit, InMem. If any of these are false, that module has likely been injected which is a really bad thing. On a normal system the grep statement above should return no output.

Processes aren't the only area we're concerned with when we're examining a machine. Using the 'apihooks' command we can view unexpected patches in the standard system DLLs. If we see an instance where Hooking module: <unknown> that's really bad. Injected code can be a huge issue and is highly indicative of very very bad things. We can check for this with the command `malfind`. Using the full command

	
```
volatility -f MEMORY_FILE.raw --profile=PROFILE malfind -D <Destination Directory> 
```
we can not only find this code, but also dump it to our specified directory.

Last but certainly not least we can view all of the DLLs loaded into memory. DLLs are shared system libraries utilized in system processes. These are commonly subjected to hijacking and other side-loading attacks, making them a key target for forensics.For this we use the command dlllist
