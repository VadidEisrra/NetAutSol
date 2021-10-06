# Data models

In this assigment I explore the process of developing infrastructure and service data models to manage l2vpn over mpls in a multi-vendor environment.

- network infrastructure data is stored in node specific YAML in hostvars directory
- service data is stored in _services/services.yml_ file

Infrastructure configuration snippets are generated via _devices_underlay.yml_ playbook using ansible roles and stored in _device-config_ directory.

Service configuration is generated in two steps:
1. **generate_service_model.yml** consumes customer specific data in _services.yml_ and produces a complete service model containing all elements required for l2circuit template to generate device-specific configuration. These are found in *services/svc-CUSTOMER/service-model.json*

2. **generate_service_config.yml** parses the customer _service-model.json_ and produces device-specific configuration for the l2circuit. These configuration snippets are stored in *service-configs/DEVICE/customer-[a|z]_service_details.conf*

## Data model

**hostvars** contain yaml files for each device in the lab. Each defines
- hostname
- management IP
- router ID
- iso address
- interfaces
```
---
hostname: cfr1.lab

mgmt_ip: 10.10.5.2
router_id: 1.1.1.1
router_iso: 49.0010.0100.1001.00

interfaces:
  - name: xe-0/0/2
    peer: cfr3.lab
    ip_address: 172.16.0.9/30
  - name: xe-0/0/3
    peer: cfr4.lab
    ip_address: 172.16.0.5/30
  - name: ae0
    peer: cfr2.lab
    ip_address: 172.16.0.1/30
    members: [xe-0/0/0]
```
**services.yml** contains service definition per customer. Each defines
- circuit id
- service type
- A endpoint device and port
- Z endpoint device and port
```
---
Neptune:
  cid: 100
  service: l2ckt
  a_loc: { node: cfr1.lab, iface: "xe-0/0/4" }
  z_loc: { node: cfr2.lab, iface: "xe-0/0/4" }
  
Venus:
  cid: 200
  service: l2ckt
  a_loc: { node: cfr1.lab, iface: "xe-0/0/5" }
  z_loc: { node: cfr3.lab, iface: "Gi0/0/0/2" }
```
