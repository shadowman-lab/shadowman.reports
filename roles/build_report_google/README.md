build_report_google
========

Installs Apache and creates a report based on facts from Google.

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

The role can be used to create an html report on Google statistics onto an apache server.


```
---
- name: Create Google Detailed Report
  hosts: localhost
  tasks:

  - name: Get list of all Zones
    ansible.builtin.shell: |
      gcloud compute zones list --uri \
        --account=${GCE_EMAIL} \
        --project=${GCP_PROJECT} \
        --credential-file-override=${GCE_CREDENTIALS_FILE_PATH} \
        | sed -e 's,.*/,,'
    register: all_zones
    changed_when: false
    environment:
      GCE_EMAIL: "{{ lookup('env', 'GCE_EMAIL') }}"
      GCP_PROJECT: "{{ lookup('env', 'GCP_PROJECT') }}"
      GCE_CREDENTIALS_FILE_PATH: "{{ lookup('env', 'GCE_CREDENTIALS_FILE_PATH') }}"

  - name: Get all virtual networks
    google.cloud.gcp_compute_network_info:
      auth_kind: serviceaccount
    register: gcp_networks

  - name: Retrieve VMs from your environment
    google.cloud.gcp_compute_instance_info:
      zone: "{{ item }}"
      project: "{{ gcp_project }}"
      auth_kind: serviceaccount
    loop: "{{ all_zones.stdout_lines }}"
    register: gcp_vms

  - name: Build the report
    ansible.builtin.include_role:
      name: shadowman.reports.build_report_google
      apply:
        delegate_to: report.shadowman.dev
        run_once: true
```
