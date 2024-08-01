**Port**: UDP 161 (Control Commands), UDP 162 (Traps from Server to Client)
**Config file**: `/etc/snmp/snmpd.conf`
### MIB
Management Information Base do not contain data, but they explain where to find which information and what it looks like, which returns values for the specific OID, or which data type is used. MIB files are written in the `Abstract Syntax Notation One` (`ASN.1`) based ASCII text format.


## SNMPv1
No authentication, no encryption

## SNMPv2
Uses [[#Community Strings]]. But this is transmitted only in plain-text, so no in-built security.
### Community Strings
- These are like passwords to determine authorization
- Bound to an IP address
- Often CS are named as hostname of that IP. Use [[crunch]] to create custom wordlists
- Sent in plain-text across the network.
- Once we get CS, use [[braa]] to bruteforce.
## SNMPv3
Has authentication and encryption (uses [[PSK]])

## Dangerous Settings
| **Settings**                                     | **Description**                                                                       |
| ------------------------------------------------ | ------------------------------------------------------------------------------------- |
| `rwuser noauth`                                  | Provides access to the full OID tree without authentication.                          |
| `rwcommunity <community string> <IPv4 address>`  | Provides access to the full OID tree regardless of where the requests were sent from. |
| `rwcommunity6 <community string> <IPv6 address>` | Same access as with `rwcommunity` with the difference of using IPv6.                  |

### Tools 
1. [[snmpwalk]]: query the OIDs with their information.
2. [[onesixtyone]]: brute-force the names of the community strings.
3. [[braa]]: brute-force individual OIDs and enumerate their information.
