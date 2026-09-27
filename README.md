# DHCP-Configuration---CCNA
No more manual IP typing. Today I configured the router to automatically hand out IP addresses to all PCs in both VLANs.


CCNA to CCNP Journey: Day 4 - DHCP Configuration! 🚀

No more manual IP typing. Today I configured the router to automatically hand out IP addresses to all PCs in both VLANs. 

🛠️ What this lab does:
- Creates two DHCP pools on the router (one for each VLAN).
- Excludes gateway IPs so they aren't given to PCs.
- Sets DNS server for all clients.
- Switches all PCs from Static to DHCP.
- Verifies that PCs get correct IPs and can still ping across VLANs.

💻 Commands used:
Router Configuration
Router> enable
Router# configure terminal

! Create VLAN 10 sub-interface (SALES)
Router(config)# interface g0/0.10
Router(config-subif)# encapsulation dot1Q 10
Router(config-subif)# ip address 192.168.10.1 255.255.255.0
Router(config-subif)# no shutdown
Router(config-subif)# exit

! Create VLAN 20 sub-interface (HR)
Router(config)# interface g0/0.20
Router(config-subif)# encapsulation dot1Q 20
Router(config-subif)# ip address 192.168.20.1 255.255.255.0
Router(config-subif)# no shutdown
Router(config-subif)# exit

! Enable physical interface
Router(config)# interface g0/0
Router(config-if)# no shutdown
Router(config-if)# exit

Exclude gateway IPs (.1 – .9)
bash

Router(config)# ip dhcp excluded-address 192.168.10.1 192.168.10.9
Router(config)# ip dhcp excluded-address 192.168.20.1 192.168.20.9


Create the DHCP pools

! SALES_POOL (VLAN 10)
Router(config)# ip dhcp pool SALES_POOL
Router(dhcp-config)# network 192.168.10.0 255.255.255.0
Router(dhcp-config)# default-router 192.168.10.1
Router(dhcp-config)# dns-server 8.8.8.8
Router(dhcp-config)# exit

! HR_POOL (VLAN 20)
Router(config)# ip dhcp pool HR_POOL
Router(dhcp-config)# network 192.168.20.0 255.255.255.0
Router(dhcp-config)# default-router 192.168.20.1
Router(dhcp-config)# dns-server 8.8.8.8
Router(dhcp-config)# exit

Switch Configuration -
Switch> enable
Switch# configure terminal

! Access ports
Switch(config)# vlan 10
Switch(config-vlan)# name SALES
Switch(config-vlan)# exit
Switch(config)# vlan 20
Switch(config-vlan)# name HR
Switch(config-vlan)# exit

Switch(config)# interface range fa0/1-2
Switch(config-if-range)# switchport mode access
Switch(config-if-range)# switchport access vlan 10
Switch(config-if-range)# exit

Switch(config)# interface fa0/3
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 20
Switch(config-if)# exit

! Trunk to router
Switch(config)# interface g0/1
Switch(config-if)# switchport mode trunk
Switch(config-if)# exit




Automation is the future of networking! 🧠💻

#ccna #ccnp #networking #packettracer #cisco #networkengineer #ittraining #techtok #learnnetworking #dhcp #networkautomation
