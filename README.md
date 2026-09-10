# Small Business Network

## Project Overview

This project is a small-business network designed and simulated using Cisco Packet Tracer.

The goal is to build a functional and secure network that supports multiple departments, wired and wireless devices, centralized network services, and external connectivity.

The project began as a basic two-LAN network and has gradually been expanded to include VLAN segmentation, inter-VLAN routing, DHCP, DNS, NAT/PAT, and Access Control Lists (ACLs).

[Packet Tracer Link](https://drive.google.com/file/d/16NBdSeYAxc-3l89d4LKqRURZH3oqxMwb/view?usp=drive_link)

The network currently separates users and resources into four VLANs based on their roles:

- Staff
- Guests
- Servers
- Management

---

## Network Topology

The network uses four VLANs with separate IPv4 subnets:

| VLAN | Name | Network | Default Gateway |
|------|------|---------|-----------------|
| 10 | STAFF | 192.168.10.0/24 | 192.168.10.1 |
| 20 | GUESTS | 192.168.20.0/24 | 192.168.20.1 |
| 30 | SERVERS | 192.168.30.0/24 | 192.168.30.1 |
| 40 | MANAGEMENT | 192.168.40.0/24 | 192.168.40.1 |

Router-on-a-stick is used to provide inter-VLAN routing.

The router is also connected to a simulated ISP through the `10.0.0.0/30` transit network.

- R1: `10.0.0.1/30`
- ISP Router: `10.0.0.2/30`

An external network is used to simulate communication beyond the organization's internal network.
![image alt](https://github.com/maryjane-ccc/small-business-network-packet-tracer/blob/68e2158e25f279c6a98500de8d97c9bd1b9af022/Network%20Topology.png)

---

## Initial Configuration

The initial network configuration included:

- Router interface configuration
- IPv4 addressing
- Default gateways
- DHCP configuration
- Wired client connectivity
- Wireless connectivity through an access point
- Router-to-ISP connectivity
- Connectivity testing using ICMP

DHCP is configured on R1 to automatically assign network information to client devices.

Reserved addresses are excluded from the DHCP pools so they can be used for gateways, servers, and other infrastructure devices.

---

## VLAN Segmentation

The network was segmented into four VLANs to separate devices based on their roles.

### VLAN 10 - Staff

Used for employee devices and normal business operations.

### VLAN 20 - Guests

Used for guest devices. Access to internal business resources is restricted using ACLs.

### VLAN 30 - Servers

Contains centralized network services such as the DNS and web server.

### VLAN 40 - Management

Used for administrative and network management devices.

Access ports on the switch were assigned to their appropriate VLANs.

An 802.1Q trunk connects the switch to R1 and carries traffic belonging to VLANs 10, 20, 30, and 40.

Router subinterfaces provide a default gateway for each VLAN and allow inter-VLAN routing.

---

## DNS Configuration

A DNS server was configured in the Server VLAN using the static address:

`192.168.30.2`

The server uses:

- IP Address: `192.168.30.2`
- Default Gateway: `192.168.30.1`
- DNS Server: `192.168.30.2`

An A record was created to map:

`www.mywebsite.com` → `192.168.30.2`

The DNS server address is distributed automatically to clients through the DHCP pools.

This allows users to access network services using domain names instead of remembering individual IP addresses.

---

## NAT/PAT Configuration

Network Address Translation was implemented to allow devices using private IPv4 addresses to communicate with the simulated external network.

The four VLAN subinterfaces were configured as NAT inside interfaces, while the ISP-facing interface was configured as the NAT outside interface.

PAT (Port Address Translation), also known as NAT overload, allows multiple internal devices to share the address of R1's external interface.

A standard ACL identifies the internal networks that are eligible for NAT:

- `192.168.10.0/24`
- `192.168.20.0/24`
- `192.168.30.0/24`
- `192.168.40.0/24`

A default route was also configured:

`0.0.0.0/0 → 10.0.0.2`

This directs traffic for unknown external destinations toward the simulated ISP.

---

## ACL Security

Extended Access Control Lists were implemented to restrict communication between VLANs based on the role of each network.

### Guest VLAN - ACL 100

Guest devices are allowed to:

- Access the Internet
- Use the DNS service on `192.168.30.2`
- Access the web server using HTTP/HTTPS
- Send ICMP traffic to the DNS/Web server

Guest devices are prevented from:

- Accessing the Staff VLAN
- Accessing the Management VLAN
- Freely accessing other resources within the Server VLAN

This provides Guest users with the services they require without giving them unrestricted access to internal business resources.

### Staff VLAN - ACL 110

Staff devices are allowed to:

- Access the Server VLAN
- Access the Internet

Staff devices are prevented from:

- Accessing the Guest VLAN
- Accessing the Management VLAN

### Management VLAN

The Management VLAN maintains broad access to the network for administrative purposes.

No inbound ACL is currently applied to the Management VLAN.

---

## Testing & Verification

The network was tested throughout the configuration process to verify that each service and security control behaved as expected.

### DHCP Testing

Client devices successfully obtained:

- IPv4 addresses
- Subnet masks
- Default gateways
- DNS server information

from their appropriate DHCP pools.

### Inter-VLAN Routing Testing

Connectivity tests were performed between VLANs to verify router-on-a-stick and 802.1Q trunking.

### DNS Testing

Clients successfully resolved:

`www.mywebsite.com`

to:

`192.168.30.2`

### NAT/PAT Testing

Internal devices successfully communicated with the simulated external network.

NAT operation was verified using:

`show ip nat translations`

and:

`show ip nat statistics`

### ACL Testing

Traffic was generated from Guest and Staff devices to verify permitted and denied communication.

Testing confirmed that:

- Guests can access approved DNS and web services.
- Guests can access the external network.
- Guests cannot access Staff or Management resources.
- Staff can access Server resources.
- Staff can access the external network.
- Staff cannot access Guest or Management resources.

ACL match counters were also used to confirm that traffic was being processed by the intended rules.

---

## Future Improvements

The next phase of the project will focus on network monitoring and additional security controls.

Planned improvements include:

- Centralized Syslog logging
- Network Time Protocol (NTP)
- Security event monitoring

---

## Skills Practiced

This project has provided practical experience with:

- IPv4 addressing and subnetting
- DHCP
- VLAN configuration
- 802.1Q trunking
- Router-on-a-stick
- Inter-VLAN routing
- DNS
- Static and default routing
- NAT/PAT
- Standard ACLs
- Extended ACLs
- Wildcard masks
- Protocol and port-based traffic filtering
- Network troubleshooting
- Cisco IOS CLI
- Cisco Packet Tracer

---

## Tools Used

- Cisco Packet Tracer
- Cisco IOS CLI
- GitHub for project documentation and version control

---

## Project Status

**In Progress**

Current implementation:

`VLANs → Inter-VLAN Routing → DHCP → DNS → NAT/PAT → ACL Security`

Next phase:

`Security Monitoring → Syslog/NTP → Network Hardening`
