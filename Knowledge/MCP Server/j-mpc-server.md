define which juniper devices that mcp can manage
```
annaw@p-jtac-lnx01:/volume/pfetools/mcp/config$ cat jmcp_devices.json
{
    "router1": {
        "ip": "1.1.1.1",
        "port": 22,
        "username": "root",
        "auth": {
            "type": "password",
            "password": "Embe1mpls"
        }
    },

    "router2": {
        "ip": "2.2.2.2",
        "port": 22,
        "username": "root",
        "auth": {
            "type": "password",
            "password": "Embe1mpls"
        }
    }
}
```
