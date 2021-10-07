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

# How to use azure_new_sqlserver.yml
- Create a copy of vaults/sqlvault.yml into the vaults directory
- Change the username
- Generate a passowrd and substitute it in the vault file:
```
ansible-vault encrypt_string --stdin-name 'password'
```
- Remember the vault password
- Update the 'vars_file' stanza (azure_new_sqlserver.yml) with the new file (in case you didn't reuse the old one)
- Go to tower -> Credentials -> New Credential of type vault and enter the password
- Attach the new vault (created in the previous step) to 'Azure_SQL_Server' template
- Launch and when prompted enter the needed variables like:
```
azure_sql_servers:
  - name: uniquesqlserver116
    rg: DEMO_RG2
    location: eastus
  - name: uniquesqlserver222
    rg: DEMO_RG2
    state: absent
```

# How to use azure_new_sqldb.yml
- Create a SQL host via azure_new_sqlserver.yml
- Run Azure_SQL_DB template from Tower with parameters like:
```
sql_dbs:
  - name: newdb
    rg: DEMO_RG2
    servername: uniquesqlserver116
    location: eastus
  - name: deleteddb
    rg: DEMO_RG2
    servername: nonexistingsqldb-randomstring
    state: absent
```

# How to use azure_new_storageaccount.yml
- Run Azure_Storage_acc from Tower with similar parameters (tags must be in quotes, storage account can have lowercase letters and numbers):
```
---
azure_storageaccounts:
  - name: "createme121"
    rg: "DEMO_RG2"
    type: "Standard_RAGRS"
    tags:
      staging: "nonprod"
      demo: "true"
  - name: "deleteme121"
    rg: "DEMO_RG2"
    state: absent
```

# How to use azure_create_vm.yml
- Create an ssh key (or use existing one) via:
```
 cat demokey.pub  | ansible-vault encrypt_string --stdin-name sshkey
```
- Create a new vault or use existing one
- Create a secret in Ansible tower with the password used to encrypt the ssh key
- Replace the secret (if using a new one) in the Azure_VMs template in Tower
- Run the template Azure_VMs with parameters similar to those:

```
azure_vms:
  - name: vm1
    rg: DEMO_RG2
    managed_disk_type: Standard_LRS
    subnet_name: subnet1
    image:
      offer: RHEL
      publisher: RedHat
      sku: '8_4'
      version: 8.4.2021081003
  - name: vm2defaults
    rg: DEMO_RG2
    image:
      offer: RHEL
      publisher: RedHat
      sku: '8_4'
      version: 8.4.2021081003
  - name: notexistingvm
    rg: DEMO_RG2
    state: absent
```

# How to use azure_availability_set.yml
- Verify that there are enough CPU quotas. If the quotas have been exceeded the module errors out with a traceback (debugging nightmare)
- Next run Azure_VM_SCALESET template from Tower with similar extra vars:
```
azure_availability_set:
  - name: "scaleset1"
    rg: "DEMO_RG2"
    disk_type: "Standard_LRS"
    vm_network: "scalesetvnet1"
    subnet_name: "scalesetsubnet1"
    image:
      offer: "RHEL"
      publisher: "RedHat"
      sku: "82gen2"
      version: "8.2.2021091102"
    capacity: "5"
  - name: "scalesetunique2"
    rg: "DEMO_RG2"
    state: "absent"
```
