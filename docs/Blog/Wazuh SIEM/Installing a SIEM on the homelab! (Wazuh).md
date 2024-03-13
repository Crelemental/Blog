Configuring Wazuh, a leading Security Information and Event Management (SIEM) system, in a homelab environment is driven by my imperative to strengthen cybersecurity defenses while gaining hands-on experience with advanced security monitoring and management tools. The core objective is to develop a comprehensive understanding of how to detect, analyze, and respond to security threats in real time, using Wazuh's robust capabilities for log analysis, intrusion detection, and compliance management. This endeavor not only enhances my personal knowledge and skills in cybersecurity but also prepares me to implement and manage enterprise-grade security solutions in a professional setting. 
### Installing Wazuh all-in-one
https://documentation.wazuh.com/current/installation-guide/wazuh-indexer/step-by-step.html
We will start by installing curl 
![[Pasted image 20240224122505.png]]

![[Pasted image 20240224122834.png]]

set a static IP
![[Pasted image 20240224123633.png]]

static IP
![[Pasted image 20240224123618.png]]

`sudo nano config.yml` --  set config to have this machine's ip as the nodes
![[Pasted image 20240224123721.png]]

takes the config.yml and creates it into a installer
![[Pasted image 20240224123852.png]]

Installs wazuh, using -a makes it an all-in-one install rather than being a cluster
![[Pasted image 20240224124037.png]]

I can now access the dashboard at https://192.168.50.159:443 using the credentials given after installation
![[Pasted image 20240224131501.png]]

### Adding agents

Setting up the Wazuh agent on a Windows 10 VM (which is part of a Active Directory Domain).
1st option - GUI
![[Pasted image 20240224132845.png]]
Manager IP: 192.168.50.159
Auth key is generated to encrypt the traffic from the agent to the Wazuh server
![[Pasted image 20240224133238.png]]
Now we have an active agent added to the Wazuh dashboard

2nd option - CLI (being set up on the Windows Server 2022 Domain Controller)
![[Pasted image 20240224133924.png]]

And now we have the Windows Server showing up as an active agent 
![[Pasted image 20240224133954.png]]
(using the CLI would allow for mass deployment of Wazuh agents across a network using GPOs or something similar within a domain)

**EDIT** To start the Wazuh server up after a restart or shutdown, use:
`systemctl start wazuh-indexer`
`systemctl start wazuh-dashboard`