# Global Logistics Network — CCNA Capstone Project

A 5-site enterprise network designed and simulated in Cisco Packet Tracer, implementing VLAN segmentation, EIGRP dynamic routing, DHCP/DNS services, and Access Control Lists for a global logistics company.

---

## Technologies Used
- Cisco Packet Tracer
- Cisco IOS CLI
- EIGRP (AS 100)
- VLANs & Router-on-a-Stick (802.1Q)
- DHCP / DNS
- Extended ACLs

---

## Network Topology

```
         [HQ - R1]
        /          \
  [R5-DC]          [R2-BrA]
      |                |
  [R4-Whse]        [R3-BrB]
```

5 sites connected via **serial WAN links** with EIGRP dynamic routing (AS 100).

---

## Sites & VLANs

| Site | VLAN | Name | Subnet |
|------|------|------|--------|
| HQ | 10 | Admin | 192.168.10.0/24 |
| HQ | 20 | Finance | 192.168.20.0/24 |
| Branch A | 30 | Sales | 192.168.30.0/24 |
| Branch A | 40 | Support | 192.168.40.0/24 |
| Branch B | 50 | Marketing | 192.168.50.0/24 |
| Branch B | 60 | Dev | 192.168.60.0/24 |
| Warehouse | 70 | Inventory | 192.168.70.0/24 |
| Warehouse | 80 | Shipping | 192.168.80.0/24 |
| Data Center | 90 | Servers | 192.168.90.0/24 |
| Data Center | 99 | Management | 192.168.99.0/24 |

---

## WAN Links (Serial)

| Link | Subnet |
|------|--------|
| R1 ↔ R2 | 10.0.0.0/30 |
| R2 ↔ R3 | 10.0.0.4/30 |
| R3 ↔ R4 | 10.0.0.8/30 |
| R4 ↔ R5 | 10.0.0.12/30 |
| R5 ↔ R1 | 10.0.0.16/30 |

---

## Key Configurations

### Inter-VLAN Routing (Router-on-a-Stick)
- 802.1Q subinterfaces on each router for each VLAN
- Each subinterface assigned gateway IP (x.x.x.1)

### EIGRP Dynamic Routing
- AS Number: **100**
- Auto-summary: **disabled**
- All site networks and WAN subnets advertised

### DHCP
- Each router acts as DHCP server for its local VLANs
- First 10 IPs excluded per pool
- DNS server pointer: **192.168.90.10** (Data Center)

### DNS
- Centralized DNS server in Data Center (static IP: 192.168.90.10)
- DNS record: `www.gln.com → 192.168.90.10`
- Accessible from all sites except restricted Warehouse Inventory VLAN

### ACL Security
- Extended ACL `BLOCK-Inventory` applied on R5-DC
- **Blocks**: `192.168.70.0/24` (Warehouse Inventory) from accessing `192.168.90.0/24` (Data Center)
- **Permits**: all other traffic

---

## Testing & Verification

| Test | Result |
|------|--------|
| Admin → Warehouse Shipping ping | ✅ Success (0% loss) |
| Inventory PC → DNS Server ping | ❌ Blocked by ACL (100% loss) |
| Branch PC → DNS Server ping | ✅ Success |
| EIGRP routing table (all routes) | ✅ All 10 VLANs + WAN links present |
| VLAN verification (show vlan brief) | ✅ All VLANs active on correct ports |
| DNS resolution (www.gln.com) | ✅ Accessible via web browser |

---

## Project Structure
```
global-logistics-network/
│
├── GlobalLogisticsNetwork.pkt     # Packet Tracer file
├── README.md
└── docs/
    └── project_report.pdf         # Full project report
```

---

*CCNA Capstone Project — Creativa Innovation Hub (TIEC / ITIDA) | 2026*
