build_report_network
========

Installs Apache and creates a report based on facts from network devices. Use in combination with facts modules for different Networking devices.

Requirements
------------

Must run on Apache server

Role Variables / Configuration
--------------

N/A

Dependencies
------------

N/A

Example Playbook
----------------

The role can be used to create an html report on any number of Linux hosts using any number of network devices


```
---
- name: Build Networking Device Report
  hosts: all
  gather_facts: false

  tasks:
  
  - name: Grab Arista eos facts
    arista.eos.eos_facts:
      gather_subset: 'min'
      gather_network_resources: all
    register: eosfacts
    
  - name: Grab Cisco ios facts
    cisco.ios.ios_facts:
      gather_subset: 'min'
      gather_network_resources: all
    register: iosfacts
    
  - name: Grab vyos facts
    vyos.vyos.vyos_facts:
      gather_subset: 'min'
      gather_network_resources: all
    register: vyosfacts
    
  - name: Build the report
    ansible.builtin.include_role:
      name: shadowman.reports.build_report_network
      apply:
        delegate_to: report.shadowman.dev
        run_once: true
      
```
