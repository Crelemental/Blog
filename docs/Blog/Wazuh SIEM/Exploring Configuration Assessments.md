On the Wazuh dashboard, im able to see the latest scans of different security benchmarks against my agents
![[Pasted image 20240224135218.png]]
Exploring further into this reveals a score of 37%... I'm going to work to get this score higher and harden my Windows Server 2022 VM.

![[Pasted image 20240224135338.png]]
Looking into a failed check reveals a complex description of what the rationale is behind the failure and how to remediate it. 

Going into the Directory Controller, we can navigate to the GPO editor
![[Pasted image 20240224140158.png]]
![[Pasted image 20240224140138.png]]

After making this change, and restarting the Wazuh agent, the benchmark is re-ran.
![[Pasted image 20240224141030.png]]
We can now see that the GPO policy is configured correctly to pass the benchmark.

### Automating hardening
##### **BEFORE DOING THIS, PROCEED WITH CAUTION**

Created by a user "eneerge", there is a powershell script that automates hardening for CIS benchmarks. https://github.com/eneerge/CIS-Windows-Server-2022

I will run this script and see how much better the VM fares in its security posture.
![[Pasted image 20240224142314.png]]
We now have a score of 92%, which is a ton better than the 37% we had before. 
Obviously we will still have some failures due to services that need to be running on this Domain Controller, such as RDP. To further narrow down the failures and get even better hardening, a risk assessment must be done on the network to determine which services are critical to business function and really fine tuning what needs to be left open.

**EDIT** after running this script, I got hard-locked out of the domain controller due to the new local admin account login not being a valid login type.  kinda annoying cause I had to re-do everything from an early clone of the VM :(
![[Pasted image 20240312232548.png]]
It creates a new local admin named: User
For some reason even after setting the new password for User and trying to log in, that type of login isn't supported according to the server
![[Pasted image 20240312232935.png]]
Technically I can comment out the renaming of old accounts AND
![[Pasted image 20240312233039.png]]
comment out the disabling of Administrator accounts

I would try that, but I already went through a whole reset twice (I know) and had to reset both VMs (server and the 1 joined to the domain)