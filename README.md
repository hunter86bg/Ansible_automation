# Ansible_automation

# How to use azure_rm_new_rs_group.yml playbook:
- Start 'Azure_RG_management' template from tower
- Enter the Resource group details that you want to manage in the following format:
```
RGs:
  - name: DEMO_RG2
    location: eastus
    tags:
      tag1: anothervalue
      tag2: somevalue
  - name: DEMO2
    state: absent
    forcedelete: 'yes'
```

# How to use azure_new_network.yml
- Start 'Azure_vmnet_management' template from tower
- Enter the virtual network details for creation or deletion in the following format

```
virtual_networks:
  - name: 'vnet2'
    rg: 'DEMO_RG2'
    prefixes:
      - '10.10.10.0/24'
    purge_prefixes: 'yes'
    tags:
      staging: 'prod'
      candelete: 'no'
  - name: 'vnet1'
    rg: 'DEMO_RG2'
    state: absent
```

# How to use azure_new_securitygroup.yml
- Start 'Azure_Security_groups' template from tower
- Once prompted, enter the security group details and the desired rules like Inbound/Outbound direction, priority of the rule, ports, etc

```
security_groups:
  - name: Security_group1
    rg: DEMO_RG2
    rules:
      - name: Block TCP ports from 8080 to 8090
        protocol: Tcp
        destination_port_range: 8080-8090
        access: Deny
        priority: 1000
        direction: Inbound
      - name: AllowSSHFromJumphost
        protocol: Tcp
        source_address_prefix: 213.214.92.205/32
        destination_port_range: 22
        direction: Inbound
        access: Allow
        priority: 1001
      - name: BlockOutbound
        access: Deny
        direction: Outbound
        priority: 1002
  - name: Security_group2
    rg: DEMO_RG2
    state: absent
```
