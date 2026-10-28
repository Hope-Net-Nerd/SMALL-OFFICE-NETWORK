# Small Office Network Lab (Cisco Packet Tracer)

A small office network with three VLANs, a gateway router and a simulated ISP. Download the `.pkt` file, plug in your own devices, and test it.

**Packet Tracer version:** _8.2.2.0400_



## What it covers

- VLANs and trunking
- Inter-VLAN routing (router-on-a-stick)
- DHCP on the gateway
- Access lists to control guest access
- PAT (NAT overload)
- OSPF and a static default route
- SSH remote access

## Network layout

| VLAN | Name | Subnet | Gateway | Addresses |
|---|---|---|---|---|
| 10 | Staff | 192.168.10.0/24 | 192.168.10.1 | DHCP |
| 20 | Guests | 192.168.20.0/24 | 192.168.20.1 | DHCP |
| 30 | Servers | 192.168.30.0/24 | 192.168.30.1 | Static |

| Device | Interface | IP address |
|---|---|---|
| GATEWAY | f0/1 (to ISP) | 200.100.0.11/24 |
| ISP | f0/0 (to gateway) | 200.100.0.12/24 |
| ISP | loopback0 (acts as "the internet") | 8.8.8.8/32 |
| Web/DNS server | NIC | 192.168.30.9/24 |

## Switch ports (SWI-1)

| Port | VLAN | Use |
|---|---|---|
| f0/1 | Trunk | Link to GATEWAY |
| f0/2 | 10 Staff | Staff device |
| f0/4 | 20 Guests | Guest devices |
| f0/3 | 30 Server | Server |
| f0/5 to f0/24, g0/1 to g0/2 | Shutdown | Unused |

## How it works

- **DHCP:** the gateway hands out addresses to Staff and Guests.
- **Guests:** can use the internet and open the company website (`http://soho.com`), but cannot ping or reach Staff or the Servers VLAN.
- **Staff:** can reach everything, including the servers and Guests.
- **NAT:** Staff and Guests use PAT with their own address pool, so many devices can share a few public addresses.
- **Routing:** OSPF runs between the gateway and the ISP.
- **Management:** SSH only (Telnet is disabled).

## Try it yourself

1. Open the `.pkt` file in Packet Tracer.
2. Add a PC and connect it to a free switch port.
3. Set its IP configuration to **DHCP**.
4. Run the tests below.

To open more ports on SWI-1:



## Expected test results

| From | Test | Result |
|---|---|---|
| Staff PC | ping 192.168.30.9 | Works |
| Staff PC | ping 8.8.8.8 | Works |
| Staff PC | ping a Guest PC | Works |
| Guest PC | ping 8.8.8.8 | Works |
| Guest PC | open http://soho.com | Works |
| Guest PC | ping 192.168.30.9 | Fails |
| Guest PC | ping a Staff PC | Fails |

Useful commands on GATEWAY: `show ip nat translations`, `show access-lists`, `show ip route`, `show ip ospf neighbor`.
Useful commands on SWI-1: `show vlan brief`, `show interfaces trunk`.

## Screenshots

Add yours to the `docs/` folder:

- `docs/guest-ping-internet.png`
- `docs/guest-website.png`
- `docs/guest-ping-blocked.png`
- `docs/nat-translations.png`

## Lab passwords

| Device | Console / Enable | SSH |
|---|---|---|
| GATEWAY | `gateway1234` | `admin` / `admin1234` |
| SWI-1 | `cisco1234` | none |
| ISP | `isp1234` | none |


## Ideas to extend

- Port security on access ports
- A second switch, or a Layer 3 switch
- Redundant gateway (HSRP)
- Static NAT to publish the web server
- i am also looking forward to learn about ip phones and letting them to communicate over a WAN

## What I learned

I have learned to configure the NAT/PAT,
