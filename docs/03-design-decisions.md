# Design Decisions

This document records the major architectural decisions made while building the enterprise infrastructure lab.

The objective is not only to describe the final architecture, but to explain the reasoning behind the selected topology, segmentation, management model, monitoring architecture, and automation approach.

## Decision 1 — Separate Network Segments

### Decision

Use separate virtual networks for:

- WAN / upstream connectivity
- pfSense ↔ VyOS transit
- Internal client systems
- Server / management systems
- Security testing

### Reason

Separating networks creates clearer routing boundaries and allows individual segments to be tested, monitored, and troubleshot independently.

### Result

The lab provides a structure closer to an enterprise environment than placing every VM on a single flat network.
