# About

This function/method perform configuration of interfaces on PANOS devices, adopting the basic datamodels `panos_system`.

## Function Parameters (Specs)

For additional details about variables supported by this role and theirs respective default values consult the follwoing documentations:
* [panos_mgtconfig](https://github.com/PaloAltoNetworks/ansible-pan/blob/master/library/panos_mgtconfig.py)


## How to configure system adopting a data-driven approach:

With a data-driven approach, all the specifications are defined at inventory-level either in `group_vars` or `host_vars`.

Roles called by playbooks will consume the data coming from the inventory based on role's definitions/specifications and implementations.


### Inventory Scope: Platform

> /inventory/group_vars/eos/ansible.yaml

```
ansible_network_os: panos
ansible_network_provider: victorock.paloalto_panos
```

### Inventory Scope: Device

> /inventory/host_vars/panos01/panos_system.yaml

```
panos_system:
  hostname: "{{ inventory_hostname_short }}"
  login_banner: "Automated by Ansible"
  time_zone: "Europe/Paris"
```

### Consume at Playbook


```
- name: "PANOS"
  hosts: "panos"
  connection: network_cli
  gather_facts: no
  roles:
    - role: "victorock.paloalto_panos"
      function: system
```

```
- name: "PANOS"
  hosts: "panos"
  connection: network_cli
  gather_facts: no
  roles:
    - role: "victorock.paloalto_panos"
      function: system
      panos_system:
        hostname: "{{ inventory_hostname_short }}"
        login_banner: "Automated by Ansible"
        time_zone: "Europe/Paris"
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
