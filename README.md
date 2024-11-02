# NETWORKING-CONFIGURATION
DHCP Configuration 
To configure DHCP in your network setup, here’s a detailed guide with commands for each device. This configuration assumes you’re using Cisco devices, but you can adapt it if necessary.
My Topology includes 3 Routers, 4 switches, 12 desktop and printer(client) 1 DHCP server to assign IP address to the clients.
Step-by-Step Configuration
ROUTER 1 which will serve as the ISP which will be hosting IP address of 100.100.100.0 and 200.200.200.0 from the two location which are Lagos and Portharcourt respectively.
configuration of Router 1 
step by step procees : CONFIGURING THE TWO ROUTERS PORT CONNECTED TO THE ISP
Router>en
Router#config t
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#no ip domain lookup                    
Router(config)#hostname ISP
ISP(config)#int s0/3/0
ISP(config-if)#ip address 100.100.100.1 255.255.255.252
ISP(config-if)#no sh

%LINK-5-CHANGED: Interface Serial0/3/0, changed state to down
ISP(config-if)#exit
ISP(config)#int s0/3/1
ISP(config-if)#ip address 200.200.200.1 255.255.255.252
ISP(config-if)#no sh

%LINK-5-CHANGED: Interface Serial0/3/1, changed state to down
ISP(config-if)#exit
ISP(config)#int gig0/0
ISP(config-if)#ip address 192.168.10.129 255.255.255.224
ISP(config-if)#no sh

Configuration of RIP of the two routers which are lagos and Portharcourt respectively.
ISP>EN
ISP#config t
Enter configuration commands, one per line.  End with CNTL/Z.
ISP(config)#router rip
ISP(config-router)#version 2
ISP(config-router)#network 100.100.100.0
ISP(config-router)#network 200.200.200.0
ISP(config-router)#network 192.168.10.128
ISP(config-router)#no auto-summary
ISP(config-router)#do wr
Building configuration...
[OK]
ISP(config-router)#exit


CONFIGURATION OF LAGOS ROUTER
STEP BY STEP

Configuring the port to the ISP:
Router>en
Router#config t
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#no ip domain lookup
Router(config)#hostname LAGOS
LAGOS(config)#int s0/3/0
LAGOS(config-if)#ip address 100.100.100.2 255.255.255.252
LAGOS(config-if)#no sh
LAGOS(config-if)#
%LINK-5-CHANGED: Interface Serial0/3/0, changed state to up
%LINEPROTO-5-UPDOWN: Line protocol on Interface Serial0/3/0, changed state to up
LAGOS(config-if)#exit

CONFIGURING SW1 IP ADDRESS: 
LAGOS(config)#int gig0/0
LAGOS(config-if)#ip address 192.168.10.1 255.255.255.224
LAGOS(config-if)#no sh
LAGOS(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0, changed state to up
%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0, changed state to up
LAGOS(config-if)#exit
CONFIGURING SW2 IP ADDRESS:
LAGOS(config)#int gig 0/1
LAGOS(config-if)#ip address 192.168.10.33 255.255.255.224
LAGOS(config-if)#no sh

Configuration of portharcourt router:
configuring of PH port to the ISP.


Router>en
Router#config t
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#no ip domain lookup
Router(config)#hostname PH
PH(config)#int s0/3/0
PH(config-if)#ip address 200.200.200.2 255.255.255.252
PH(config-if)#no sh

Configuring of the Switches connected to PH router.
SW1:
PH(config)#int gig0/0
PH(config-if)#ip address 192.168.10.65 255.255.255.224
PH(config-if)#no sh
PH(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0, changed state to up
%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0, changed state to up
PH(config-if)#exit
SW2:
PH(config)#int gig 0/1
PH(config-if)#ip address 192.168.10.97 255.255.255.224
PH(config-if)#no sh
PH(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/1, changed state to up

CONFIGURATION OF ROUTER RIP:
ROUTER RIP INTO LAGOS ROUTER:

LAGOS>en
LAGOS#config t
Enter configuration commands, one per line.  End with CNTL/Z.
LAGOS(config)#router rip
LAGOS(config-router)#version 2
LAGOS(config-router)#network 192.168.10.0
LAGOS(config-router)#network 192.168.10.32
LAGOS(config-router)#network 100.100.100.0
LAGOS(config-router)#no auto-summary
LAGOS(config-router)#exit

CONFIGURATION OF ROUTER RIP:
ROUTER RIP INTO PH ROUTER:

PH>en
PH#config t
Enter configuration commands, one per line.  End with CNTL/Z.
PH(config)#router rip
PH(config-router)#version 2
PH(config-router)#network 200.200.200.0
PH(config-router)#network 192.168.10.64
PH(config-router)#network 192.168.10.96
PH(config-router)#no auto-summary
PH(config-router)#exit
PH(config)#do wr


CONFIGURATION OF DHCP SERVER 
ip dhcp pool NETWORK_1
   network 192.168.10.0 255.255.255.224
   default-gateway 192.168.10.1
   dns-server 192.168.10.130

ip dhcp pool NETWORK_2
   network 192.168.10.32 255.255.255.224
   default-gateway 192.168.10.33
   dns-server 192.168.10.130

ip dhcp pool NETWORK_3
   network 192.168.10.64 255.255.255.224
   default-gateway 192.168.10.65
   dns-server 192.168.10.130

   ip dhcp pool NETWORK_4
   network 192.168.10.96 255.255.255.224
   default-gateway 192.168.10.97
   dns-server 192.168.10.130

   DHCP SERVER POOL 
   network 192.168.10.128
   default-gateway 192.168.10.129
   dns-server 192.168.10.130

configuring the ip helper commands;
we will configure it on all routers

lagos router:
LAGOS(config)#int gig0/0
LAGOS(config-if)#ip helper-address 192.168.10.130
LAGOS(config-if)#do wr
Building configuration...
[OK]
LAGOS(config-if)#exit
LAGOS(config)#int gig 0/1
LAGOS(config-if)#ip helper-address 192.168.10.130
LAGOS(config-if)#exit
LAGOS(config)#do wr
Building configuration...
[OK]
LAGOS(config)#

Portharcourt router;
PH(config)#int gig 0/0
PH(config-if)#ip helper-address 192.168.10.130
PH(config-if)#exit
PH(config)#int gig 0/1
PH(config-if)#ip helper-address 192.168.10.130
PH(config-if)#exit 
PH(config)#do wr
Building configuration...
[OK]

ISP router:
ISP(config)#int gig0/0
ISP(config-if)#ip helper-address 192.168.10.130
ISP(config-if)#do wr
Building configuration...
[OK]
ABJ(config-if)#exit
ABJ(config)#


Then we can check all the laptops connected the the switch and switch to the DHCP on the configuration settings and we are good to go.











