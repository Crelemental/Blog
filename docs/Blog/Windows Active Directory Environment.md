**Install and run Proxmox Virtual Environment on a Intel NUC**
Setup a bootable thumb drive with an iso of the latest version of Proxmox. Booting into that drive launches the Proxmox graphical install interface. It goes through which drive you want it installed on, which storage file system you want, and setting up the login credentials. Once thats done, you can control the proxmox from the web interface its accessible from. 
![[Pasted image 20240128133311.png]]
![[Pasted image 20240128133342.png]]
![[Pasted image 20240128133406.png]]
![[Pasted image 20240128133425.png]]
![[Pasted image 20240128133441.png]]
![[Pasted image 20240128133450.png]]
![[Pasted image 20240128133459.png]]
![[Pasted image 20240128133506.png]]
After that, these Virtual machines can be transformed into templates to make rapid deployment of extra clone VMs easy to do.

**Installed Windows Server 2022 (graphical) as a VM in Proxmox**
	Installed the domain controller in Windows Server 2022
	![[Pasted image 20240128125327.png]]
	selected AD domain services 
	(the domain was renamed to dc1.local)
	![[Pasted image 20240128125353.png]]
	setup AD DS 
	![[Pasted image 20240128125441.png]]
	using SCONFIG, assign static IP and default gateway to the Domain controller, as well as making itself the DNS server. This is so that the domain controller knows its own name, and allows other hosts that want to connect to the domain to discover its domain name. 
	![[Pasted image 20240128125548.png]]
	Setting up the domain controller
	![[Pasted image 20240128125836.png]]
	Set up a basic user for the Windows 10 VM to join the domain
	![[Pasted image 20240128132048.png]]
	![[Pasted image 20240128132101.png]]
	![[Pasted image 20240128132116.png]]
	![[Pasted image 20240128132125.png]]
	Now the Windows 10 VM can join the domain with these credentials

**EDIT** If the Windows 10 server isn't able to join the domain and or access the internet, do this. 
	On the domain controller, go to DNS, choose the DNS server, and right click to properties
	![[Pasted image 20240312231237.png]]
	Next go to Forwarders and add the Google DNS servers as forwarders
	![[Pasted image 20240312231316.png]]
	This allows the domain controller to access the internet, and therefore any computer joined to that domain



**Installed Windows 10 as a VM in Proxmox**
	set DNS to the Domain controller's DNS 
	![[Pasted image 20240128131002.png]]
	![[Pasted image 20240128131040.png]]
	`Set-DNSClientServerAddress -InterfaceIndex 20 -ServerAddresses 192.168.50.155`
	**EDIT*** make sure that the only DNS server on the client computer is the DNS IP of the Domain Controller, otherwise it wont recognize the domain name. The client should get internet connectivity using the forwarders on the domain controller.
	connect to Domain 
	![[Pasted image 20240128130412.png]]
	![[Pasted image 20240128130421.png]]
	![[Pasted image 20240128130429.png]]
	(should be dc1.root for the domain name)
	![[Pasted image 20240128130437.png]]
	use the credentials I made to join the domain
	![[Pasted image 20240128132307.png]]
	Now part of the domain