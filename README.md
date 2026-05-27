<h1>Building and Securing Active Directory</h1>


<h2>Description</h2>
Built a Windows Active Directory lab environment to simulate an enterprise network, including domain controllers, user accounts, organizational units (OUs), group policies, and centralized logging with Splunk Enterprise Universal Forwarder. Configured endpoint log collection and monitored authentication events, user activity, and security-related Windows logs to practice SIEM analysis and Active Directory administration.
<br />

## Lab Architecture

The lab environment was built on an Arch Linux host using VirtualBox to simulate an enterprise Active Directory network with centralized monitoring.

### Network Overview

| System | Role | Network | IP Address |
|---|---|---|---|
| ActiveDirectory_DC | Domain Controller | intnet0 | 10.0.0.1 |
| SIEM_server | SIEM / Log Collection | intnet0 + vboxnet0 | 10.0.0.2 / 192.168.56.x |
| Windows_Server_2025 | Member Server | intnet0 | 10.0.0.3 |
| Windows10_Client_01 | Domain Client | intnet0 | 10.0.0.4 |
| Windows10_Client_02 | Domain Client | intnet0 | 10.0.0.5 |

### Network Design

- `intnet0` (`10.0.0.0/24`)
  - Isolated internal enterprise network
  - Used for Active Directory communication and log forwarding

- `vboxnet0` (`192.168.56.0/24`)
  - Host-accessible management network
  - Allows the SIEM server to be remotely accessible

### Architecture Diagram

```text
                           ┌────────────────────┐
                           │    Arch Linux      │
                           │      Host OS       │
                           └─────────┬──────────┘
                                     │
                          vboxnet0 (192.168.56.0/24)
                                     │
                          ┌──────────┴──────────┐
                          │     SIEM_server     │
                          │ 10.0.0.2            │
                          │ 192.168.56.x        │
                          └──────────┬──────────┘
                                     │
                          intnet0 (10.0.0.0/24)
                                     │
           ┌──────────────┬──────────┼────────────────┐
           │              │          │                │
   ┌───────▼───────┐  ┌────▼─────┐ ┌─▼────────┐ ┌─────▼────┐
   │ ActiveDirectory│ │ Windows  │ │ Windows  │ │ Windows  │
   │      _DC       │ │ Server   │ │ Client01 │ │ Client02 │
   │   10.0.0.1     │ │ 10.0.0.3 │ │ 10.0.0.4 │ │ 10.0.0.5 │
   └────────────────┘ └──────────┘ └──────────┘ └──────────┘
```


<h2>Utilities Used</h2>

- <b>PowerShell</b> 
- <b>Active Directory Users and Computers</b>
- <b>Group Policy Management</b>
- <b>Splunk Enterprise</b>
- <b>Splunk Enterprise Universal Forwarder</b>

<h2>Environments Used </h2>

- <b>Arch Linux</b>
- <b>VirtualBox</b>
- <b>Windows Server 2025</b>
- <b>Windows 10</b>
- <b>Ubuntu Server</b>

<h2>Program walk-through:</h2>

<p align="center">
Launch the utility: <br/>
<img src="https://" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Select the disk:  <br/>
<img src="https://" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Enter the number of passes: <br/>
<img src="https://" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Confirm your selection:  <br/>
<img src="https://" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Wait for process to complete (may take some time):  <br/>
<img src="https://" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Sanitization complete:  <br/>
<img src="https://" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Observe the wiped disk:  <br/>
<img src="https://" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
