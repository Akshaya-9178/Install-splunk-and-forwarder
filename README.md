# How to Download and Connect Splunk Universal Forwarder to Splunk Server
If you're building a home lab, working in a security operations center (SOC), or managing a production environment, sending logs to a centralized Splunk server is a crucial step. The Splunk Universal Forwarder (UF) is a lightweight agent that securely forwards logs to your Splunk instance for indexing and analysis.

In this guide, you'll learn how to :

1. Download the Splunk Universal Forwarder
2. Install it on a client machine
3. Configure it to connect to your Splunk Enterprise Server
4. Start Forwarding logs

## Step 1 : Download Splunk Universal Forwarder

I. Go to official Splunk download page

II. Choose your platform

## Step 2 : Install the Forwarder

I. Open your terminal and go to Download folder

II. Move the Downloaded file to /opt folder

Command:- mv **filename** /opt

III. Install it

Command:- sudo apt install ./filename

## Step 3 : Connect Forwarder to Splunk server

Assume your Splunk Enterprise Server is at 192.168.1.100 and listening on the default port 9997.

### Start your forwarder

I. Go to your bin directory

Command :- cd splunkforwarder/splunk/bin

II. Start your splunk forwarder

Command :- sudo ./splunk start --accept-license

III. It ask you username and password (write your new username and password)

-> Please enter an administrator username : admin
-> Please enter a new password : something

III. Add Receiver Info 

Command :- sudo ./splunk add forward-server 192.168.1.100:9997

IV. It ask you your username and password (admin, something)

## Step 4 : Add log Input

I. To moniter Snort logs and send those logs to your Splunk Enterprise Server

Comman :- sudo ./splunk add monitor /var/log/snort/alert

II. You can add /var/log file like 

Command :- sudo ./splunk add monitor /var/log

III. You need to configure your input.conf

IV. So go to your local directory

Command :- cd ..
Command :- cd etc/apps/search

V. Now you enable your root user to go forward

Command :- su root

Command :- cd local

VI. And open Input.comf 

VII. Already 2 line present there

[monitor:///var/log/snort/alert]
disabled = false

VIII. You need to fill this

[splunktcp://9997]
connections_host = 192.168.1.100
[monitor:///var/log/snort/alert]
disabled = false
index = main
sourcetype = snort_alert_full
source = snort

IX. Then come to bin folder and restart forwarder

Command :- sudo ./splunk restart

## Step 5 : Verify Connection on Splunk Server

I. Log in to your Splunk Enterprise GUI

II. Go to setting -> Forwarding and Receiving -> Configure Receiving

III. Make sure port 9997 is open for receiving data

IV. Go to Search & Reporting

V. Click on data summary and you see your system name there below the Hosts

### Note : you need 

I. Python 3.9 is necessary due to its support by the forwarder.

II. Additionally, curl is required for downloading on Ubuntu.

**"The digital world may present its challenges, but it also offers immense opportunity. By embracing a proactive cybersecurity mindset – thinking ahead, securing what matters, and learning from every incident – we don't just protect data; we build a foundation of trust and resilience. Your vigilance today shapes a more secure tomorrow."**



