# BGP Communities & Traffic Engineering

## Informational Communities

RFC 1997  | Large         | Meaning (Informational)
----------|---------------|-------------------------------------
65089:1   | 395089:0:1    | self routes
65089:2   | 395089:0:2    | downstream routes
65089:6   | 395089:0:6    | transit routes
65089:8   | 395089:0:8    | peering routes
----------|---------------|-------------------------------------
65089:101 | 395089:5:1    | Accepted from peer because of valid IRR entry
65089:102 | 395089:5:2    | Accepted from peer because of valid ROA
65089:104 | 395089:5:4    | Accepted while RPKI invalid because it is added to our whitelist
----------|---------------|-------------------------------------
65089:10  | 395089:6:10   | received from Les.net
65089:11  | 395089:6:11   | received from Voyageur
----------|---------------|-------------------------------------
none      | 395089:8:703  | received via MBIX

### Rejection Reasons

RFC 1997  | Large         | Meaning (Informational)
----------|---------------|-------------------------------------
none      | 395089:7:1    | More specifics covering AS395089 space are considered hijacks
none      | 395089:7:2    | Somewhere in the AS_PATH a Bogon ASN is present (0, 23456, 64496..65534, 4200000000|)
none      | 395089:7:3    | The prefix is Bogon garbage (rfc1918, rfc4291 etc)
none      | 395089:7:4    | The prefix is an RPKI Invalid and as such rejected
none      | 395089:7:5    | The route's prefix length is unacceptable (too small or too large)
none      | 395089:7:6    | There is no IRR object that covers this route announcement
none      | 395089:7:7    | Coloclue member is not authorized to announce this route

## Action Communities

RFC 1997  | Large         | Meaning (Action)
----------|---------------|-------------------------------------
65089:50  | 395089:1:50   | Set LOCAL_PREF to 50 (default is 100)
65089:150 | 395089:1:150  | Set LOCAL_PREF to 150 (default is 100)
65089:202 | 395089:2:0    | Prepend 395089 once to all eBGP peers
none      | 395089:2:nnn  | Prepend 395089 once to peer AS nnn
65089:203 | 395089:3:0    | Prepend 395089 twice to all eBGP peers
none      | 395089:3:nnn  | Prepend 395089 twice to peer AS nnn
65089:204 | 395089:4:0    | Do not export to eBGP peers
none      | 395089:4:nnn  | Do not export to peer AS nnn
65535:0   | -             | G-SHUT [draft-ietf-grow-bgp-gshut]
65535:666 | -             | BLACKHOLE [RFC 7999]

### Customers Only

RFC 1997  | Large         | Meaning (Action)
----------|---------------|-------------------------------------
65089:50  | 395089:1:50   | Set LOCAL_PREF to 50 (default is 500)
| *** WARNING: default is 100 for other routes, so your prefixes may be become unreachable!
65089:450 | 395089:1:450  | Set LOCAL_PREF to 450 (default is 500)
65089:550 | 395089:1:550  | Set LOCAL_PREF to 550 (default is 500)
65089:301 | 395089:301:0  | Do not export to other Coloclue members
65089:302 | 395089:302:0  | Do not export to transit providers
65089:303 | 395089:303:0  | Do not export to direct peers

## Examples

* 395089:2:3320   - prepend 395089 to AS 3320 (path on export will be 395089_395089_you)
* 395089:3:200023 - prepend 395089 to AS 200023 (path on export will be 395089_395089_395089_you)
* 395089:4:198203 - do not announce the route over any BGP sessions with AS 198203
* (notice how you can 32-bit ASNs? :-)
