FlareResolver Setup Guide
Introduction
This guide provides step-by-step instructions for setting up FlareResolver in a Proxmox Container (CT). FlareResolver is a tool used to handle Cloudflare challenges and obtain the real IP addresses of websites protected by Cloudflare.

Prerequisites
Proxmox environment
Basic knowledge of Proxmox container management
Access to terminal/console of the Proxmox container
Setup Instructions
1. Create Proxmox Container (CT)
Create a CT named flaresolver.
Allocate sufficient resources:
Debian 11 template
8GB storage
2 cores
512MB RAM
2. Installation Steps
Connect to the Proxmox container console.
Update package lists:
bash
Copy code
apt update
Install required packages:
bash
Copy code
apt install python3 git chromium xvfb python3-pip
3. Cloning FlareSolverr Repository
Clone the FlareSolverr repository:
bash
Copy code
git clone https://github.com/FlareSolverr/FlareSolverr
Navigate to the FlareSolverr directory:
bash
Copy code
cd FlareSolverr
Install Python dependencies:
bash
Copy code
pip install -r requirements.txt
4. Running FlareSolverr
Execute FlareSolverr:
bash
Copy code
python3 ./src/flaresolverr.py
Note the port (usually 8191) and the IP address (use ip a command).
5. Configuration in Prowlarr
In Prowlarr, add Flaresolverr as an indexer proxy.
Use the IP address and port noted in the previous step.
6. Setting Up FlareSolverr Service
Clear the shell (Ctrl + C) and enter the following command to edit the service file:
bash
Copy code
nano /etc/systemd/system/flaresolverr.service
Paste the following configuration:
ini
Copy code
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
Save and exit the editor.
7. Enable and Start FlareSolverr Service
Enable the FlareSolverr service:
bash
Copy code
systemctl enable flaresolverr.service
Start the FlareSolverr service:
bash
Copy code
systemctl start flaresolverr.service
Check the status of the FlareSolverr service:
bash
Copy code
systemctl status flaresolverr.service
8. Reboot (Optional)
If everything is configured correctly, you can reboot the system for changes to take effect.

