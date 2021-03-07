### Windows Event Logs

Main Resource used : https://tryhackme.com/room/windowseventlogs

Event Logs record events that happen in the execution of a system to provide an audit trail and to understand  the activity of the system and to also find problems.

OS by default also write error messages to these logs. This can be very useful for Blue teamers to find correlations between logs from Multiple sources, leading to statistical analysis and find out events which wouldnt have been found otherwise. 

To help with this we can SIEM tools like : Splunk, Elastic. 
Event though we can access remote machine's logs its not feasable, So thats where SIEM systems come in and help us look at logs from all the endpoints, appliances, etc and they allow us to query logs from multiple devices instead of manually viewing each one.


The Log files are not text files and viewable in text editor but we convert them to XML with Windows API. Common Exentsion : .evt, .evtx 
Common Locations : `C:\\Windows\\System32\\winevt\\Log`

3 main tools : 
1.  **Event Viewer** (GUI-based application)
2.  **Wevtutil.exe** (command-line tool)
3.  **Get-WinEvent** (PowerShell cmdlet)

Each with their pros and cons 

##### Event Viewer 
To open this tool right click on windows button and click on Event Viewer or by writting `eventvwr.msc` in cmd.exe 

It has 3 panes 
1. Left one provides a tree lsiting the event log providers
2. Middle one displays a general overview and summary or the events specific to the selct provider.
3. Right is the actions one. 

There are 5 types of events that can be logged :

**Error**
An event that indicates a significant problem such as loss of data or loss of functionality. For example, if a service fails to load during startup, an Error event is logged.

**Warning**
An event that is not necessarily significant, but may indicate a possible future problem. For example, when disk space is low, a Warning event is logged. If an application can recover from an event without loss of functionality or data, it can generally classify the event as a Warning event.

**Information**
An event that describes the successful operation of an application, driver, or service. For example, when a network driver loads successfully, it may be appropriate to log an Information event. Note that it is generally inappropriate for a desktop application to log an event each time it starts.

**Success Audit**
An event that records an audited security access attempt that is successful. For example, a user's successful attempt to log on to the system is logged as a Success Audit event.

**Failure Audit**
An event that records an audited security access attempt that fails. For example, if a user tries to access a network drive and fails, the attempt is logged as a Failure Audit event.

Source : https://docs.microsoft.com/en-us/windows/win32/eventlog/event-types

In the left pane the standard logs are visible under Windows Logs , Here is a description for each : 

**Application**
Contains events logged by applications. For example, a database application might record a file error. The application developer decides which events to record.

**Security**
Contains events such as valid and invalid logon attempts, as well as events related to resource use such as creating, opening, or deleting files or other objects. An administrator can start auditing to record events in the security log.

**System**
Contains events logged by system components, such as the failure of a driver or other system component to load during startup.

**CustomLog** 
Contains events logged by applications that create a custom log. Using a custom log enables an application to control the size of the log or attach ACLs for security purposes without affecting other applications.

PowerShell will log operations from the engine, providers, and cmdlets to the Windows event log.