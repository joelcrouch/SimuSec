# Home Lab Security Simulation with Orchestrating Linux VM

## Overview

This project is a home lab simulation of a multinational company to provide hands-on experience with security tools, attacks, and forensic analysis. The environment includes a central Linux VM orchestrating services such as a SIEM, vulnerable applications, and simulated endpoints. It enables you to study real-world security scenarios by deploying, attacking, and analyzing various systems.

---

## Features

- **Orchestrating Linux VM** to manage services and simulate a corporate network.
- **SIEM Setup** for log collection, monitoring, and incident analysis.
- **Vulnerable Applications** (e.g., OWASP Juice Shop, DVWA) to test exploits.
- **Log Collection** from endpoints and containers using Syslog or Beats.
- **Simulated Attacks** leveraging tools like Metasploit and Kali Linux utilities.
- **Forensic Analysis** with log investigation and network monitoring.

---

## System Requirements
I am just kinda making these up.  You could probably get away with fewere resources by using minified Linux versions.  Also the initial version will run on Linux, with a 'sudo' restricted user account, and internet disabled, such that taraffic will be modeled and ran and recorded using wireshark pcap, chron jobs, tcpreplay (for 'normal traffic').  All traffic will be via intranet,  between the VM's on the restricted user account.  
- **Host Machine**:
  - CPU: 4-8 cores.
  - RAM: 16+ GB.
  - Storage: 100+ GB.

- **Orchestrating Linux VM**:
  - CPU: 4-8 cores.
  - RAM: 8-16 GB.
  - Storage: 50+ GB.

- **Software**:
  - Hypervisor: VirtualBox, VMware, or KVM.
  - Docker and Docker Compose.
  - SIEM: Elastic Stack (ELK), Graylog, or Wazuh.

---

## Installation

### 1. Set Up the Orchestrating Linux VM
1. Download and install [Ubuntu Server](https://ubuntu.com/download/server) or another lightweight Linux distribution.
2. Install it as a VM.  Use KVM (linux) or hyper-v (Windows)  or something similar on IOS.
3. Update the VM:
   ```bash
   sudo apt update && sudo apt upgrade -y
   sudo apt install -y curl wget git vim build-essential


#############   Add reference to scripts for setting up the server, and getting the host machine better suited for use


## 4. Install Docker  

```bash 
sudo apt update
sudo apt install -y docker.io
sudo systemctl start docker
sudo systemctl enable docker


```

## 5.  Get Docker Compose

```
bash
sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose

```


## Set up the SIEM (ELASTIC STACK)

### what is Elastic Stack define it  

```
bash

wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
echo "deb https://artifacts.elastic.co/packages/8.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-8.x.list
sudo apt update
sudo apt install -y elasticsearch
sudo systemctl enable elasticsearch
sudo systemctl start elasticsearch

```

## Get Kibana 

```
bash 

sudo apt install -y kibana
sudo systemctl enable kibana
sudo systemctl start kibana


```

##  Get Logstash
```
bash 

sudo apt install -y logstash
sudo systemctl enable logstash
sudo systemctl start logstash


```

## Configure Beats (Optional):

Use Filebeat/Metricbeat to forward logs from endpoints:

```
bash

sudo apt install filebeat -y
sudo nano /etc/filebeat/filebeat.yml


```

## Deploy Vulnerable Stuff



OWASP JUICE SHOP
```
bash


docker run -d -p 3000:3000 bkimminich/juice-shop

```


DVWA
```
bash
docker run -d -p 80:80 vulnerables/web-dvwa

```


Metasploitable 2:

    Download the VM: Metasploitable 2.
    Run it in your hypervisor.



## SIMULATE ATTACK

Install Kali Linux tools on the orchestrating VM:
```
bash

sudo apt install -y kali-tools-top10


```
Examples of attacks:

    Phishing: Use Gophish.
    SQL Injection: Run sqlmap against DVWA.
    Lateral Movement: Use Metasploit or Impacket.



## Analyze Logs

    Access the Kibana dashboard at:
```
bash
http://<orchestrator_IP>:5601
```
Create dashboards to:

Monitor authentication attempts.
Detect anomalies in network traffic.
Investigate suspicious activities.


##  Respond to incidents:

Block IPs using ufw or iptables.
Take memory dumps for further analysis


NETWORK DIAGRAM

+------------------+
| Orchestrating VM |
| - Elastic Stack  |
| - Vulnerable Apps|
+------------------+
        |
+---------------------+
| Endpoints/Clients   |
| - Linux/Windows VMs |
+---------------------+
        |
+---------------------+
| Attack Tools        |
| - Kali Linux Tools  |
| - Metasploit        |
+---------------------+



Future work:

Deploy Wazuh for additional HIDS capabilities.
Add automated attack simulations with scripts.
Integrate network monitoring using Suricata or Zeek.
Maybe Dockerize the whole thing?

## Manageable Milestones to Track Progress: (these are wildly guesses)


Basic Setup (1-2 weeks)
    Set up web server, vulnerable app, attacker VM.
    Simulate basic user traffic.
    Set up basic logging (Suricata/Zeek).

Attack Simulation (2-3 weeks)
        Execute simple attacks (SQL injection, XSS).
        Monitor attack logs.
        Test the effect of attacks on the service.

Service Expansion (2-3 weeks)
        Add database server, employee workstations.
        Introduce more services and simulated user behavior.
        Set up network traffic replay.

Advanced Monitoring/Logging (3-4 weeks)
        Expand monitoring with a SIEM stack.
        Configure alerts and automated responses.
        Compare logs from different sources (attacker, employee, service).

Dockerization (Optional - 1-2 weeks)
        Containerize the experiment using Docker.
        Make the environment portable and easily replicable.  


Disclaimer

This project is intended for educational purposes only. Use it in a safe, controlled environment. Unauthorized attacks or testing on live networks is illegal.