# CCIE SP v5.1 lab

Configuration for a 25-node Containerlab topology built for CCIE Service Provider v5.1 study: two service providers and three customer autonomous systems. The lab is written up stage by stage at [craftednetworks.work](https://craftednetworks.work/).

**SP-A, AS 64501**, the legacy provider: OSPFv2 and OSPFv3, LDP, RSVP-TE. Seven CSR1000v 16.09.08 and one XRd Control-Plane 7.9.2 as route reflector.

**SP-B, AS 64502**, the modern provider: IS-IS Level-2, SR-MPLS, SR-TE, Flex-Algo. Three XRv9000 7.7.1, four CSR1000v and one XRd Control-Plane as route reflector.

**Customers**: CustA (AS 64510) and CustB (AS 64520) on IOL, CustC (AS 64530) on CSR1000v.

## What is here

- `topology.clab.yml`: the Containerlab topology, name `spv5-main`. Tag `topology-v2`.
- `topology.svg`, `topology.png`: the diagram, with link numbers matching `ipam.md`.
- `ipam.md`: IPv4 and IPv6 allocation for every node, link and loopback.
- `lab_configs/<node>/`: the IP-only baseline per node, hostname, Loopback0, data interfaces with /30 and /64, management VRF. No protocols; every stage adds its own on top.
- `stage_configs/`: per-stage protocol configuration, one folder per lab stage, tagged `lab01-s1a`, `lab01-s1b` and so on. See the README there for the folder list.
