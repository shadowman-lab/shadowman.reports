scan_packages - This module handles scanning Linux packages
====================================
- [Synopsis](Synopsis)
- [Requirements](Requirements)
- [Parameters](Parameters)
- [Examples](Examples)

## Synopsis
    This module will scan all packages on a Linux host when supplied with the os_family

## Requirements
None

## Parameters

<table>
<tr>
<th> Parameter </th>
<th> Choices/Defaults </th>
<th> Configuration </th>
<th> Comments </th>
</tr>
<tr>
<td><b>os_family</b></br>
</td>
<td></td>
<td></td>
<td> The os_family from Ansible fact gathering to determine what package management system to use
</td>
</tr>
</table>

## Examples
```

- name: Scan packages (Unix/Linux)
scan_packages:
    os_family: '{{ ansible_os_family }}'

```