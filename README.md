# pfsense-clitools

### Setup

You can set PFAUTH statically or use references from a 1password vault ; do a "op signin" or whatever before doing this. PFURL is the device you want to talk to....

```
export PFAUTH='Authorization: SHORT-HEX LONG-HEX'
export PFAUTH="Authorization: "`op read "op://Private/pfSense API Key/username"`" "`op read "op://Private/pfSense API Key/password"`
export PFURL='https://10.0.0.1/api/v1'
```

### Aliases

Here are some zsh/bash aliases:

```
pf-devices() {  curl -H "$PFAUTH" -sk "${PFURL}/services/dhcpd/static_mapping?interface=lan" | jq -c '.data[] | [ .ipaddr, .hostname, .mac ]' ; }
pf-leases() {  curl -H "$PFAUTH" -sk "${PFURL}/services/dhcpd/lease" | jq -c '.data[]' | grep -v 'expired'; }
pf-arpinfo() {  curl -H "$PFAUTH" -sk "${PFURL}/system/arp" | jq -c '.data[] | [.interface, .mac, .ip]'  ; }
pf-whatsnew() {  curl -H "$PFAUTH" -sk "${PFURL}/services/dhcpd/lease" | jq -c '.data[] | [.starts,.ip, .mac, .hostname]' |  grep -v 'expired'; }
```
