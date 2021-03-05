### Core Windows Processes

Resources Used : https://tryhackme.com/room/btwindowsinternals

Learning about this helps us understand the normal behaviour within a windows OS and detect if any issues /malicious activity happen.

Task Manager : This is a main tool GUI based that shows whats running on the Windows System, resource usages and also can be used to kill processes.

You can right click on the places where it says Name, Staus, CPU usage and so on to to show more fields and tabs

-   **Type** \- Each process falls into 1 of 3 categories (Apps, Background process, or Windows process).
-   **Publisher** \- Think of this column as the name of the author of the program/file.
-   **PID** \- This is known as the process identifier number. Windows assigns a unique process identifier each time a program starts. If the same program has multiple processes running, each will have its own unique process identifier (PID).
-   **Process name** \- This is the file name of the process. In the above image, the file name for Task Manager is Taskmrg.exe. 
-   **Command line** \- The full command used to launch the process. 
-   **CPU** \- The amount of CPU (processing power) used by the process.
-   **Memory** \- The amount of physical working memory utilized by the process.

Then you can also go tot he Details tab which has a lot more details about the processes , a good way to know in Details tab if something is of is off is checking the Image Path name and Command Line used for it.

Parent-Child Process view is not a feature in Task Manager , but we can use tools like Process Hacker and Process Exploirer for that. cmd line equivalent :
```
tasklist
Get-Process 
ps 
wmic
```

##### System

This is of the PID 4, this is the home of thread that runs only in kernel mode a kernel-mode system thread.System threads have all the attributes and contexts of regular user-mode threads (such as a hardware context, priority, and so on) but are different in that they run only in kernel-mode executing code loaded in system space, whether that is in Ntoskrnl.exe or in any other loaded device driver. This is run by the Image Path : ```C:\\Windows\\system32\\ntoskrnl.exe (NT OS Kernel)```


###### smss.exe

This process is known for Windows Session Manager which is responsible for creating new sessions, This is the 1st user-mode process by the kernel.Smss.exe starts csrss.exe (Windows subsystem) and wininit.exe in Session 0, an isolated Windows session for the operating system, and csrss.exe and winlogon.exe for Session 1, which is the user session. SMSS is also responsible for creating environment variables, virtual memory paging files and starts winlogon.exe

Normal Configurations are like this : 

**Image Path**:  %SystemRoot%\\System32\\smss.exe

**Parent Process**:  System

**Number of Instances**:  One master instance and child instance per session. The child instance exits after creating the session.

**User Account**:  Local System

**Start Time**:  Within seconds of boot time for the master instance

###### csrss.exe 

This is Client Server runitime process in the user-mode side. This is always running and critical to OS. Responsible for win32 console window and process thread creation and deletion. For each time csrsrv.dll, basesrv.dll, and winsrv.dll are loaded

Normal Configurations :
**Image Path**:  %SystemRoot%\\System32\\csrss.exe

**Parent Process**:  Created by an instance of smss.exe

**Number of Instances**:  Two or more

**User Account**:  Local System

**Start Time**:  Within seconds of boot time for the first 2 instances (for Session 0 and 1).  Start times for additional instances occur as new sessions are created, although often only Sessions 0 and 1 are created.

###### Wininit.exe 

This is responsibel for launching services.exe , lsass.exe and lsaiso.exe within session 0. Vewry crtical process. lsaiso.exe is a process associated with Credential Guard and Key Guard. You will only see this process if Credential Guard is enabled.

**Image Path**:  %SystemRoot%\\System32\\wininit.exe
**Parent Process**:  Created by an instance of smss.exe
**Number of Instances**:  One
**User Account**:  Local System
**Start Time**:  Within seconds of boot time

###### Services.exe
This is Service Control Manager (SCM) and its responsible to handle system services, loading services, startin/ending service also manages db for sc.exe.
Information about services is stored in HKLM\\System\\CurrentControlSet\\Services

Parent Process to vchost.exe, spoolsv.exe, msmpeng.exe, dllhost.exe, to name a few

Normal COnfiguration : 
**Image Path**:  %SystemRoot%\\System32\\services.exe
**Parent Process**:  wininit.exe
**Number of Instances**:  One
**User Account**:  Local System
**Start Time**:  Within seconds of boot time


###### lsass.exe 

This is a process that is responsible for enforcing the security policy on the system.Handles password changes, verifies logins, creates access tokens. It creates security tokens for SAM (Security Account Manager), AD (Active Directory), and NETLOGON.

https://yungchou.wordpress.com/2016/03/14/an-introduction-of-windows-10-credential-guard/

Normal Configuration : 
**Image Path**:  %SystemRoot%\\System32\\lsass.exe
**Parent Process**:  wininit.exe
**Number of Instances**:  One
**User Account**:  Local System
**Start Time**:  Within seconds of boot time

###### winlogon.exe 

This is responsible for Secure Attention Sequence (SAS) , This is the alt+ctrl+delete key combination users press to enter username and passwords. Responsible for loading the user profile. Also responsible for locking the screen and running the user's screensaver, among other functions.

Normal Configuration :
**Image Path**:  %SystemRoot%\\System32\\winlogon.exe
**Parent Process**:  Created by an instance of smss.exe that exits, so analysis tools usually do not provide the parent process name.
**Number of Instances**:  One or more
**User Account**:  Local System
**Start Time**:  Within seconds of boot time for the first instance (for Session 1).  Additional instances occur as new sessions are created, typically through Remote Desktop or Fast User Switching logons.

###### Explorer.exe 

This is the process that gives us acccess to our folders and files, Also functionality to other features as start menu, taskbar,etc.

Normal Configuration :
**Image Path**:  %SystemRoot%\\explorer.exe
**Parent Process**:  Created by userinit.exe and exits
**Number of Instances**:  One or more per interactively logged-in user
**User Account**:  Logged-in user(s)
**Start Time**:  First instance when the first interactive user logon session begins


###### svchost.exe 

This is responsible for hosting and managing windows services. Services running in this process are implemented as DLLs. The DLL to implement is stored in the registry for the service under the `Parameters` subkey in `ServiceDLL`. The full path is `HKLM\SYSTEM\CurrentControlSet\Services\SERVICE NAME\Parameters` . PID 748

Normal Configuration :
**Image Path**: %SystemRoot%\\System32\\svchost.exe
**Parent Process**: services.exe
**Number of Instances**: Many
**User Account**: Varies (SYSTEM, Network Service, Local Service) depending on the svchost.exe instance. In Windows 10 some instances can run as the logged-in user.
**Start Time**: Typically within seconds of boot time. Other instances can be started after boot