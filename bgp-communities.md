# Peering with Hextet Systems

Hextet Systems is present at several exchanges. For an up-to-date list of these exchanges, please see our PeeringDB record:

https://peeringdb.com/asn/395089

---

## Reporting Abuse

Abuse issues should be reported to:

**abuse@hextet.net**

Abuse-related e-mail sent to any other address **will not be read**.

---

## Routing Policy

- All peering sessions are strictly filtered based on IRR data
- RPKI **INVALID** prefixes are rejected

---

# BGP Communities and Traffic Engineering

The following BGP community scheme draws inspiration from:

- https://datatracker.ietf.org/doc/html/draft-ietf-grow-large-communities-usage

---

## Informational Communities

| RFC 1997 | Large Community | Meaning (Informational) |
|---------:|-----------------|--------------------------|
| – | `395089:0:1` | Self routes |
| – | `395089:0:2` | Downstream routes |
| – | `395089:0:3` | ?? routes |
| – | `395089:0:4` | ?? routes |
| – | `395089:0:6` | Transit routes |
| – | `395089:0:8` | Peering routes |
| – | `395089:5:1` | Accepted from peer because of valid IRR entry |
| – | `395089:5:2` | Accepted from peer because of valid ROA |
| – | `395089:5:4` | Accepted while RPKI invalid due to whitelist |
| – | `395089:6:10` | Received from Les.net |
| – | `395089:8:n` | Received via IXP with PeeringDB ID `n` |
| – | `395089:8:703` | Received via MBIX |

---

## Rejection Reasons

| RFC 1997 | Large Community | Reason |
|---------:|-----------------|--------|
| – | `395089:7:1` | More specifics covering AS395089 space are considered hijacks |
| – | `395089:7:2` | AS_PATH contains a Bogon ASN (0, 23456, 64496–65534, 4200000000+) |
| – | `395089:7:3` | Prefix is Bogon space (RFC1918, RFC4291, etc.) |
| – | `395089:7:4` | Prefix is RPKI INVALID |
| – | `395089:7:5` | Prefix length is unacceptable (too small or too large) |
| – | `395089:7:6` | No IRR object covers this route |
| – | `395089:7:7` | Hextet Systems member not authorized to announce this route |

---

## Action Communities

| RFC 1997 | Large Community | Action |
|---------:|-----------------|--------|
| – | `395089:1:50` | Set LOCAL_PREF to **50** (default: 100) |
| – | `395089:1:150` | Set LOCAL_PREF to **150** (default: 100) |
| – | `395089:2:0` | Prepend AS395089 once to **all** eBGP peers |
| – | `395089:2:nnn` | Prepend AS395089 once to peer AS `nnn` |
| – | `395089:3:0` | Prepend AS395089 twice to **all** eBGP peers |
| – | `395089:3:nnn` | Prepend AS395089 twice to peer AS `nnn` |
| – | `395089:4:0` | Do **not** export to any eBGP peers |
| – | `395089:4:nnn` | Do **not** export to peer AS `nnn` |
| – | `65535:0` | G-SHUT (draft-ietf-grow-bgp-gshut) |
| – | `65535:666` | BLACKHOLE (RFC 7999) |

---

## Members-Only Communities

> ⚠️ These communities apply **only** to Hextet Systems members.

| RFC 1997 | Large Community | Action |
|---------:|-----------------|--------|
| – | `395089:1:50` | Set LOCAL_PREF to **50** (default: 500)<br>**WARNING:** Non-member routes default to 100, so reachability may be impacted |
| – | `395089:1:450` | Set LOCAL_PREF to **450** (default: 500) |
| – | `395089:1:550` | Set LOCAL_PREF to **550** (default: 500) |
| – | `395089:301:0` | Do not export to other Hextet Systems members |
| – | `395089:302:0` | Do not export to transit providers |
| – | `395089:303:0` | Do not export to direct peers |

---

## Examples

```text
395089:2:3320
→ prepend 395089 to AS 3320
→ exported path: 395089 395089 YOU

395089:3:200023
→ prepend 395089 twice to AS 200023
→ exported path: 395089 395089 395089 YOU

395089:4:198203
→ do not announce the route to AS 198203
→ (yes, 32-bit ASNs are supported)
