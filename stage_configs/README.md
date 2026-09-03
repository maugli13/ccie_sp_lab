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

## One line to read before pasting

`lab01-s1c-igp-opt/B-P1.ios` and `B-PE1.ios` carry `lsp-mtu 128`. That is a
deliberate teaching setting for observing IS-IS LSP fragmentation, and it is
**reverted in Stage 2B before Segment Routing goes on** - at 128 bytes the
Router-CAP TLV carrying the SRGB and the Prefix-SID sub-TLV do not reliably
survive LSP re-origination. Keep it only as long as you are looking at
fragments.

Topology and addressing for every node are in `topology.clab.yml` and `ipam.md`
at the root of this repo.
