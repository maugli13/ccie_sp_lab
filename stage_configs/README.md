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
| `lab02-s4-spa-multicast/` | Lab 02, Stage 4 | 6 | SP-A IP multicast core: PIM sparse mode in the global table on the P router links and the PE uplinks, an anycast RP at 10.1.0.100 on A-P1 and A-P3 with MSDP between them, a static mroute on each PE that keeps the RPF lookup for the other PE's Loopback0 off its RSVP-TE tunnel | SP-A IP Multicast Core |
| `lab02-s5-spa-mvpn-p0/` | Lab 02, Stage 5 | 5 | SP-A draft-Rosen mVPN Profile 0 for the CustA VRF: a GRE default MDT on 239.1.10.1 through the anycast RP, an SSM data MDT pool 232.1.10.0/24 with a 1 kbps threshold, the BGP IPv4 MDT address family reflected by the IOS-XR route reflector, customer Auto-RP with the candidate RP on CustA-HQ and the mapping agent on CustA-Br1 | SP-A mVPN Profile 0 |
| `lab03-s1-spb-flexalgo/` | Lab 03, Stage 1 | 3 | SP-B Flex-Algo 128 on the three XRv9k routers: a definition that excludes the CSR-EXIT affinity, advertised by all three with B-P1 at priority 200, an algorithm-128 prefix-SID on each Loopback0, the CSR-EXIT affinity on the PE links to the CSR routers, and IS-IS metric 100 on the two links through B-P1 | SP-B Flex-Algo |
| `lab03-s2-spb-srte/` | Lab 03, Stage 2 | 3 | SP-B SR-TE with on-demand next-hop: BGP-LS from both PEs to B-RR as the SR-PCE, a route-policy that adds colour 128 to CustC's VPNv4 and VPNv6 routes sent to B-RR, an ODN template for colour 128 computed by the PCE over algorithm-128 prefix-SIDs, and an explicit colour-200 policy with one segment on each PE | SP-B SR-TE |
| `lab03-s3-interas-opta/` | Lab 03, Stage 3 | 2 | Inter-AS L3VPN Option A between A-ASBR and B-ASBR: a CustC VRF on each ASBR with its own provider's RD and RT, the dot1Q sub-interface Gi4.30 in the VRF, dual-stack eBGP in the VRF between the ASBRs, and an EVC service instance on Gi4 that switches the CSR's virtual NIC into promiscuous mode so the sub-interfaces receive their tagged frames | Inter-AS Option A |
| `lab03-s4-interas-optb/` | Lab 03, Stage 4 | 5 | Inter-AS L3VPN Option B between A-ASBR and B-ASBR for CustA: the dot1Q sub-interface Gi4.255 in the global table, `no bgp default route-target filter`, route-maps that rewrite CustA's RT on the routes from the other ASBR and send it only CustA's routes, one IPv4 eBGP session that carries VPNv4 and VPNv6, `next-hop-self` toward the route reflector, and the same OSPF domain ID on the three CustA PEs | Inter-AS Option B |
| `lab03-s5a-spa-mvpn-p11/` | Lab 03, Stage 5A | 5 | SP-A mVPN Profile 11 for the CustA VRF: BGP auto-discovery with an SSM default MDT on 232.0.10.1, customer joins carried as BGP C-multicast routes, the BGP IPv4 MVPN address family in place of IPv4 MDT, reflected by the IOS-XR route reflector, and a static customer RP on the CEs and in the VRF on both PEs | SP-A mVPN Profile 11 |

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

`lab02-s4-spa-multicast/` holds the four P routers and the two PEs; A-RR and A-ASBR take no configuration in this stage. A-P3 differs from A-P1 only in the Loopback1 description and the MSDP peer, A-P4 is the same as A-P2, and A-PE2 differs from A-PE1 only in the static mroute. `tunnels-rpf` (SVG and PNG) is the article's diagram.

`lab02-s5-spa-mvpn-p0/` holds the two PEs, A-RR and the two CustA CEs. A-PE2 is the same as A-PE1, and CustA-Br1 differs from CustA-HQ only in the last line. The `mdt default`, `mdt data` and `address-family ipv4 mdt` lines are not in the running configuration, because a later stage moved CustA to an mLDP default MDT with the BGP `ipv4 mvpn` address family. They were checked against this stage's captures instead. `profile0-mdt` and `data-mdt-flow` (SVG and PNG) are the article's diagrams.

`lab03-s1-spb-flexalgo/` holds B-P1, B-PE1 and B-PE2. The CSR routers, B-RR and B-ASBR take no configuration in this stage. `spb-metrics` and `flexalgo-paths` (SVG and PNG) are the article's diagrams.

`lab03-s2-spb-srte/` holds B-PE1, B-PE2 and B-RR. B-P1, the CSR routers and B-ASBR take no configuration in this stage. B-PE2 differs from B-PE1 only in the explicit policy, which points to B-PE1 with label 16550, and in the PCC source address. The running configuration indents the `128` under `extcommunity-set opaque FLEX128` by two spaces. `srte-odn-path` (SVG and PNG) is the article's diagram.

`lab03-s3-interas-opta/` holds A-ASBR and B-ASBR. The PEs, P routers and route reflectors take no configuration in this stage; `as-override` on A-PE2 is in `lab01-s5-edge-l3vpn/`. B-ASBR differs from A-ASBR only in the description, the AS numbers, the RD and RT, and the addresses. `opta-path` (SVG and PNG) is the article's diagram.

`lab03-s4-interas-optb/` holds A-ASBR, B-ASBR, A-PE1, A-PE2 and B-PE1. The P routers and route reflectors take no configuration in this stage, and the service instance on Gi4 is in `lab03-s3-interas-opta/`. B-ASBR differs from A-ASBR only in the addresses, the AS numbers, the RTs, and the order of the two extcommunity lists, and A-PE2 is the same as A-PE1. The running configuration shows `address-family vpnv4` and `address-family vpnv6` without `unicast`, and IOS-XE adds `mpls bgp forwarding` to Gi4.255 when the VPN eBGP session comes up. `optb-path` (SVG and PNG) is the article's diagram.

`lab03-s5a-spa-mvpn-p11/` holds the two PEs, A-RR and the two CustA CEs, as changes to `lab02-s5-spa-mvpn-p0/`. A-PE2 is the same as A-PE1, and CustA-Br1 is the same as CustA-HQ. The `no` lines remove the Profile 0 default MDT group and the IPv4 MDT address family, and the Profile 0 `mdt data` line stays configured. `mdt auto-discovery pim` and `mdt default 232.0.10.1` are not in the running configuration, because the next stage moved CustA to an mLDP default MDT. They were checked against this stage's captures instead. `default-mdt-type1`, `customer-joins-bgp` and `data-mdt-type3` (SVG and PNG) are the article's diagrams.

Topology and addressing for every node are in `topology.clab.yml` and `ipam.md` at the root of this repo.
