# Data models assignment

## About
In this assigment I explore the process of developing infrastructure and service data models for l2vpn over ldp mpls using isis

## Details
**device_underlay.yml** 
 - _uses roles to generate config snippets of feature components of the infrastructure_

**generate_service_model.yml** 
 - _generates transactional service data model (**services/svc-CUSTOMER/service-model.json**) for customers in services.yml. Service model contains data required by templates to produce device config for an l2circuit_

**generate_service_config.yml** 
 - _generates device specific configuration required for a customer l2vpn service using the customer service data model_


**device-configs** contains per device snippets (ldp mpls and isis configuration) produced by device_underlay.yml

**service-configs** contains per device configuration snippets for customer services produced by generate_service_config.yml


## Data model

**hostvars** contain yaml files for each device in the lab; this is used to generate network infrastructure configuration. Each device has a hostname, management IP, router ID and iso address as well as defined interfaces used for links among peers in the network.
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
**services.yml** contains service definition per customer. Each customer is a dictionary containing the circuit id, service type, and endpoint locations for the l2vpn service.
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
