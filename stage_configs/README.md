# Stage configs

Per-stage protocol configuration for the CCIE SPv5.1 two-provider lab, one folder
per lab stage. These changes add only the protocol logic for
that stage and the next layer of the configuration and assume the matching baseline from `lab_configs/` is already on the node. 

Each folder holds one `.ios` file per node, plus the diagram used in the
companion article.

The `.ios` extension is deliberate: it is what the Cisco IOS syntax-highlighting
extensions for VS Code key off, so these files colour correctly in the editor
rather than rendering as plain text. The contents are ordinary configuration -
paste them into a router as they are.

| Folder | Lab / stage | Topic | Article |
|---|---|---|---|
| `lab01-s1a-spa-ospf/` | Lab 01, Stage 1A | SP-A dual-stack multi-area OSPFv2 + OSPFv3 | SP-A IGP Foundation |
| `lab01-s1b-spb-isis/` | Lab 01, Stage 1B | SP-B dual-stack single-level IS-IS, Multi-Topology | SP-B IGP Foundation |

Topology and addressing for every node are in `topology.clab.yml` and `ipam.md`
at the root of this repo.
