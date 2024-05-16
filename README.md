# FlareResolver Setup Guide (Proxmox)


# Introduction 
This guide provides step-by-step instructions for setting up FlareResolver in a Proxmox Container (CT). FlareResolver is a tool used to handle Cloudflare challenges and obtain the real IP addresses of websites protected by Cloudflare.

# Prerequisites

Basic knowledge of Proxmox container management
Access to terminal/console of the Proxmox container
Setup Instructions

# Setup
1. Create Proxmox Container (CT)
2. Create a CT named flaresolver.
3. Allocate sufficient resources:
- Debian 11 template
- 8GB storage
- 2 cores
- 512MB RAM
   
# Installation Steps
# Update package lists:
```bash
  sudo apt update
```

# Install required packages:
``` bash
apt install python3 git chromium xvfb python3-pip
```
# Cloning FlareSolverr Repository
Clone the official FlareSolverr Repository [FlareSolverr](https://github.com/FlareSolverr/FlareSolverr)
``` bash
git clone https://github.com/FlareSolverr/FlareSolverr
```
# Navigate to the FlareSolverr directory:
``` bash
cd FlareSolverr
```
# Install Python dependencies:
``` bash
pip install -r requirements.txt
```
# Running FlareSolverr
- Note the IP Address
``` bash
ip a
```
- Now run the FlareSolver Service
``` bash
python3 ./src/flaresolverr.py
```
Note the port (usually 8191).

# Configuration in Prowlarr
- In Prowlarr, add Flaresolverr as an indexer proxy.
- Use the IP address and port noted in the previous step.

```diff
-Note: This is still in manual mode
```

# Setting Up FlareSolverr Service to run automatically
- Clear the shell (Ctrl + C) and enter the following command to edit the service file:
```bash
nano /etc/systemd/system/flaresolverr.service
```
- Paste the following configuration:
```bash
[Unit]
Description=FlareSolverr
After=syslog.target network.target

[Service]
WorkingDirectory=/root/FlareSolverr/
Restart=on-failure
RestartSec=5
Type=simple
ExecStart=/usr/bin/python3 /root/FlareSolverr/src/flaresolverr.py
KillSignal=SIGINT
TimeoutStopSec=20
SyslogIdentifier=FlareSolverr

[Install]
WantedBy=multi-user.target
```
Save and exit the editor.
```diff
-For beginners: Use Ctrl+X  Ctrl+Y and Enter to exit nano
```
# Enable and Start FlareSolverr Service (Automatically)
Enable the FlareSolverr service:
``` bash
systemctl enable flaresolverr.service
```
Start the FlareSolverr service:
``` bash
systemctl start flaresolverr.service
```
Check the status of the FlareSolverr service:
``` bash
systemctl status flaresolverr.service
```
# Reboot (Optional)
If everything is configured correctly, you can reboot the system for changes to take effect.
# Conclusion
Using this we can create flaresolvere in proxmox container which is useful for home media automation using the "arr" stack.

