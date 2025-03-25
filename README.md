# Active Directory Lab with Splunk and Atomic Red Team

## Objective
![adproj drawio](https://github.com/user-attachments/assets/ac36a5aa-0f0b-48b4-8e83-d64839c075a3)

This lab focuses on setting up a home Active Directory environment, incorporating Splunk, Kali Linux, and Atomic Red Team. Following MyDFIR's five-part YouTube series, I explored the workings of a domain environment, learned how to ingest events into a SIEM, and generated telemetry for real-world attacks.

### Tools Used
- Windows Server 2019 to install Active Directory services, create user accounts, and set up the Splunk Universal Forwarder and Sysmon for event forwarding to Splunk.
- Splunk Server to install and configure Splunk for receiving logs from Windows Server and workstations, enabling the identification of attacks within the forwarded logs.
- Windows 10 workstation was configured as a target PC within the Active Directory domain, integrating Splunk Universal Forwarder, Sysmon, and Atomic Red Team to simulate MITRE ATT&CK techniques.
- Kali Linux to perform a brute force attack on the Windows 10 workstation using Crowbar.

## Lab Environment Set Up

### Splunk Server
Installed and configured Splunk on a virtual machine running Ubuntu Server 24.04.1. Assigned a static IP of 192.168.10.10 by modifying the `/etc/netplan/00-installer-config.yaml` file, ensuring proper indentation as it is necessary in YAML formatting.

![Ubuntu 64-bit-2025-03-25-01-08-22](https://github.com/user-attachments/assets/c871c023-e072-416a-a266-77f0884c9cbc)
![Screenshot 2025-03-25 010856](https://github.com/user-attachments/assets/a595d09b-e104-4827-ad74-55fda6a22949)

Configured Splunk to listen on port 9997 and enabled web access via a browser at 192.168.10.10:8000. Created an "endpoint" index to receive logs forwarded from the Active Directory server and Windows workstation.

![Screenshot 2025-03-25 012209](https://github.com/user-attachments/assets/c3e56cb3-80de-4cd2-a263-aaab1a84d9b1)

### AD Server
Installed Active Directory services and created the allen.local domain. Established a "Computers" organizational unit and added the Windows 10 PC to it. In addition, I created organizational units for IT and HR, adding two users to each.

![Screenshot 2025-03-25 012546](https://github.com/user-attachments/assets/b4913417-d1bc-4120-8415-f0cf01795ac7)
![image](https://github.com/user-attachments/assets/d537f0e9-8f69-4a50-aac9-c490c1204afd)









Credits go to MyDFIR: <https://www.youtube.com/@MyDFIR>


