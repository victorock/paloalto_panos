# About

This function/method perform configuration of interfaces on PANOS devices, adopting the basic datamodel (`panos_interfaces`) specified by `ansible-network.interfaces`.

## Function Parameters (Specs)

For additional details about variables supported by this role and theirs respective default values consult the [panos_interface](https://github.com/PaloAltoNetworks/ansible-pan/blob/master/library/panos_interface.py)


## How to configure interfaces adopting a data-driven approach:

With a data-driven approach, all the specifications are defined at inventory-level either in `group_vars` or `host_vars`.

Roles called by playbooks will consume the data coming from the inventory based on role's definitions/specifications and implementations.


## Specify Scope at Inventory

### Scope: Platform

> /inventory/group_vars/eos/ansible.yaml


```
ansible_network_os: panos
ansible_network_provider: victorock.paloalto_panos
```

### Scope: Device

> /inventory/host_vars/panos1/panos_interfaces.yaml


```
panos_interfaces:
  - name: "ethernet1/1"
    zone: "EXTERNAL"
    management_profile: "ALLOW-MGMT"
    enable_dhcp: "false"
    mode: "layer3"
    vr_name: "VRF_EXTERNAL"
    ip: "10.00.0.1/24"
    default_route: "false"
```


## Consume at Playbook

> Example1:

```
- name: "PANOS"
  hosts: "panos"
  connection: network_cli
  gather_facts: no
  roles:
    - role: "victorock.paloalto_panos"
      function: interfaces
```

> Example2:
```
- name: "PANOS"
  hosts: "panos"
  connection: network_cli
  gather_facts: no
  roles:
    - role: "victorock.paloalto_panos"
      function: interfaces
      panos_interfaces:
        - name: "ethernet1/1"
          zone: "EXTERNAL"
          management_profile: "ALLOW-MGMT"
          enable_dhcp: "false"
          mode: "layer3"
          vr_name: "VRF_EXTERNAL"
          ip: "10.10.0.1/24"
          default_route: "false"

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