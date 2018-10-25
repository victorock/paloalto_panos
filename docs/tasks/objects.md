# About

This function/method perform configuration of interfaces on PANOS devices, adopting the basic datamodels `panos_address_object` and `panos_service_object`.

## Function Parameters (Specs)

For additional details about variables supported by this role and theirs respective default values consult the follwoing documentations:
* [panos_address_object](https://github.com/PaloAltoNetworks/ansible-pan/blob/master/library/panos_address_object.py)
* [panos_service_object](https://github.com/PaloAltoNetworks/ansible-pan/blob/master/library/panos_service_object.py)


## How to configure objects adopting a data-driven approach:

With a data-driven approach, all the specifications are defined at inventory-level either in `group_vars` or `host_vars`.

Roles called by playbooks will consume the data coming from the inventory based on role's definitions/specifications and implementations.


### Inventory Scope: Platform

> /inventory/group_vars/eos/ansible.yaml


```
ansible_network_os: panos
ansible_network_provider: victorock.paloalto_panos
```

### Inventory Scope: Application

> /inventory/group_vars/myapp/panos_address_objects.yaml


```
panos_address_objects:
  - name: "win01"
    value: "10.0.0.11"
    description: "server win01"

  - name: "lin01"
    value: "10.0.0.12"
    description: "server lin01"
```

> /inventory/group_vars/myapp/panos_service_objects.yaml

```
panos_service_objects:
  - name: "service-ssh"
    destination_port: "22"
    protocol: "tcp"
    description: "ssh on 22"

  - name: "service-rdp"
    destination_port: "3389"
    protocol: "tcp"
    description: "rdp on 3389"

  - name: "service-winrms"
    destination_port: "5986"
    protocol: "tcp"
    description: "winrm secure on 5986"
```

### Consume at Playbook


```
- name: "PANOS"
  hosts: "panos:&myapp"
  connection: network_cli
  gather_facts: no
  roles:
    - role: "victorock.paloalto_panos"
      function: objects
```

```
- name: "PANOS"
  hosts: "panos:&myapp"
  connection: network_cli
  gather_facts: no
  roles:
    - role: "victorock.paloalto_panos"
      function: objects
      panos_address_objects:
        - name: "win01-win2016"
            value: "10.10.0.11"
            description: "server win01-win2016"

        - name: "lin01-win2016"
            value: "10.10.0.12"
            description: "server win01-win2016"
      panos_service_objects:
        - name: "service-ssh"
            destination_port: "22"
            protocol: "tcp"
            description: "ssh on 22"

        - name: "service-rdp"
            destination_port: "3389"
            protocol: "tcp"
            description: "rdp on 3389"

        - name: "service-winrms"
            destination_port: "5986"
            protocol: "tcp"
            description: "winrm secure on 5986"
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