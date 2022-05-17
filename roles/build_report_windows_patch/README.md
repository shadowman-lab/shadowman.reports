build_report_windows_patch
========

Installs Apache and creates a report based on facts from Windows update job

Requirements
------------

Must run on Apache server on RHEL

Role Variables / Configuration
--------------

Set patching results to patchresult variable. Set var sendemailreport to false if not also sending the report via e-mail

Dependencies
------------

N/A

Example Playbook
----------------

The role can be used to create an html patching report on a Linux host using any number of Windows servers


```
---
- name: Windows patching playbook
  hosts: all

  tasks:
  
  - name: Install Windows Updates
    ansible.windows.win_updates:
      category_names: '*'
      reboot: yes
    register: patchresult
    
  - name: Build the report
    ansible.builtin.include_role:
      name: shadowman.reports.build_report_windows_patch
      apply:
        delegate_to: report.shadowman.dev
        run_once: true
      
```
