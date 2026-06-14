# 🔀 Inter VLAN Routing – Cisco Packet Tracer
This project demonstrates Inter-VLAN Routing using the Router-on-a-Stick method in Cisco Packet Tracer.

# 📌 Project Overview
A Cisco switch was configured with multiple VLANs, and a Cisco router was used to route traffic between the VLANs through a single trunk link. Devices in different VLANs were able to communicate successfully after configuring VLANs, trunking, subinterfaces, and gateway IP addresses.

# What I Did in This Project
- Create multiple VLANs on a switch.
- Assign switch ports to specific VLANs.
- Configure a trunk link between the switch and router.
- Configure Router-on-a-Stick using router subinterfaces.
- Enable communication between different VLANs.
- Verify connectivity using ping tests.

#  Network Topology
```text
PC1 (VLAN 10) ----\
                   \
PC2 (VLAN 10) ----- Switch ---- Router (G0/0)
                   /
PC3 (VLAN 20) ----/
                  \
PC4 (VLAN 20) -----/

Switch Fa0/24 <---- Trunk Link ----> Router G0/0
```

![Network Topology](Screenshots/topology.png)

If the connection between the switch & router shows **RED TRIANGLE** then it means that connection is not working both on physical and configuration level. Follow these steps to fix it.

**1. Check the cable:-**
- Correct cable is **Copper Straight-Through**.
- Don't use serial, console, crossover cable.
- Crossover cable sometimes gives issue.

**2. Check the correct ports:-**
- Switch (fastethernet0/24) <--------> Router (gigabitethernet0/0)

**3. Turn on the Router Ports (Very Imp):-**
- By default router port is turned off
- Click on Router
- Go to CLI tab
- Press Enter
- **Router>** Prompt will shown
- Router>**enable**
- Router#**Configure terminal**
- Router(config)#**interface gigabitethernet0/0**
- Router(config-if)#**no shutdown**
- This will turn on the gigabitethernet0/0 port

**4. Turn on the Switch Port:-**
- Most of the time as soon as you turn ON the router port, the GREEN TRIANGLE LIGHT will be shown for both the switch & router
- If not, then follow these commands:-
- Click on Switch
- Go to CLI tab
- Press Enter
- **Switch>** Prompt will shown
- Switch>**enable**
- Switch#**Configure terminal**
- Switch(config)#**interface fastethernet0/24**
- Switch(config-if)#**no shutdown**

In this way you can turn on the green light.

# VLAN & IP Address Assignment
| Device | Department | VLAN | IP Address | Subnet Mask | Switch Port |
|--------|------------|------|--------------|-----------|--------------|
| PC0 | Sales | VLAN 10 | 192.168.10.2 | 255.255.255.0 | Fa0/1 |
| PC1 | Sales | VLAN 10 | 192.168.10.3 | 255.255.255.0 | Fa0/2 |
| PC2 | HR | VLAN 20 | 192.168.20.2 | 255.255.255.0 | Fa0/3 |
| PC3 | HR | VLAN 20 | 192.168.20.3 | 255.255.255.0 | Fa0/4 |

# Default Gateways
| VLAN | Department | Network Address | Default Gateway |
|------|------------|----------------|----------------|
| VLAN 10 | Sales | 192.168.10.0/24 | 192.168.10.1 |
| VLAN 20 | HR | 192.168.20.0/24 | 192.168.20.1 |

# Devices Used
- Cisco 2960 Switch
- Cisco 1941 Router
- 4 PCs
- Copper Straight-Through Cables
- Cisco Packet Tracer


# Switch Configuration
**1. Create VLANs**
- Click on Switch
- Go to CLI tab
- Press Enter
- **Switch>** Prompt will shown
- Switch>**enable**
- Switch#**Configure terminal**
- Enter configuration commands, one per line. End with CNTL/Z.
- Switch(config)#**vlan 10**
- Switch(config-vlan)#**name SALES**
- Switch(config-vlan)#**exit**
- Switch(config)#**vlan 20**
- Switch(config-vlan)#**name HR**
- Switch(config-vlan)#**end (1 time) OR Control+Z OR exit (2 times) All three works. Ultimate Goal is to come back to Switch# prompt**
- press Enter
- Switch#**show vlan brief**
- **Output looks something like this**

| VLAN | Name | Status | Ports |
|------|------|--------|-------|
| 10 | Sales | active | |
| 20 | HR | active | |


**2. Assign the Ports**
- Click on Switch
- Go to CLI tab
- Press Enter
- **Switch>** Prompt will shown
- Switch>**enable**
- Switch#**Configure terminal**

- Switch(config)#**interface fastethernet0/1**
- Switch(config-if-range)#**switchport mode access**
- Switch(config-if-range)#**switchport access vlan 10**
- Switch(config-if-range)#**exit**

- Switch(config)#**interface fastethernet0/2**
- Switch(config-if-range)#**switchport mode access**
- Switch(config-if-range)#**switchport access vlan 10**
- Switch(config-if-range)#**exit**

- Switch(config)#**interface fastethernet0/3**
- Switch(config-if-range)#**switchport mode access**
- Switch(config-if-range)#**switchport access vlan 20**
- Switch(config-if-range)#**exit**

- Switch(config)#**interface fastethernet0/4**
- Switch(config-if-range)#**switchport mode access**
- Switch(config-if-range)#**switchport access vlan 20**
- Switch(config-if-range)#**end (1 time) OR Control+Z OR exit (2 times) All three works. Ultimate Goal is to come back to Switch# prompt**
- Press Enter
- Switch#**show vlan brief**
- **Output looks something like this**

| VLAN | Name | Status | Ports |
|------|------|--------|-------|
| 10 | Sales | active | fa0/1, fa0/2 |
| 20 | HR | active | fa0/3,fa0/4 |

**3. Trunk Port (Most Important)**

The trunk port is needed because a single switch port needs to carry traffic from multiple VLANs at the same time.
- Switch>**enable**
- Switch#**Configure terminal**
- Switch(config)#**interface fastethernet0/24**
- Switch(config-if-range)#**switchport mode trunk**
- Switch(config-if-range)#

**4. Verify Switch Trunk Port**
- Switch#**show interfaces trunk**

| Port | Mode | Encapsulation | Status | Native vlan |
|------|------|---------------|--------|-------------|
| fa0/24 | on | 802.1q | trunking | 1 |

This shows that connection between switch & router are configured properly.

# Router Configuration (Router-on-a-stick)
There is only one cable between the switch & a router. How router will understand that the packets belongs to which VLAN? Thats why we will create subinterfaces.

**1. Enable Physical Interface**
- Router>**enable**
- Router#**Configure terminal**
- Router(config)#**interface gigabitethernet0/0**
- Router(config-if)#**no shutdown**
- Router(config-if)#**exit**
- Router(config)#

**2. Configure VLAN 10 Subinterface**
- Router(config)#**interface gigabitethernet0/0.10**
- Router(config-subif)#**encapsulation dot1Q 10**
- Router(config-subif)#**ip address 192.168.10.1 255.255.255.0**
- Router(config-subif)#**exit**

**3. Configure VLAN 20 Subinterface**
- Router(config)#**interface gigabitethernet0/0.20**
- Router(config-subif)#**encapsulation dot1Q 20**
- Router(config-subif)#**ip address 192.168.20.1 255.255.255.0**
- Router(config-subif)#**end (1 time) OR Control+Z OR exit (2 times) All three works. Ultimate Goal is to come back to Router# prompt**
- Press Enter
- Router#

# Verification Commands

**1. Switch**
- Switch#**show vlan brief**

| VLAN | Name | Status | Ports |
|------|------|--------|-------|
| 10 | Sales | active | fa0/1, fa0/2 |
| 20 | HR | active | fa0/3,fa0/4 |

- Switch#**show interfaces trunk**

| Port | Mode | Encapsulation | Status | Native vlan |
|------|------|---------------|--------|-------------|
| fa0/24 | on | 802.1q | trunking | 1 |

**2. Router**
- Router>**show ip interface brief**

![Network Topology](Screenshots/Router-IP-Interface-Status-Check.png)

- Router>**show ip route**

![Network Topology](Screenshots/Router-Connected-Network-Check.png)

- Router>**show running-config**

![Network Topology](Screenshots/Router-on-a-Stick-Configuration-(VLAN-10-&-VLAN-20-Subinterfaces).png)

# Connectivity Testing

**1. Same VLAN Test**
- **PC0** ping **PC1**

![Network Topology](Screenshots/PC0-Same-VLAN-Ping-Test.png)

**1. Inter-VLAN Test**
- **PC2** ping **PC0 and PC1**

![Network Topology](Screenshots/PC2-Different-VLAN-Ping-Test.png)

Successful ping responses confirmed that routing between VLAN 10 and VLAN 20 was working correctly

# Key Concepts Learned
- VLAN Creation
- VLAN Port Assignment
- Access Ports
- Trunk Ports
- 802.1Q VLAN Tagging
- Router-on-a-Stick
- Subinterface Configuration
- IP Addressing
- Default Gateway Configuration
- Inter-VLAN Routing
- Network Verification and Troubleshooting

# Troubleshooting Performed
- Verified VLAN assignments using **show vlan brief**.
- Verified trunk configuration using **show interfaces trunk**.
- Verified subinterface status using **show ip interface brief**.
- Verified routing table using **show ip route**.
- Tested communication using ping commands.
- Checked router and switch interface status.

# Project Outcome
Successfully implemented Inter-VLAN Routing using the Router-on-a-Stick method.
- Created VLAN 10 and VLAN 20 on a Cisco switch.
- Configured access ports and trunk ports.
- Configured router subinterfaces using 802.1Q encapsulation.
- Assigned gateway IP addresses for each VLAN.
- Enabled communication between different VLANs.
- Verified connectivity using ping and routing verification commands.

The project successfully demonstrated communication between devices located in different VLANs through a single router interface.

# Skills Demonstrated
- VLAN Configuration
- Switch Port Assignment
- Trunk Port Configuration
- Router-on-a-Stick Configuration
- 802.1Q VLAN Tagging
- Inter-VLAN Routing
- IP Addressing and Subnetting
- Default Gateway Configuration
- Cisco Router Configuration
- Cisco Switch Configuration
- Network Troubleshooting
- Connectivity Testing
- Cisco Packet Tracer
- TCP/IP Networking

## Thank You

Thank you for reviewing this project.

Feedback and suggestions are always welcome.

## Author

Created by Nirmal Chandra Kunal.

This project demonstrates Inter-VLAN Routing using VLANs, trunking, and Router-on-a-Stick in Cisco Packet Tracer.

GitHub: https://github.com/nckunal

LinkedIn: https://linkedin.com/in/nckunal007
