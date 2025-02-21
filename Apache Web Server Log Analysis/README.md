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

then run ' ls ' to check for files in current directory you should see **index.html** there

# 3️⃣ Apache webpage

enter this command to direct you to the webpage:

firefox index.html

this should direct you to the website which you then will have to change by putting your ip address of your VM

http://<IpAddress>/index.html

you can find your ip address by inputting this in your terminal: **ip a | grep inet**

# 4️⃣ Bash Script

before analysing the apache logs its better if we create a script to generate logs as the traffic to the webpage will be low and there will be not much to analyze

#!/bin/bash


 **Define the Apache access log file path**

 log_file="/var/log/apache2/access.log"
# -------------------------------------------------------------


**Random User-Agents**

USER_AGENTS=(
  "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.124 Safari/537.36
  
  "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Firefox/89.0"
  
  "Mozilla/5.0 (Linux; Android 10; SM-G970F) AppleWebKit/537.36 (KHTML, like Gecko) Mobile Safari/537.36"
  
  "Mozilla/5.0 (iPhone; CPU iPhone OS 14_6 like Mac OS X) AppleWebKit/537.36 (KHTML, like Gecko) Mobile/15E148"
)
# -------------------------------------------------------------
**Random HTTP Methods**

METHODS=("GET" "POST" "PUT" "DELETE")
# -------------------------------------------------------------
**Random URLs (paths)**
URLS=("/index.html" "/login.php" "/wp-admin" "/admin" "/search?q=hello" "/products?id=1' OR '1'='1")
# -------------------------------------------------------------
**Random Response Codes**
STATUS_CODES=("200" "301" "403" "404" "500")
# -------------------------------------------------------------
**Generate logs**

for i in {1..600}; do

  IP="192.168.1.$((RANDOM % 255))"
    
   DATE=$(date +"%d/%b/%Y:%H:%M:%S %z")
    
  METHOD=${METHODS[$RANDOM % ${#METHODS[@]}]}
    
  URL=${URLS[$RANDOM % ${#URLS[@]}]}
  
  STATUS=${STATUS_CODES[$RANDOM % ${#STATUS_CODES[@]}]}
    
  SIZE=$((RANDOM % 5000 + 100))
    REFERRER=$(shuf -e "https://www.google.com" "https://www.bing.com" "https://www.youtube.com" "https://www.facebook.com" "https://twitter.com" "https://www.reddit.com" "https://www.amazon.com" "https://www.instagram.com" -n 1)
    
  USER_AGENT=${USER_AGENTS[$RANDOM % ${#USER_AGENTS[@]}]}

   **echo "$IP - - [$DATE] \"$METHOD $URL HTTP/1.1\" $STATUS $SIZE \"$REFERRER\" \"$USER_AGENT\"" >> $LOG_FILE**
   
   done

echo " Fake Apache Log Entries Generated in $LOG_FILE"

Run the script a couple times
# 5️⃣ Analyzing logs

Go into apache directory 

cat /var/log/apache2/

print apache logs

cat access.log

you will see that there is a lot of information to scan the valuable information

we will first see how many logs are there

wc -l access.log

![image](https://github.com/user-attachments/assets/15e958a0-9467-4478-8f64-4d56086d1879)

I have 1205 logs

if you want to  check for either the tail(last log entries) or head(start of log entries) you can run this command

![image](https://github.com/user-attachments/assets/8a625f26-9df8-4adb-97f0-a226d35e1efa)

![image](https://github.com/user-attachments/assets/238a1116-b789-42a4-92cf-739a6b3f12f1)




