build_report_linux_patch
========

Installs Apache and creates a report based on facts from Linux patching

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

The role can be used to create an html report on any number of Linux hosts using any number of Linux servers about their patching results(yum and dnf)


```
---
- name: Patch RHEL Servers
  hosts: all

  tasks:
  
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
      name: shadowman.reports.build_report_linux_patch
      apply:
        delegate_to: report.shadowman.dev
        run_once: true     
```
