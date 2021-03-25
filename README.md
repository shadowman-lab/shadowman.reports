# Shadowman Reports Collection

The Ansible Shadowman Reports collection includes a variety of Ansible content to help automate the creation of Linux, Windows and Networking reports.

<!--start requires_ansible-->
## Ansible version compatibility

This collection has been tested against following Ansible versions: **>=2.9.10**.

Plugins and modules within a collection may be tested with only specific Ansible versions.
A collection may contain metadata that identifies these versions.
PEP440 is the schema used to describe the versions of Ansible.
<!--end requires_ansible-->

## Tested with Ansible

This collection has been tested against RHEL 7 and 8, Windows Server 2016, IOS, EOS, VyOS
<!-- List the versions of Ansible the collection has been tested with. Must match what is in galaxy.yml. -->

## External requirements
<!-- List any external resources the collection depends on, for example minimum versions of an OS, libraries, or utilities. Do not list other Ansible collections here. -->

## Included content
<!--start collection content-->


### Modules
Name | Description
--- | ---
[shadowman.reports.scan_packages](https://github.com/shadowman-lab/shadowman.reports/blob/main/docs/shadowman.reports.scan_packages_module.rst)|Scans for all packages on a Linux server
[shadowman.reports.scan_services](https://github.com/shadowman-lab/shadowman.reports/blob/main/docs/shadowman.reports.scan_services_module.rst)|Scans for all services on a Linux server
[shadowman.reports.win_scan_packages](https://github.com/shadowman-lab/shadowman.reports/blob/main/docs/shadowman.reports.win_scan_packages_module.rst)|Scans for all packages on a Windows server
[shadowman.reports.win_scan_services](https://github.com/shadowman-lab/shadowman.reports/blob/main/docs/shadowman.reports.win_scan_services_module.rst)|Scans for all services on a Windows server

<!--end collection content-->

## Installing this collection

You can install the Shadowman Reports collection with the Ansible Galaxy CLI:

    ansible-galaxy collection install shadowman.reports

You can also include it in a `requirements.yml` file and install it with `ansible-galaxy collection install -r requirements.yml`, using the format:

```yaml
---
collections:
  - name: shadowman.reports
```
## Using this collection

### Using Shadowman Reports Ansible Collection

An example for using this collection to scan a Linux host for packages:


```yaml
---
- name: Scan packages of all Linux hosts
  hosts: linux

  

  tasks:

    - name: Scan packages (Unix/Linux)
      shadowman.reports.scan_packages:
        os_family: '{{ ansible_os_family }}'
      
```


### Code of Conduct
This collection follows the Ansible project's
[Code of Conduct](https://docs.ansible.com/ansible/devel/community/code_of_conduct.html).
Please read and familiarize yourself with this document.

## Roadmap

<!-- Optional. Include the roadmap for this collection, and the proposed release/versioning strategy so users can anticipate the upgrade/update cycle. -->

## More information

- [Ansible Collection overview](https://github.com/ansible-collections/overview)
- [Ansible User guide](https://docs.ansible.com/ansible/latest/user_guide/index.html)
- [Ansible Developer guide](https://docs.ansible.com/ansible/latest/dev_guide/index.html)
- [Ansible Community code of conduct](https://docs.ansible.com/ansible/latest/community/code_of_conduct.html)

## Licensing

GNU General Public License v3.0 or later.

See [LICENSE](https://www.gnu.org/licenses/gpl-3.0.txt) to see the full text.