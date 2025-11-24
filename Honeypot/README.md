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

<img width="743" height="213" alt="image" src="https://github.com/user-attachments/assets/ef8f1d51-bd73-4fad-be20-c023be565db1" />


