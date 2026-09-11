# Stage configs

Per-stage protocol configuration for the CCIE SPv5.1 two-provider lab, one folder
per lab stage. These changes add only the protocol logic for
that stage and the next layer of the configuration and assume the matching baseline from `lab_configs/` is already on the node. 

Each folder holds one `.ios` file per node, and where the companion article has
a diagram, that too. Most folders cover one provider; a stage applied to both
carries all sixteen nodes.

The `.ios` extension is deliberate: it is what the Cisco IOS syntax-highlighting
extensions for VS Code key off, so these files colour correctly in the editor
rather than rendering as plain text. The contents are ordinary configuration -
paste them into a router as they are.

| Folder | Lab / stage | Topic | Article |
|---|---|---|---|
| `lab01-s1a-spa-ospf/` | Lab 01, Stage 1A | SP-A dual-stack multi-area OSPFv2 + OSPFv3 | SP-A IGP Foundation |
| `lab01-s1b-spb-isis/` | Lab 01, Stage 1B | SP-B dual-stack single-level IS-IS, Multi-Topology | SP-B IGP Foundation |
| `lab01-s1c-igp-opt/` | Lab 01, Stage 1C | Prefix suppression, overload bit, LSP MTU, hello padding - both providers | IGP Optimization |
| `lab01-s2a-spa-ldp/` | Lab 01, Stage 2A | SP-A LDP transport: per-node label ranges, host-route allocation filtering, OSPF-LDP sync, session protection, MD5 auth, OSPF per-prefix LFA | SP-A MPLS Transport |
| `lab01-s2b-spb-sr/` | Lab 01, Stage 2B | SP-B SR-MPLS transport: SRGB, prefix-SIDs by index, TI-LFA on XR, classic LFA on XE | SP-B MPLS Transport |

## One line to read before pasting

`lab01-s1c-igp-opt/B-P1.ios` and `B-PE1.ios` carry `lsp-mtu 128`. That is a
deliberate teaching setting for observing IS-IS LSP fragmentation, and it is
**reverted in Stage 2B before Segment Routing goes on** - at 128 bytes the
Router-CAP TLV carrying the SRGB and the Prefix-SID sub-TLV do not reliably
survive LSP re-origination. Keep it only as long as you are looking at
fragments.

`lab01-s2b-spb-sr/B-P1.ios` and `B-PE1.ios` carry `no lsp-mtu`, which is that
revert. It is a negation, so it is the one line in this folder that does not
appear in a running config - what you see on the box afterwards is the absence of
any `lsp-mtu` line, back at the interface default. Every other line in the folder
was verified against the running configuration.

`lab01-s2a-spa-ldp/A-RR.ios` is the only file here that is not a paste-alike of
its neighbours. IOS-XR reserves the label space below 16000, so the per-node scheme
that gives the XE routers 1100-1899 gives A-RR **16700-16799**. The range takes an
optional label-table index - `mpls label range 16700 16799` is accepted as written
and `show run mpls` renders it back as `mpls label range table 0 16700 16799`.
A-RR's LDP password uses the default `neighbor` block, which is XR's equivalent of
the `mpls ldp password fallback` the seven XE nodes carry. The router stores that
password encrypted; `password clear LDP_AUTH` is the form you type.

Topology and addressing for every node are in `topology.clab.yml` and `ipam.md`
at the root of this repo.
