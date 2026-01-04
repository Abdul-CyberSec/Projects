A honeypot is a decoy system used to attract and trap cyber attackers, diverting them from real targets and allowing you to study their methods and gather valuable threat intelligence

### Scope of the Project

This project focuses on deploying and monitoring an internal honeypot environment using the following components:

- **Cowrie**: A medium‐interaction SSH/Telnet honeypot used to emulate a vulnerable system and capture attacker commands, malware downloads, and brute-force attempts.
- **Zeek**: A powerful network analysis framework used to log and interpret network traffic patterns, enabling deeper insight into attacker behavior and lateral movement attempts.
- **Suricata**: A network intrusion detection and prevention engine used to generate alerts and detect malicious signatures or abnormal traffic activity.
  
In selecting Cowrie, Zeek, and Suricata, the goal was to create a balanced environment capable of capturing both the *how* and the *why* behind attacker activity. 
By combining this tools, this project will help me build a more comprehensive understanding of adversary tactics within a controlled internal network.

### Learning Objectives
- Understand the purpose and role of honeypots in cybersecurity.
- Deploy and configure a honeypot in a controlled environment.
- Monitor and analyze malicious activity captured by the honeypot.
- Document findings to improve threat detection and security response.


# Steps
1. Upload Ubuntu Server ISO to Proxmox storage.
2. Create internal bridge `vmbrHP` (no IP, no ports).


 <img width="1336" height="89" alt="image" src="https://github.com/user-attachments/assets/60f73356-451a-4fde-9962-7b128ceefa9e" />

3. Create VMs:
   - HPcowrie (VM 108) — 2 GB RAM, 2 cores, 20–40 GB disk, network should be → vmbrHP
   - analysis  (VM 109) — 4–8 GB RAM, 2–4 cores, 40–80 GB disk, network should be → vmbrHP
   - attacker  (VM 110) — 4–8 GB RAM, 2–4 cores, 30–60 GB disk, network should be → vmbrHP
    <img width="255" height="71" alt="image" src="https://github.com/user-attachments/assets/efc3d6cb-976e-4920-9b0a-1e5695dea5a7" />

## 4. Set IPs 
**helpful documentation for this step**
- https://documentation.ubuntu.com/server/explanation/networking/configuring-networks/
- https://netplan.readthedocs.io/en/latest/netplan-yaml/

### In order to Find the interface name
run
ip -br a

in this case the 3 virtual machines have the same interface name  <img width="256" height="30" alt="image" src="https://github.com/user-attachments/assets/edbf72c7-a6e6-42dc-a814-4508d870bf18" />

run this command

<img width="858" height="26" alt="image" src="https://github.com/user-attachments/assets/4519cbc0-3671-4d18-96c5-56a604f15d2a" />

then for the HPcowrie I will set this 

<img width="924" height="252" alt="image" src="https://github.com/user-attachments/assets/4bbece17-281d-409e-9b3a-d2dac4a88f07" />
<img width="743" height="213" alt="image" src="https://github.com/user-attachments/assets/d6530ad8-c1f6-4cfa-8a75-55269cde6666" />


For my Analysis VM I will set this

<img width="723" height="224" alt="image" src="https://github.com/user-attachments/assets/3300e580-0970-46d8-9978-40af86410b73" />

<img width="1202" height="167" alt="image" src="https://github.com/user-attachments/assets/f30106a4-60cd-4ec0-8ec5-7f511123d6f3" />


<img width="1146" height="96" alt="image" src="https://github.com/user-attachments/assets/28955e1b-1914-4f85-8189-7885e647f7a2" />

For the attacker VM I will set

<img width="399" height="223" alt="image" src="https://github.com/user-attachments/assets/64f0eb2f-3a1e-436d-bc22-854974db3d8b" />
<img width="779" height="180" alt="image" src="https://github.com/user-attachments/assets/6d963193-0b35-4eba-9117-d99d32aac83e" />

Ping to ensure connectivity 

<img width="699" height="139" alt="image" src="https://github.com/user-attachments/assets/f9a4e562-44a8-4409-b07d-8a3147ab00ca" />
<img width="743" height="246" alt="image" src="https://github.com/user-attachments/assets/cd168b46-74f2-4425-af5a-20ebe2bef671" />


## 5.Installing Cowrrie 

First Install Docker Engine and Compose on my VM 

**You can get the script : https://docs.docker.com/engine/install/ubuntu**

make sure it is up and running

<img width="774" height="649" alt="Screenshot 2025-11-26 142431" src="https://github.com/user-attachments/assets/fcbbd71e-6405-44f7-9657-ff4557fcd4ea" />

Install cowrie from here : https://github.com/cowrie/cowrie  **includes docker support and documentation**

Clone official Cowrie repo
move into the docker directory inside the repository
<img width="753" height="308" alt="image" src="https://github.com/user-attachments/assets/67eebe5c-1a5d-4dbf-87a5-a1ce41453991" />
<img width="499" height="114" alt="image" src="https://github.com/user-attachments/assets/36984e4f-4522-4af2-aad1-9108088a239c" />

make sure you see this message after running this command : **docker run -p 2222:2222 cowrie/cowrie:latest**
<img width="740" height="156" alt="image" src="https://github.com/user-attachments/assets/e722071f-8f97-47ba-bc04-b392d3dd05f1" />


## 6.Installing Zeek & Suricata

 Add the Zeek repository to your Ubuntu system : echo 'deb http://download.opensuse.org/repositories/security:/zeek/xUbuntu_24.04/ /' | sudo tee /etc/apt/sources.list.d/security:zeek.list
<img width="1205" height="71" alt="image" src="https://github.com/user-attachments/assets/f04d4600-80e8-4a80-b88d-52c31ad83d22" />

add the GPG key for the Zeek repository
<img width="814" height="110" alt="image" src="https://github.com/user-attachments/assets/a0616aa3-0b2e-4856-bdc7-68454719f56b" />

Install zeek with this command 
<img width="737" height="29" alt="image" src="https://github.com/user-attachments/assets/5b06dc13-db0d-4a68-908d-0a8e17760fc3" />

Tab and OK to confirm installation with default configuration 

<img width="774" height="487" alt="Screenshot 2025-12-01 221410" src="https://github.com/user-attachments/assets/56952d43-2835-41aa-9f52-a628f28c6409" />

This command will allow you to run the Zeek command line through your terminal
<img width="784" height="80" alt="image" src="https://github.com/user-attachments/assets/caf3e8d9-a016-4b29-b6f3-3cd578161adc" />

run this command to check the Zeek version and Zeek commands
<img width="708" height="50" alt="image" src="https://github.com/user-attachments/assets/28f5c4d4-746e-4e8c-baa2-ccd967e770e8" />

Install Suricata with this command 
<img width="1169" height="75" alt="image" src="https://github.com/user-attachments/assets/de123086-cebd-458a-a9f7-c9b7275792f4" />

enter sudo nano /etc/default/suricata

make sure to change IFACE so it matches your interface
<img width="1178" height="575" alt="image" src="https://github.com/user-attachments/assets/0222e40b-5580-4f72-b598-645c99e13f70" />

make sure Suricata is up and running 
<img width="1198" height="276" alt="Screenshot 2026-01-04 163113" src="https://github.com/user-attachments/assets/e3c68ba6-f2f7-4179-9bdb-136b78c2cb1d" />


