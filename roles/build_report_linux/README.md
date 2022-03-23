build_report_linux
========

Installs Apache and creates a report based on facts from Linux services and packages modules. Utilize var "detailedreport=false" if you do not need packages or services information

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

The role can be used to create an html report on any number of Linux hosts using any number of Linux servers about their services and packages installed


```
---
- name: Create Linux Report
  hosts: all
  tasks:
  
    - name: Scan packages (Unix/Linux)
      shadowman.reports.scan_packages:
        os_family: '{{ ansible_os_family }}'
      when: ansible_os_family != "Windows"

    - name: Scan services (Unix/Linux)
      shadowman.reports.scan_services:
      when: ansible_os_family != "Windows"

    - name: Build the report
      ansible.builtin.include_role:
        name: shadowman.reports.build_report_linux
        apply:
          delegate_to: report.shadowman.dev
          run_once: true      
```
