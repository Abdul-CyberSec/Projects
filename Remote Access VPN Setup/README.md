## In this tutorial we will be looking at how to set up a secure VPN on a Proxmox server using WireGuard or Tailscale, allowing remote access without exposing ports.

#First of all you should already have:
- A running Proxmox VE server
- A Debian/Ubuntu LXC container for the Wireguard/Tailscale VPN
- Network on the container should be set on static


  If you are installing WireGuard follow this following steps:

  #1 Install WireGuard inside our container
  SSH into the container and run
  
