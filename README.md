# Ansible_automation

# How to use azure_rm_new_rs_group playbook:
- Start 'Azure_RG_management' template
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
