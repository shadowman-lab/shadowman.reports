build_report_patch
========

Installs Apache and creates a report based on facts from Linux patching and Windows update job

Requirements
------------

Must run on Apache server on RHEL

Role Variables / Configuration
--------------

Must register yum results as patchingresult and dnf results as patchingresultdnf. Set Windows patching results to patchresult variable. Set var sendemailreport to false if not also sending the report via e-mail

Dependencies
------------

N/A

Example Playbook
----------------

The role can be used to create an html patching report on a Linux host using any number of Windows & RHEL servers


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

  - name: upgrade all packages (yum)
    ansible.builtin.yum:
      name: '*'
      state: latest
    when: ansible_pkg_mgr == "yum"
    register: patchingresult

  - name: upgrade all packages (dnf)
    ansible.builtin.dnf:
      name: '*'
      state: latest
    when: ansible_pkg_mgr == "dnf"
    register: patchingresultdnf
    
  - name: Build the report
    ansible.builtin.include_role:
      name: shadowman.reports.build_report_patch
      apply:
        delegate_to: report.shadowman.dev
        run_once: true
      
```
