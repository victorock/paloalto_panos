# About

This function/method perform configuration of interfaces on PANOS devices, adopting the basic datamodels `panos_security_rule` and `panos_nat_rule`.

## Function Parameters (Specs)

For additional details about variables supported by this role and theirs respective default values consult the follwoing documentations:
* [panos_security_rule](https://github.com/PaloAltoNetworks/ansible-pan/blob/master/library/panos_security_rule.py)
* [panos_nat_rule](https://github.com/PaloAltoNetworks/ansible-pan/blob/master/library/panos_nat_rule.py)


## How to configure rules adopting a data-driven approach:

With a data-driven approach, all the specifications are defined at inventory-level either in `group_vars` or `host_vars`.

Roles called by playbooks will consume the data coming from the inventory based on role's definitions/specifications and implementations.


### Inventory Scope: Platform

> /inventory/group_vars/eos/ansible.yaml


```
ansible_network_os: panos
ansible_network_provider: victorock.paloalto_panos
```

### Inventory Scope: Application

> /inventory/group_vars/myapp/panos_security_rules.yaml


```
panos_security_rules:
  - rule_name: "Incoming SSH"
    description: "Allow Incoming SSH traffic"
    service: 
      - "service-ssh"
    source_zone: 
      - "EXTERNAL"
    destination_zone:
      - "INTERNAL"

  - rule_name: "Incoming WINRMS"
    description: "Allow Incoming WINRMS traffic"
    service:
      - "service-winrms"
    source_zone:
      - "EXTERNAL"
    destination_zone:
      - "INTERNAL"

  - rule_name: "Outgoing All"
    description: "Allow All Outgoing traffic"
    source_zone:
      - "INTERNAL"
    destination_zone:
      - "EXTERNAL"
```

> /inventory/group_vars/myapp/panos_nat_rules.yaml

```
panos_nat_rules:
  - rule_name: "ALLOUT"
    snat_type: "dynamic-ip-and-port"
    snat_interface: "ethernet1/1"
    snat_address_type: "interface-address"
    snat_interface_address: "10.0.0.1/24"
    destination_zone: "EXTERNAL"
    source_zone:
      - "INTERNAL"
    source_ip:
      - "any"
    destination_ip:
      - "any"
```

### Consume at Playbook

> Example1:
```
- name: "PANOS"
  hosts: "panos:&myapp"
  connection: network_cli
  gather_facts: no
  roles:
    - role: "victorock.paloalto_panos"
      function: rules
```

> Example2:
```
- name: "PANOS"
  hosts: "panos:&myapp"
  connection: network_cli
  gather_facts: no
  roles:
    - role: "victorock.paloalto_panos"
      function: rules
      panos_security_rules:
        - rule_name: "Incoming SSH"
            description: "Allow Incoming SSH traffic"
            service: 
            - "service-ssh"
            source_zone: 
            - "EXTERNA"
            destination_zone:
            - "INTERNA"

        - rule_name: "Incoming WINRMS"
            description: "Allow Incoming WINRMS traffic"
            service:
            - "service-winrms"
            source_zone:
            - "EXTERNA"
            destination_zone:
            - "INTERNA"

        - rule_name: "Outgoing All"
            description: "Allow All Outgoing traffic"
            source_zone:
            - "INTERNA"
            destination_zone:
            - "EXTERNA"
      panos_nat_rules:
        - rule_name: "NATAllOut"
            snat_type: "dynamic-ip-and-port"
            snat_interface: "ethernet1/1"
            snat_address_type: "interface-address"
            snat_interface_address: "10.10.0.1/24"
            destination_zone: "EXTERNA"
            source_zone:
            - "INTERNA"
            source_ip:
            - "any"
            destination_ip:
            - "any"
```

# Requirements

The following variables must be set for `ansible-network.network-engine`:

```
ansible_network_os: panos
ansible_network_provider: victorock.paloalto_panos
```

# Dependencies

This role requires the following roles:

```
ansible-network.network-engine
paloaltonetworks.paloaltonetworks
```