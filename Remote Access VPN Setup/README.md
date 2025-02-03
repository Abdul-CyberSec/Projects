## In this tutorial we will be looking at how to set up a secure VPN on a Proxmox server using WireGuard or Tailscale, allowing remote access without exposing ports.

#❗First of all you should already have:
- A running Proxmox VE server
- A Debian/Ubuntu LXC container for the Wireguard/Tailscale VPN
- Network on the container should be set on static


  <h1> If you are installing WireGuard follow this following steps: </h1> 

  #1️⃣ Install WireGuard inside our container
  SSH into the main node and run this script:
  bash -c "$(wget -qLO - https://github.com/tteck/Proxmox/raw/main/ct/wireguard.sh)"
  ## Script Source: https://tteck.github.io/Proxmox/

  # After the install it will give you an ip address were wireguard will be running off e.g. 192.168.**.**:10086

  #2️⃣ Copy and paste that IP to the search bar and you should be welcomed with this sign in page: 
  
![image](https://github.com/user-attachments/assets/47d2992c-74a2-45a7-8c40-d186ab0e9865)

default user and password is admin | admin

#You will be then be prompted to create an account 

#3️⃣ On the homepage click on configuration where you will be able to create the WireGuard tunnel to create the VPN connection

  ![image](https://github.com/user-attachments/assets/11ad6b59-51df-4c76-9cf6-ff0ee1a85bcd)

# set listen port to 1194, and for the ip address use 10.0.0.1/24

#4️⃣ After that you will need to create the actual client in order to use the VPN

##So you will click this button: ![image](https://github.com/user-attachments/assets/7f569cb8-fa26-44f7-bdfc-c3a7220888fa)
**Note name the peer and you can leave the settings as default**




