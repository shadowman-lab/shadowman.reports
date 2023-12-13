build_report_aws
========

Installs Apache and creates a report based on facts from AWS.

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

The role can be used to create an html report on AWS statistics onto an apache server.


```
---
- name: Create AWS Detailed Report
  hosts: localhost
  tasks:

  - name: Get all regions
    amazon.aws.aws_region_info:
      region: "us-east-2"
    register: allregions

  - name: Loop over all ec2 regions
    ansible.builtin.include_tasks:
      file: retrieve_info.yml
    loop: "{{ allregions.regions }}"
    loop_control:
      label: "{{ item.region_name }}"

  - name: Grab boto version
    ansible.builtin.pip:
      name: boto3
    register: register_boto3_version
    check_mode: true

  - name: Grab information about AWS user
    amazon.aws.aws_caller_info:
    register: whoami

  - name: Save username of AWS user and boto3 version
    ansible.builtin.set_fact:
      aws_user: '{{ whoami.arn.split("/")[-1] }}'
      boto3_version: >-
        {{ lookup('pipe', ansible_playbook_python ~ ' -c "import boto3; print(boto3.__version__)"') | default('unknown') }}

  - name: Build the report
    ansible.builtin.include_role:
      name: shadowman.reports.build_report_aws
      apply:
        delegate_to: report.shadowman.dev
        run_once: true      
```

```
---
# retrieve_info.yml
- name: Retrieve vpc information for {{ item.region_name }}
  amazon.aws.ec2_vpc_net_info:
    region: "{{ item.region_name }}"
  register: vpc_info
  delegate_to: localhost

- name: Retrieve info for ec2 instances
  amazon.aws.ec2_instance_info:
    region: "{{ item.region_name }}"
  register: ec2_instance_info
  delegate_to: localhost

- name: Retrieve information about Internet Gateways IGWs
  amazon.aws.ec2_vpc_igw_info:
    region: "{{ item.region_name }}"
  register: igw_info
  delegate_to: localhost

# Dashes are not allowed as Ansible var names so we use underscores _
- name: Set facts all info for {{ item.region_name }}
  ansible.builtin.set_fact:
    '{{ item.region_name|replace("-", "_") }}':
      vpc_info: '{{ vpc_info }}'
      ec2_instance_info: '{{ ec2_instance_info }}'
      igw_info: '{{ igw_info }}'

- name: Set facts all info for {{ item.region_name }}
  ansible.builtin.set_fact:
    all_ec2_regions: "{{ all_ec2_regions | default([]) + [{item.region_name | replace('-', '_'): hostvars[inventory_hostname][item.region_name | replace('-', '_')]}] }}"
```