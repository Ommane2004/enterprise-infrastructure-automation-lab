# Incident Timeline — INC-001

## Incident

**Incident ID:** INC-001  
**Title:** VyOS `eth1` Interface Failure  
**Environment:** Enterprise Infrastructure Automation Lab  
**Affected network:** `10.10.10.0/24` — Internal LAN  
**Affected interface:** `eth1` — `10.10.10.1/24`  
**Incident type:** Controlled infrastructure failure / routing impact  
**Severity:** Lab-controlled simulation

---

## Timeline

### 1. Baseline validation

The VyOS baseline was captured before failure injection.

`eth1` was operational:

```text
eth1  10.10.10.1/24  u/u
