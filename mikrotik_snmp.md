# SNMP Configuration

## MikroTik Configuration

Configuration using SNMPv3 with `authPriv` (authentication and encryption).

```
/snmp community
add addresses=::/0 authentication-password=<auth-passphrase> \
    authentication-protocol=SHA1 encryption-password=<enc-passphrase> \
    encryption-protocol=AES name=<snmp-username> security=private
/snmp
set enabled=yes
```


## Linux Client Test

In order to avoid exposing the password in the command line, create configuration file:

```shell
mkdir ~/.snmp
chmod 700 ~/.snmp
touch ~/.snmp/snmp.conf
```

The SNMP configuration file content:

```
defVersion 3
defSecurityLevel AuthPriv
defSecurityName <snmp-username>
defAuthType SHA
defAuthPassphrase <auth-passphrase>
defPrivType AES
defPrivPassphrase <enc-passphrase>
```

Then run: `snmpwalk <snmp-host>`.
