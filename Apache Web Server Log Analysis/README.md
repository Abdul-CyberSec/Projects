# MY GOAL
Analyze Apache access logs to detect malicious activities like brute-force attacks, SQL injection attempts, and directory traversal attacks.

# 1️⃣ You should have a VM with UBUNTU running

Open terminal on Ubuntu and install apache by running this commands :

sudo apt update

sudo apt install apache2

<h2> We can check if apache was installed correctly by running this command: </h2> 
  
  sudo systemctl status apache2

  # 2️⃣ Check web server file directory

run this command:

cd /var/www/html

then run ' ls ' to check for files in current directory
