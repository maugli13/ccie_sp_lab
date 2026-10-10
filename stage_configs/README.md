# Stage configs

One folder per lab stage, one `.ios` file per node, plus the diagram (SVG and PNG), where the companion article has one. Each file holds only the protocol configuration that stage adds; the IP-only baseline from `lab_configs/` is assumed to be on the node already.

The `.ios` extension is what the Cisco IOS syntax-highlighting extensions for VS Code key off. The contents are plain configuration.

| Folder | Lab / stage | Nodes | Topic | Article |
|---|---|---|---|---|
| `lab01-s1a-spa-ospf/` | Lab 01, Stage 1A | 8 | SP-A dual-stack multi-area OSPFv2 + OSPFv3 | SP-A IGP Foundation |
| `lab01-s1b-spb-isis/` | Lab 01, Stage 1B | 8 | SP-B dual-stack single-level IS-IS, Multi-Topology | SP-B IGP Foundation |
| `lab01-s1c-igp-opt/` | Lab 01, Stage 1C | 16 | Prefix suppression, overload bit, LSP MTU, hello padding, both providers | IGP Optimization |
| `lab01-s2a-spa-ldp/` | Lab 01, Stage 2A | 8 | SP-A LDP transport: per-node label ranges, host-route allocation filtering, OSPF-LDP sync, session protection, MD5 auth, OSPF per-prefix LFA | SP-A MPLS Transport |
| `lab01-s2b-spb-sr/` | Lab 01, Stage 2B | 8 | SP-B SR-MPLS transport: SRGB, prefix-SIDs by index, TI-LFA on XR, classic LFA on XE | SP-B MPLS Transport |
| `lab01-s3-bgp/` | Lab 01, Stage 3 | 8 | iBGP VPNv4 + VPNv6 on both providers: one route reflector per AS, BGP-free core, TCP MD5 auth | BGP Control Plane |
| `lab01-s4-convergence/` | Lab 01, Stage 4 | 14 | BFD on every IGP adjacency on both providers, per protocol and per address family; IPv6 TI-LFA on the IOS-XR nodes | Convergence |
| `lab01-s5-edge-l3vpn/` | Lab 01, Stage 5 | 10 | Dual-stack intra-AS L3VPN on both providers: per-PE RDs, per-provider RTs, PE-CE OSPFv2 + OSPFv3 on IOS-XE, PE-CE eBGP with `as-override` on IOS-XR, two orphan sites | Intra-AS L3VPN |
| `lab02-s1-spa-rsvp-te/` | Lab 02, Stage 1 | 6 | SP-A inter-area RSVP-TE: loose-hop explicit paths through the ABRs, `autoroute destination`, a tunnel steered onto the A-P1 to A-P3 diagonal, FRR with NHOP and NNHOP backups on A-P1 | Inter-Area MPLS-TE |
| `lab02-s2-spa-sr-ldp/` | Lab 02, Stage 2 | 5 | SP-A SR/LDP interworking: SR-MPLS over OSPF on A-PE1, A-P1 and A-P2 with an SRGB of 17000-17999, `sr-label-preferred` on A-PE1 only, a mapping server on A-RR for the LDP-only loopbacks, a second loopback on A-PE2 for the SR-to-LDP swap | SP-A SR/LDP Interworking |
| `lab02-s3a-spa-vpws/` | Lab 02, Stage 3A | 4 | SP-A VPWS E-Line: an LDP-signalled pseudowire, VC 2011, between CustB-Hub and CustB-Spk1, attached to an 802.1Q service instance for VLAN 11 on Gi5 of A-PE1 and A-PE2, with the control word on | SP-A VPWS |
| `lab02-s3b-spa-evpn/` | Lab 02, Stage 3B | 5 | SP-A single-homed EVPN-ELAN: EVI 12 on an 802.1Q service instance for VLAN 12 on Gi5 of A-PE1 and A-PE2, bridge domain 12, route distinguisher and route target derived by the PEs, ingress replication, the `l2vpn evpn` address family reflected by the IOS-XR route reflector | SP-A EVPN-ELAN |

Every line in every folder was checked against the running configuration of the lab, with the exceptions below.

## Notes per folder

`lab01-s1c-igp-opt/B-P1.ios` and `B-PE1.ios` set `lsp-mtu 128` to make IS-IS LSP fragmentation visible. At 128 bytes the Router-CAP TLV carrying the SRGB and the Prefix-SID sub-TLV do not reliably survive LSP re-origination, so `lab01-s2b-spb-sr/B-P1.ios` and `B-PE1.ios` revert it with `no lsp-mtu` before Segment Routing goes on. `no lsp-mtu` is a negation and does not show in a running configuration.

`lab01-s2a-spa-ldp/A-RR.ios` uses the label range 16700-16799 where the XE nodes use 1100-1899, because IOS-XR reserves the labels below 16000. `mpls label range 16700 16799` is accepted as typed and reads back as `mpls label range table 0 16700 16799`. The LDP password sits under the default `neighbor` block, the XR equivalent of `mpls ldp password fallback` on XE; `password clear LDP_AUTH` is the form you type and the router stores it encrypted.

`lab01-s3-bgp/`: the SP-A BGP password on the live lab was changed after this stage. The four `BGP_AUTH` lines on A-RR, A-PE1, A-PE2 and A-ASBR carry the value the stage was built with.

`lab01-s4-convergence/` holds 14 of the 16 nodes. A-RR and B-RR run the XRd control-plane image and take no configuration in this stage, so they have no file. IS-IS binds BFD per topology on IOS-XE: the four SP-B XE nodes carry `bfd all-interfaces` under `router isis` for the IPv4 topology and again under `address-family ipv6` for the IPv6 one.

`lab01-s5-edge-l3vpn/` holds the four PEs and six CEs; no P router or route reflector takes configuration in this stage. On B-PE1 and B-PE2 the IPv4 PE-CE neighbor sits under `address-family ipv4 unicast`. The live lab shows `address-family ipv4 labeled-unicast` there, because the Carrier Supporting Carrier stage in Lab 04 changed it. `vrf-planes.svg` and `vrf-planes.png` are the article's diagram.

`lab02-s1-spa-rsvp-te/` holds the four P routers and the two PEs; A-RR and A-ASBR take no configuration in this stage. The explicit paths are written as typed. The running configuration reads them back as `ip explicit-path name <name> enable` with an `index` on each `next-address`, and all ten paths match hop for hop. The four PE tunnels that set `bandwidth` also show `tunnel mpls traffic-eng priority 7 7`, the default setup and hold priority, in the running configuration. On the live lab, Tunnel10's description on A-PE1 reads `TE to A-PE2: NORTH primary / SOUTH secondary`, without the space before the colon. `tunnel10-paths` and `frr-backups` (SVG and PNG) are the article's diagrams.

`lab02-s2-spa-sr-ldp/` holds A-PE1, A-P1, A-P2, A-RR and A-PE2. A-P3, A-P4 and A-ASBR stay LDP-only and take no configuration in this stage. On A-PE2, the running configuration reads the Loopback1 IPv6 address back in upper case, `2001:DB8:1::106/128`. `sr-ldp-swap` (SVG and PNG) is the article's diagram.

`lab02-s3a-spa-vpws/` holds A-PE1, A-PE2, CustB-Hub and CustB-Spk1. The `no ipv6 address` lines remove the baseline PE-CE addresses from Gi5 and `Ethernet0/1`, and a negation does not show in a running configuration. On the live lab, CustB-Hub's `Ethernet0/1.11` has no description. `pw-control-word` and `pw-path` (SVG and PNG) are the article's diagrams.

`lab02-s3b-spa-evpn/` holds A-PE1, A-PE2, A-RR, CustB-Hub and CustB-Spk1. The configuration on A-PE2 is the same as on A-PE1. `evpn-path` (SVG and PNG) is the article's diagram.

Topology and addressing for every node are in `topology.clab.yml` and `ipam.md` at the root of this repo.
