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

### Brute Forcing using Kali

Enabled RDP on the Windows 10 workstation and conducted a brute force attack from Kali Linux using Hydra. The attack targeted the user Jenny Smith (jsmith), utilizing a password list (passwords.txt) containing 21 extracted entries from rockyou.txt. To ensure a successful attack, Jenny's actual password was included in the passwords.txt file.

![kali-linux-2024 4-vmware-amd64-2025-03-22-13-27-59](https://github.com/user-attachments/assets/ad3cedc6-439d-498b-bfe8-7ebb542563e8)

Examining our Splunk instance, we observed 21 logged events associated with the `jsmith` account for Event Code 4625, indicating failed logon attempts—matching the number of passwords in the text file. In contrast, there was one entry for Event Code 4624 which displays a succesful logon attempt.

![image](https://github.com/user-attachments/assets/a150ee43-a260-45b8-ad9d-92d6b5a7c66d)

### Using Atomic Red Team to Emulate an Attack

Atomic Red Team is a collection of straightforward, targeted tests aligned with the MITRE ATT&CK matrix, commonly used to assess an organization's security controls and their ability to detect attacks.

Atomic Red Team was installed on the Windows 10 workstation, where several attack simulations were executed and successfully detected in Splunk.

### Local Account Creation (T1136.001)

A common persistence technique used by attackers on a compromised host is creating new local accounts, which corresponds to technique T1136.001 in the MITRE ATT&CK framework. For this lab, Atomic Red Team was used to create a new local account named "NewLocalUser." You can also see the corresponding Splunk entry below.

![Screenshot 2025-03-25 021656](https://github.com/user-attachments/assets/a8051f48-b5e3-4a95-a09f-1b7c6d917311)
![Screenshot 2025-03-25 021846](https://github.com/user-attachments/assets/2b73d6c7-dffa-445f-ad88-5a2158506048)

### Command and Scripting Interpreter: Windows Command Shell (T1059.003)

Attackers may use cmd to execute commands and payloads, either by running a single command or interactively abusing cmd with input and output relayed through a command-and-control channel.

In my Atomic Red Team simulation, an attack was executed via the Windows command shell by repeatedly launching the Wordpad application, causing excessive resource consumption and rendering my workstation unusable.

![Screenshot 2025-03-25 023346](https://github.com/user-attachments/assets/db70e90d-eb83-449c-9796-c44aca100c76)
![Screenshot 2025-03-25 023835](https://github.com/user-attachments/assets/ded24c0e-e007-4da0-a619-1aa0d3573fdc)

Credits go to MyDFIR: <https://www.youtube.com/@MyDFIR>
