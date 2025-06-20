 Suricata + Wazuh Integration README
1. 🔧 Overview
Use this guide to integrate Suricata (NIDS/IPS) with Wazuh for centralized detection and alerting. Suricata analyzes network traffic and logs events to eve.json, while Wazuh ingests these logs for SIEM capabilities.2. 🖥 Environment
Suricata host: Linux endpoint (e.g., Ubuntu 22.04/24.04 or kali-linux)

Wazuh: Manager or agent deployed accordingly (Suricata runs on agent hosts)
3. 🛠 Install Suricata
On Debian/Ubuntu:
sudo apt update
sudo apt install suricata -y
Let’s first check the status of Suricata,
sudo systemctl status suricata
Once the agent is successfully deployed, change the directory to 
sudo nano /var/ossec/etc and edit ossec.conf
<ossec_config>
  <localfile>
    <log_format>json</log_format>
    <location>/var/log/suricata/eve.json</location>
  </localfile>
</ossec_config>
In the suricata machine, download and extract the Emerging Threats Suricata ruleset
cd /tmp/ && curl -LO https://rules.emergingthreats.net/open/suricata-6.0.8/emerging.rules.tar.gz
sudo tar -xvzf emerging.rules.tar.gz && mkdir /etc/suricata/rules && sudo mv rules/*.rules /etc/suricata/rules/
sudo chmod 640 /etc/suricata/rules/*.rules
Now modify Suricata settings in the file and set the following variables
sudo nano /etc/suricata/suricata.yaml 
HOME_NET: "<UBUNTU_IP>"
EXTERNAL_NET: "any"
default-rule-path: /etc/suricata/rules
rule-files:
- "*.rules"
# Global stats configuration
stats:
enabled: yes
# Linux high speed capture support
af-packet:
- interface: enp0s3
- Now restart Suricata and wazuh-agent,
systemctl restart suricata
systemctl restart wazuh-agent
Wazuh automatically parses data from /var/log/suricata/eve.json and generates related alerts on the Wazuh dashboard.
