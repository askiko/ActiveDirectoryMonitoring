<h1>Building and Configuring Active Directory</h1>


<h2>Description</h2>
Built a Windows Active Directory lab environment to simulate an enterprise network, including domain controllers, user accounts, organizational units (OUs), group policies, and centralized logging with Splunk Enterprise Universal Forwarder. Configured endpoint log collection and monitored authentication events, user activity, and security-related Windows logs to practice SIEM analysis and Active Directory administration.<br />This environment was later used for attack simulation and detection engineering exercises.
<br/>

## Lab Architecture

The lab environment was built on an Arch Linux host using VirtualBox to simulate an enterprise Active Directory network with centralized monitoring.

### Network Overview

| System | Role | Network | IP Address |
|---|---|---|---|
| ActiveDirectory_DC | Domain Controller | intnet0 | 10.0.0.1 |
| SIEM_server | SIEM / Log Collection | intnet0 + vboxnet0 | 10.0.0.2 / 192.168.56.102 |
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
                          │ 192.168.56.102      │
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

- <b>Active Directory Domain Service</b> 
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
On Windows server, launch server manager and select Add roles and features: <br/>
<img src="https://raw.githubusercontent.com/askiko/my_files/main/ad_setup_shots/1.add_roles_and_features.png" height="80%" width="80%" alt="AD Domain service installation"/>
<br />
<br />
On server roles tab select Active Directory Domain Servive:  <br/>
<img src="https://raw.githubusercontent.com/askiko/my_files/main/ad_setup_shots/2.select_ad_domain_services.png" height="80%" width="80%" alt="AD Domain service installation"/>
<br />
<br />
Click 'Add features' button and follow along with procedures to complete installation: <br/>
<img src="https://raw.githubusercontent.com/askiko/my_files/main/ad_setup_shots/3.add_features.png" height="80%" width="80%" alt="AD Domain serive installation"/>
<br />
<br />
Under tools menu, select Active Directory Users and Computers to create and configure users:  <br/>
<img src="https://raw.githubusercontent.com/askiko/my_files/main/ad_setup_shots/4.active_directory_users_and_computers.png" height="80%" width="80%" alt="AD Domain service installation"/>
<br />
<br />
Navigate back to Tools menu and select Group Policy Management option to configure policies:  <br/>
<img src="https://raw.githubusercontent.com/askiko/my_files/main/ad_setup_shots/5.group_policy_management.png" height="80%" width="80%" alt="AD Domain service installation"/>
<br />
<br />
Log in to windows 10. Launch control pannel and navigate to network settings.<br/> Right-click on the network adapter and select properties.<br/> Under Internet Protocol(ipv4), set preferred DNS as the ip address of domain controller.<br/>
Launch powershell and ping the domain:  <br/>
<img src="https://raw.githubusercontent.com/askiko/my_files/main/ad_setup_shots/7.ping_cj_com.png" height="80%" width="80%" alt="AD Joining steps"/>
<br />
<br />
To join an Active Directory, right-click on This pc and select properties.<br> Scroll down to advanced settings and click. Navigate to Computer Name tab.<br/>
Select Domain and enter the domain name. Enter domain admin username and password:  <br/>
<img src="https://raw.githubusercontent.com/askiko/my_files/main/ad_setup_shots/8.successful_domain_join.png" height="80%" width="80%" alt="AD Joining steps"/>
<br />
<br />
Restart the PC and then sign in with username and password that you created:  <br/>
<img src="https://raw.githubusercontent.com/askiko/my_files/main/ad_setup_shots/9.login_screen_showing_domain_user.png" height="80%" width="80%" alt="AD Joining steps"/>
<br />
<br />
To configure SIEM, sign in to the ubuntu server. Install Splunk Enterpise and configure it.<br/>
Log in to domain controller as admin.<br/>Download 'splunk enterprise universal forwarder' and install. Use IP address of Ubuntu server for server IP.<br/>
Once installation is finished, navigate to C:\Program files\SplunkUniversalForwarder\etc\apps\search folder.<br/>Create a folder named 'local' and inside it create a file named 'inputs.conf' with contents shown in image below:  <br/>
<img src="https://raw.githubusercontent.com/askiko/my_files/main/ad_setup_shots/A.inputs_conf.png" height="80%" width="80%" alt="Splunk configuration"/>
<br />
<br />
Launch CMD as Administrator and change directory to C:\Program FIles\SplunkUniversalForwareder\bin<br/>Type splunk restart and press Enter:  <br/>
<img src="https://raw.githubusercontent.com/askiko/my_files/main/ad_setup_shots/B.restart_splunk_on_cmd.png" height="80%" width="80%" alt="Splunk configuration"/>
<br />
<br />
Launch web browser in any pc and enter the IP address of Ubuntu server with the Splunk web interface port.<br/>Sign in to splunk with username and password that you created.<br/>Click 'New Receiving Port' and enter the port that you configured in SplunkUniversalForwarder:  <br/>
<img src="https://raw.githubusercontent.com/askiko/my_files/main/ad_setup_shots/C.configure_listening_port_on_splunk_web.png" height="80%" width="80%" alt="Splunk configuration"/>
<br />
<br />
Navigate to indexes tab and click 'New Index'. Enter the index name that you configured in inputs.conf file and save:  <br/>
<img src="https://raw.githubusercontent.com/askiko/my_files/main/ad_setup_shots/D.configure_new_index_on_splunk_web.png" height="80%" width="80%" alt="Splunk configuration"/>
<br />
<br />
Observe that events will start populating in the index:  <br/>
<img src="https://raw.githubusercontent.com/askiko/my_files/main/ad_setup_shots/E.logs_are_captured_on_new_index.png" height="80%" width="80%" alt="Splunk configuration"/>
</p>

## Lessons Learned

- Learned Windows administration.
- Improved understanding of Splunk indexes, sourcetypes, and inputs.conf.
- Gained experience configuring VirtualBox internal networking for isolated enterprise environments.
- Learned how Sysmon enhances endpoint visibility for SOC analysis.

<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
