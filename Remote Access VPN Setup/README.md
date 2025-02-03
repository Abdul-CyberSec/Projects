## In this tutorial we will be looking at how to set up a secure VPN on a Proxmox server using WireGuard or Tailscale, allowing remote access without exposing ports.

<h2>❗First of all you should already have: </h2>

- A running Proxmox VE server

- A Debian/Ubuntu LXC container for the Wireguard/Tailscale VPN
  
- Network on the container should be set on static


  <h1> If you are installing WireGuard follow this following steps: </h1> 

  <h3>1️⃣ Install WireGuard inside our container</h3>
  
  SSH into the main node and run this script:
  
  bash -c "$(wget -qLO - https://github.com/tteck/Proxmox/raw/main/ct/wireguard.sh)"
  
  **Script Source: https://tteck.github.io/Proxmox/**

  # After the install it will give you an ip address were wireguard will be running off e.g. 192.168.**.**:10086

  <h3>2️⃣ Copy and paste that IP to the search bar and you should be welcomed with this sign in page: </h3>
  
![image](https://github.com/user-attachments/assets/47d2992c-74a2-45a7-8c40-d186ab0e9865)

default user and password is admin | admin

You will  then be prompted to create an account 

<h3>3️⃣ On the homepage click on configuration where you will be able to create the WireGuard tunnel to create the VPN connection </h3>

  ![image](https://github.com/user-attachments/assets/11ad6b59-51df-4c76-9cf6-ff0ee1a85bcd)

 set listen port to 1194, and for the ip address use 10.0.0.1/24

<h3>4️⃣ After that you will need to create the actual client in order to use the VPN </h3>

## click this button:
![image](https://github.com/user-attachments/assets/7f569cb8-fa26-44f7-bdfc-c3a7220888fa)

**you can leave the settings as default**


Then VPN will be ready ❗


 <h1> If you are installing Tailscale follow this following steps: </h1> 

  <h3>1️⃣ Install Tailscale inside our container </h3>
  
  SSH into the into the container, and run this script:
  
  curl -fsSL https://tailscale.com/install.sh | sh
  ## Script Source:(https://tailscale.com/download/linux)

  **Enabling SSH connection in the container**
  
  Log in to the container using default user name root and password you picked when you set it up
  then type this to enable a SSH connection:
  
  nano /etc/ssh/sshd_config
  
  Find PermitRootLogin and remove # and add yes after it

  ![image](https://github.com/user-attachments/assets/fc8a4ccb-9389-4530-ad1c-167e0d1227e1)

  
  ctrl + O to save and the ctrl + X to exit nano mode

 <h3>2️⃣ Before setting up Tailscale, enable IPv4 and IPv6 forwarding to allow subnet advertising.</h3>

 to enable that use this nano command :

 **nano /etc/sysctl.conf**

  scroll down and uncomment the lines like the image shown below :

  ![image](https://github.com/user-attachments/assets/1df0995f-1e4b-423d-b808-86fc269048ee)

  ctrl + O to save and the ctrl + X to exit nano mode

  **SHUTDOWN THE CONTAINER**
  
  3️⃣Run command lines into Proxmox main node in SSH

  enter this command :

 nano /etc/pve/lxc/103.conf

  **NOTE** 103 is my LXC ID in your case it might be different

  then add this two command lines below the unpriveleged line:
  
lxc.cgroup2.devices.allow: c 10:200 rwm
lxc.mount.entry: /dev/net/tun dev/net/tun none bind,create=file

**Script Source: https://tailscale.com/kb/1130/lxc-unprivileged**

![image](https://github.com/user-attachments/assets/ff7c3e5a-7e0e-446a-b575-0221a6152b64)

4️⃣ TAILSCALE IS UP AND READY

Start the container by running tailscale up --advertise routes=**<YOUR IP ADDRESS>** --advertise-exit-node

**scripts:** 
https://tailscale.com/kb/1019/subnets
https://tailscale.com/kb/1408/quick-guide-exit-nodes

  

  

