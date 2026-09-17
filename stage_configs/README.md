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

Every line in every folder was checked against the running configuration of the lab, with the exceptions below.

## Notes per folder

`lab01-s1c-igp-opt/B-P1.ios` and `B-PE1.ios` set `lsp-mtu 128` to make IS-IS LSP fragmentation visible. At 128 bytes the Router-CAP TLV carrying the SRGB and the Prefix-SID sub-TLV do not reliably survive LSP re-origination, so `lab01-s2b-spb-sr/B-P1.ios` and `B-PE1.ios` revert it with `no lsp-mtu` before Segment Routing goes on. `no lsp-mtu` is a negation and does not show in a running configuration.

`lab01-s2a-spa-ldp/A-RR.ios` uses the label range 16700-16799 where the XE nodes use 1100-1899, because IOS-XR reserves the labels below 16000. `mpls label range 16700 16799` is accepted as typed and reads back as `mpls label range table 0 16700 16799`. The LDP password sits under the default `neighbor` block, the XR equivalent of `mpls ldp password fallback` on XE; `password clear LDP_AUTH` is the form you type and the router stores it encrypted.

`lab01-s3-bgp/`: the SP-A BGP password on the live lab was changed after this stage. The four `BGP_AUTH` lines on A-RR, A-PE1, A-PE2 and A-ASBR carry the value the stage was built with.

Topology and addressing for every node are in `topology.clab.yml` and `ipam.md` at the root of this repo.
