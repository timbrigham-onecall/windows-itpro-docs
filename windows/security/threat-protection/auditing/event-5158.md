---
title: 5158(S) The Windows Filtering Platform has permitted a bind to a local port. (Windows 10)
description: Describes security event 5158(S) The Windows Filtering Platform has permitted a bind to a local port.
ms.pagetype: security
ms.prod: windows-client
ms.mktglfcycl: deploy
ms.sitesec: library
ms.localizationpriority: none
author: vinaypamnani-msft
ms.date: 09/08/2021
ms.reviewer: 
manager: aaroncz
ms.author: vinpa
ms.technology: itpro-security
ms.topic: reference
---

# 5158(S): The Windows Filtering Platform has permitted a bind to a local port.


<img src="images/event-5158.png" alt="Event 5158 illustration" width="491" height="466" hspace="10" align="left" />

***Subcategory:***&nbsp;[Audit Filtering Platform Connection](audit-filtering-platform-connection.md)

***Event Description:***

This event generates every time [Windows Filtering Platform](/windows/win32/fwp/windows-filtering-platform-start-page) permits an application or service to bind to a local port.

> **Note**&nbsp;&nbsp;For recommendations, see [Security Monitoring Recommendations](#security-monitoring-recommendations) for this event.

<br clear="all">

***Event XML:***
```
- <Event xmlns="http://schemas.microsoft.com/win/2004/08/events/event">
- <System>
 <Provider Name="Microsoft-Windows-Security-Auditing" Guid="{54849625-5478-4994-A5BA-3E3B0328C30D}" /> 
 <EventID>5158</EventID> 
 <Version>0</Version> 
 <Level>0</Level> 
 <Task>12810</Task> 
 <Opcode>0</Opcode> 
 <Keywords>0x8020000000000000</Keywords> 
 <TimeCreated SystemTime="2015-09-22T05:24:03.376171200Z" /> 
 <EventRecordID>308122</EventRecordID> 
 <Correlation /> 
 <Execution ProcessID="4" ThreadID="3712" /> 
 <Channel>Security</Channel> 
 <Computer>DC01.contoso.local</Computer> 
 <Security /> 
 </System>
- <EventData>
 <Data Name="ProcessId">4556</Data> 
 <Data Name="Application">\\device\\harddiskvolume2\\documents\\listener.exe</Data> 
 <Data Name="SourceAddress">0.0.0.0</Data> 
 <Data Name="SourcePort">3333</Data> 
 <Data Name="Protocol">6</Data> 
 <Data Name="FilterRTID">0</Data> 
 <Data Name="LayerName">%%14608</Data> 
 <Data Name="LayerRTID">36</Data> 
 </EventData>
 </Event>

```

***Required Server Roles:*** None.

***Minimum OS Version:*** Windows Server 2008, Windows Vista.

***Event Versions:*** 0.

***Field Descriptions:***

**Application Information**:

-   **Process ID** \[Type = Pointer\]: hexadecimal Process ID of the process that was permitted to bind to the local port. Process ID (PID) is a number used by the operating system to uniquely identify an active process. To see the PID for a specific process you can, for example, use Task Manager (Details tab, PID column):

    <img src="images/task-manager.png" alt="Task manager illustration" width="585" height="375" />

    If you convert the hexadecimal value to decimal, you can compare it to the values in Task Manager.

    You can also correlate this process ID with a process ID in other events, for example, “[4688](event-4688.md): A new process has been created” **Process Information\\New Process ID**.

<!-- -->

-   **Application Name** \[Type = UnicodeString\]**:** full path and the name of the executable for the process.

    Logical disk is displayed in format \\device\\harddiskvolume\#. You can get all local volume numbers by using **diskpart** utility. The command to get volume numbers using diskpart is “**list volume”**:

<img src="images/diskpart.png" alt="DiskPart illustration" width="786" height="246" />

**Network Information:**

-   **Source Address** \[Type = UnicodeString\]**:** local IP address on which application was bound the port.

    -   IPv4 Address

    -   IPv6 Address

    -   :: - all IP addresses in IPv6 format

    -   0.0.0.0 - all IP addresses in IPv4 format

    -   127.0.0.1, ::1 - localhost

-   **Source Port** \[Type = UnicodeString\]**:** port number which application was bind.

-   **Protocol** \[Type = UInt32\]: number of the protocol that was used.

| Service                                            | Protocol Number |
|----------------------------------------------------|-----------------|
| Internet Control Message Protocol (ICMP)           | 1               |
| Transmission Control Protocol (TCP)                | 6               |
| User Datagram Protocol (UDP)                       | 17              |
| General Routing Encapsulation (PPTP data over GRE) | 47              |
| Authentication Header (AH) IPSec                   | 51              |
| Encapsulation Security Payload (ESP) IPSec         | 50              |
| Exterior Gateway Protocol (EGP)                    | 8               |
| Gateway-Gateway Protocol (GGP)                     | 3               |
| Host Monitoring Protocol (HMP)                     | 20              |
| Internet Group Management Protocol (IGMP)          | 88              |
| MIT Remote Virtual Disk (RVD)                      | 66              |
| OSPF Open Shortest Path First                      | 89              |
| PARC Universal Packet Protocol (PUP)               | 12              |
| Reliable Datagram Protocol (RDP)                   | 27              |
| Reservation Protocol (RSVP) QoS                    | 46              |

**Filter Information:**

-   **Filter Run-Time ID** \[Type = UInt64\]: unique filter ID that allows the application to bind the port. By default, Windows firewall won't prevent a port from being bound by an application. If this application doesn’t match any filters, you'll get value 0 in this field.

    To find a specific Windows Filtering Platform filter by ID, run the following command: **netsh wfp show filters**. As a result of this command, the **filters.xml** file will be generated. Open this file and find specific substring with required filter ID (**&lt;filterId&gt;**)**,** for example:

    <img src="images/filters-xml-file.png" alt="Filters.xml file illustration" width="840" height="176" />

-   **Layer Name** \[Type = UnicodeString\]: [Application Layer Enforcement](/windows/win32/fwp/application-layer-enforcement--ale-) layer name.

-   **Layer Run-Time ID** \[Type = UInt64\]: Windows Filtering Platform layer identifier. To find a specific Windows Filtering Platform layer ID, run the following command: **netsh wfp show state**. As a result of this command, the **wfpstate.xml** file will be generated. Open this file and find specific substring with required layer ID (**&lt;layerId&gt;**)**,** for example:

<img src="images/wfpstate-xml.png" alt="Wfpstate xml illustration" width="1563" height="780" />

## Security Monitoring Recommendations

For 5158(S): The Windows Filtering Platform has permitted a bind to a local port.

-   If you have a predefined application that should be used to perform the operation that was reported by this event, monitor events with “**Application**” not equal to your defined application.

-   You can monitor to see if “**Application**” isn't in a standard folder (for example, not in **System32** or **Program Files**) or is in a restricted folder (for example, **Temporary Internet Files**).

-   If you have a pre-defined list of restricted substrings or words in application names (for example, “**mimikatz**” or “**cain.exe**”), check for these substrings in “**Application**.”

-   Check that “**Source Address”** is one of the addresses assigned to the computer.

-   If you need to monitor all actions with a specific local port, monitor for [5158](event-5158.md) events with that “**Source Port.”**

-   Monitor for all connections with a “**Protocol Number”** that isn't typical for this device or computer, for example, anything other than 6 or 17.

-   If the computer’s communication with “**Destination Address”** should always use a specific “**Destination Port**,**”** monitor for any other “**Destination Port**.”