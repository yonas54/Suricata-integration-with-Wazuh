 Suricata + Wazuh Integration README
1. 🔧 Overview
Use this guide to integrate Suricata (NIDS/IPS) with Wazuh for centralized detection and alerting.

Wazuh: Manager or agent deployed accordingly (Suricata runs on agent hosts)
3. 🛠 Install Suricata
On Debian/Ubuntu:
sudo apt update
sudo apt install suricata -y
Let’s first check the status of Suricata,
sudo systemctl status suricata
Once the agent is successfully deployed, change the directory to 
sudo nano /var/ossec/etc and edit ossec.conf
In the suricata machine, download and extract the Emerging Threats Suricata ruleset
Now modify Suricata settings in the file and set the following variables
# Global stats configuration
stats:
enabled: yes
# Linux high speed capture support
af-packet:
- interface: enp0s3
- Now restart Suricata and wazuh-agent,
Wazuh automatically parses data from /var/log/suricata/eve.json and generates related alerts on the Wazuh dashboard.
