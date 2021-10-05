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
