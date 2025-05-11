# Secure-Network-Design-Using-Cisco-Packet-Tracer
Creating a secure network plan for a campus by configuring switches, routers and servers in Cisco Packet Tracer

## Network Topology
* Campus has different buildings with several departments in it
* A star topology has been implemented in this design with a top-down approach
* There is one main core router from which all connections are made.
* This design can be expanded in the future if necessary

![image](https://github.com/Sadhvi19/Secure-Network-Design-Using-Cisco-Packet-Tracer/assets/53933893/5c60ed52-9ef7-444d-9ac4-1ac8d299c82d)

## Design Layout
* Core Router sends/receives packets from other routers.
* The core router divides into sub-routers for each department.
* End devices like PC’s, laptops and mobile phones are all connected to a switch 
* There is an Email-server for each department.
* Each department has Access-Point to connect to the campus Wi-Fi

## Security Features
* Software firewalls
* ACL - Access control Lists
* SSH Configuration
* Switch Security
* VPN-Tunneling with IPsec
* RFID Authentication (Library)

## Securing a Router
Configurations of the Router

* Assign a Hostname to the Router
```
Router> enable
Router> configuration terminal
Router(config)# hostname “   “
```
*	Prevent attempts to resolve mistyped domain name
```
Router(config)# no ip domain-lookup
```

*	Display banner for unauthorized access to router
```
Router(config)# banner motd @ Unauthorised Access Not Allowed @
```
*	Assign console password, at least 10 characters in length and ensure console sessions close after 7 minutes.
```
Router(config)# security passwords min-length 10
Router(config)# line console 0
Router(config)# exec-timeout 7 0
Router(config)# password routerpassword 
Router(config)# login
```
*	Enable remote login(ssh) and enable vty password
```
Router(config)# line vty  0 4
Router(config)# exec-timeout 7 0
Router(config)# password routerpassword
Router(config)# login
Router(config)# transport input ssh
```
*	ssh configuration: 
```
Router(config)# ip domain-name.org
Router(config)# crypto key generate rsa
How many bits in modulus [512]: 1024
Router(config)# user admin secret routerpassword
Router(config)# aaa new-model
```
*	Assign password for privileged mode 
```
Router(config)# enable secret routerpassword
```
*	Encrypt all passwords
```
Router(config)# service password-encryption
```
*	Impede brute force attacks by blocking login attempts for 45 seconds if a user attempts 3 times in 100 seconds
```
Router(config)# login block-for 45 attempts 3 within 100
```

## Part2: Securing a switch
Configurations for a switch
*	Assign a Hostname to the Switch
```
Switch>enable 
Switch>config terminal
Switch(config)#hostname “ “
```
*	Display banner for unauthorized access to router
```
banner motd @ Unauthorised Access Not Allowed @
```
*	Assign console password, at least 10 characters in length and ensure console sessions close after 7 minutes.
```
Switch(config)#line console 0
Switch(config)#exec-timeout 7 0
Switch(config)#password switchpassword
Switch(config)#login
```
*	Assign vty password
```
Switch(config)#line vty 0 4
Switch(config)#exec-timeout 7 0
Switch(config)#password switchpassword
Switch(config)#login
```
*	Assign password for privileged mode 
```
Switch(config)#enable secret switchpassword
```
*	Encrypt all passwords
```
Switch(config)#service password-encryption
```
*	Impede brute force attacks by blocking login attempts for 45 seconds if a user attempts 3 times in 100 seconds
```
Switch(config)#login block-for 45 attempts 3 within 100
```

## Part3: Switch Security
3.1. Configure port security
3.1. DHCP Snooping 
3.2. Dynamic ARP inspection
3.4. Enable Port Fast and BPDU Guard
3.5 . Disable CDP

### 3.1. DHCP Snooping
*	Select the switch “       “. Go to config  mode and enable DHCP snooping .
```
Switch (c0nfig)#ip dhcp snooping
Switch (c0nfig)#ip dhcp snooping vlan 1
Switch (c0nfig)#no ip dhcp snooping information option
```
* The port connected to the router must be configured as trusted port, rest of the ports should be configured with a limit rate of 15
```
Switch (c0nfig)#interface fastEthernet 0/1
Switch (c0nfig-if)#ip dhcp snooping trust
Switch (c0nfig)#Interface range fastEthernet 0/2-7
Switch (c0nfig-if)#ip dhcp snooping limit rate 15
```
### 3.2. Dynamic ARP inspection
*	Enable arp inspection on vlan 1
```
Switch (c0nfig)#ip arp inspection vlan 1
```
*	Port connected to router must be configured as trusted
```
Switch (c0nfig)#interface fastEthernet 0/1
Switch (c0nfig-if)#ip arp inspection trust
```
### 3.4. Portfast and BPDU guard
*	Enable Portfast and BPDU guard on access ports
```
Switch (c0nfig)#int range f0/2-7
Switch(config-if-range)#spanning-tree portfast
Switch(config-if-range)#spanning-tree bpduguard enable

