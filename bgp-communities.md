-----------------------------------------------------
Peering with Hextet Systems

Hextet Systems is present at several exchanges. For an up-to-date
list of these exchanges, please see our PeeringDB record.
https://peeringdb.com/asn/395089

-----------------------------------------------------
Abuse issues should be reported to abuse@hextet.net
Abuse related e-mail to any other address(es)
will *not* be read
-----------------------------------------------------
Routing Policy

- All peering sessions are strictly filtered based on IRR data
- RPKI INVALID prefixes are rejected

-----------------------------------------------------
BGP Communities and Traffic Engineering
=======================================

The following BGP Community scheme draws inspiration from:
draft-ietf-grow-large-communities-usage
https://tools.ietf.org/html/draft-ietf-grow-large-communities-usage

INFORMATIONAL COMMUNITIES:
==========================
RFC 1997  | Large         | Meaning (Informational)
----------|---------------|-------------------------------------
-         | 395089:0:1    | self routes
-         | 395089:0:2    | downstream routes
-         | 395089:0:3    | ?? routes
-         | 395089:0:4    | ?? routes
-         | 395089:0:6    | transit routes
-         | 395089:0:8    | peering routes

----------+---------------+-------------------------------------
-         | 395089:5:1    | Accepted from peer because of valid IRR entry
-         | 395089:5:2    | Accepted from peer because of valid ROA
-         | 395089:5:4    | Accepted while RPKI invalid because it is added to our whitelist
----------+---------------+-------------------------------------
-         | 395089:6:10   | received from Les.net
----------+---------------+-------------------------------------
-         | 395089:8:n    | received via IXP with PeeringDB ID "n"
-         | 395089:8:703  | received via MBIX

| Rejection Reasons
----------+---------------+-------------------------------------
-         | 395089:7:1    | More specifics covering AS395089 space are considered hijacks
-         | 395089:7:2    | Somewhere in the AS_PATH a Bogon ASN is present (0, 23456, 64496..65534, 4200000000+)
-         | 395089:7:3    | The prefix is Bogon garbage (rfc1918, rfc4291 etc)
-         | 395089:7:4    | The prefix is an RPKI Invalid and as such rejected
-         | 395089:7:5    | The route's prefix length is unacceptable (too small or too large)
-         | 395089:7:6    | There is no IRR object that covers this route announcement
-         | 395089:7:7    | Hextet Systems member is not authorized to announce this route
----------+---------------+-------------------------------------

ACTION COMMUNITIES:
===================
RFC 1997  | Large         | Meaning (Action)
----------+---------------+-------------------------------------
-         | 395089:1:50   | Set LOCAL_PREF to 50 (default is 100)
-         | 395089:1:150  | Set LOCAL_PREF to 150 (default is 100)
-         | 395089:2:0    | Prepend 395089 once to all eBGP peers
-         | 395089:2:nnn  | Prepend 395089 once to peer AS nnn
-         | 395089:3:0    | Prepend 395089 twice to all eBGP peers
-         | 395089:3:nnn  | Prepend 395089 twice to peer AS nnn
-         | 395089:4:0    | Do not export to eBGP peers
-         | 395089:4:nnn  | Do not export to peer AS nnn
65535:0   | -             | G-SHUT [draft-ietf-grow-bgp-gshut]
65535:666 | -             | BLACKHOLE [RFC 7999]
----------+---------------+-------------------------------------

MEMBERS ONLY:
=============
RFC 1997  | Large        | Meaning (Action)
----------+--------------+-------------------------------------
-         | 395089:1:50  | Set LOCAL_PREF to 50 (default is 500)
| *** WARNING: default is 100 for other routes, so your prefixes may be become unreachable!
-         | 395089:1:450 | Set LOCAL_PREF to 450 (default is 500)
-         | 395089:1:550 | Set LOCAL_PREF to 550 (default is 500)
-         | 395089:301:0 | Do not export to other Hextet Systems members
-         | 395089:302:0 | Do not export to transit providers
-         | 395089:303:0 | Do not export to direct peers
----------+--------------+-------------------------------------

Examples:
---------
395089:2:3320   - prepend 395089 to AS 3320 (path on export will be 395089_395089_you)
395089:3:200023 - prepend 395089 to AS 200023 (path on export will be 395089_395089_395089_you)
395089:4:198203 - do not announce the route over any BGP sessions with AS 198203
(notice how you can 32-bit ASNs? :-)

