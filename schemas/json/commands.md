
## OpenC2 Command examples - JSON encodings

**Concise** and **Verbose** encodings are transmitted over the wire between
systems.  Concise format encodes structures as JSON arrays; field names
are not transmitted.  Verbose format encodes structures as JSON objects
with field names transmitted as property names.  A decoder will accept
and validate a message in either format and return the same information
to the application as a python dict.  

Concise and Verbose encodings are concrete message formats derived from
the same abstract syntax; they are not syntax alternatives.  An OpenC2
binary message format would likewise be derived from the same abstract
syntax; it would not be a different syntax that requires additional effort
for definition and configuration management.  The purpose of showing Concise
JSON encodings is to illustrate the relationship between a single abstract
syntax and multiple equivalent message formats.  Additional message examples show
only the Verbose format with the understanding that they can be mechanically
converted to equivalent Concise, Flattened and Binary formats. 

**Flattened** encoding is used internally by some applications.  The codec
can convert in both directions between Verbose and Flattened representations.

Target specifiers shown here are derived from CybOX version 2.1 XML definitions.  These
will be updated to use CybOX 3.0 JSON defintions when a stable spec is released.

### MITIGATE
#### Concise
```
["mitigate",
["cybox:Hostname",["cdn.badco.org"]]]
```
#### Verbose
```
{
	"action": "mitigate",
	"target": {
		"type": "cybox:Hostname",
		"specifiers": {"Hostname_Value": "cdn.badco.org"}
	}
}
```
#### Flattened
```
{
	"action": "mitigate",
	"target.type": "cybox:Hostname",
	"target.specifiers.Hostname_Value": "cdn.badco.org"
}
```
### ALLOW / DENY
#### Concise
```
["DENY",
["cybox:Network_Connection",["IPv4","TCP",[["ip_address",["any"]]],[["ip_address",["10.10.10.2"]]]]],
["network-firewall",[null,"30"]],
{"context_ref": 91}]
```
#### Verbose
```
{
	"ACTION": "DENY",
	"TARGET": {
		"type": "cybox:Network_Connection",
		"specifiers": {
			"Layer3Protocol": "IPv4",
			"Layer4Protocol": "TCP",
			"SourceSocketAddress": {
				"IP_Address": {
					"Address_Value": "any"
				}
			},
			"DestinationSocketAddress": {
				"IP_Address": {
					"Address_Value": "10.10.10.2"
				}
			}
		}
	},
	"ACTUATOR": {
		"type": "network-firewall",
		"specifiers": {
			"asset_id": "30"
		}
	},
	"MODIFIERS": {
		"context_ref": 91
	}
}
```
#### Flattened
```
{
	"ACTION": "DENY",
	"TARGET.type": "cybox:Network_Connection",
	"TARGET.specifiers.Layer3Protocol": "IPv4",
	"TARGET.specifiers.Layer4Protocol": "TCP",
	"TARGET.specifiers.SourceSocketAddress.IP_Address.Address_Value": "any",
	"TARGET.specifiers.DestinationSocketAddress.IP_Address.Address_Value": "10.10.10.2",
	"ACTUATOR.type": "network-firewall",
	"ACTUATOR.specifiers.asset_id": "30",
	"MODIFIERS.context_ref": 91
}
```