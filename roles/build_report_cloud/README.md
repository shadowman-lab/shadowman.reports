build_report_cloud
========

Installs Apache and creates a report based on facts from Azure.

Requirements
------------

Must deploy to Apache server

Role Variables / Configuration
--------------

N/A

Dependencies
------------

N/A

Example Playbook
----------------

The role can be used to create an html report on Azure statistics onto an apache server.


```
---
- name: Create Cloud Report
  hosts: all
  tasks:

    - name: Retrieve VNet information
      azure.azcollection.azure_rm_virtualnetwork_info:
      register: vnet_info

    - name: Retrieve Network Interface information
      azure.azcollection.azure_rm_networkinterface_info:
      register: interface_info

    - name: Set Azure information
      ansible.builtin.set_fact:
        azure_sub_id: "{{ lookup('ansible.builtin.env', 'AZURE_SUBSCRIPTION_ID') }}"
        azure_client: "{{ lookup('ansible.builtin.env', 'AZURE_CLIENT_ID') }}"

    - name: Build the report
      ansible.builtin.include_role:
        name: shadowman.reports.build_report_cloud
        apply:
          delegate_to: report.shadowman.dev
          run_once: true      
```
