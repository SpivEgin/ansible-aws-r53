# ansible-aws-r53

travis badge

## Description 

This role manages records within zones in Amazon AWS Route53.

## Provides

## Requires

## Variables

## Playbook and Variable Directory Structure

Recommended directory structure
```
.
+-- playbook.yml
+-- host_vars
|   +-- aws
|   +-- global_constants.yml
|   |   +-- us
|   |   |   +-- regional_constants.yml
|   |   |   +-- r53
|   |   |   |   +-- service_constants.yml
|   |   |   |   +-- r53z_testing_com.yml
```

## Usage

Include this role in your plays, it is recommended that you use a similar directory structure for zone splitting.
I recommend that you add a localhost and local entry to your hosts file. AWS roles/plays do not need to have a system target.

```yaml
---
  name: Manage AWS Route53 ansible-aws-r53
  hosts: localhost

  vars_files:
    - "host_vars/aws/global_constants.yml" # use this for anything that's needed across all AWS regions/properties
    - "host_vars/aws/us/regional_constants.yml" # use this for localized regional constants
    - "host_vars/aws/us/r53/service_constants.yml" # use this for R53 specific regional constants
    
  connection: local
  roles: # each file contains rule definitions for separate domains - easier to manage and tie in with CI
    - { role: ansible-aws-r53, "vars_file": "host_vars/aws/us/r53/r53z_test_com.yml" }
    - { role: ansible-aws-r53, "vars_file": "host_vars/aws/us/r53/r53z_second_test_com.yml" }
```
    
    
    
## Limitations

- Unfortunately, Ansible < 2.0 does not yet support creation/deletion of the zones.
- It will error out if you do not create the zones first.
- It will also error out if the zones were created and maintained outside of the Ansible role. They will need to be created by this role to continue to successfully be maintained by this role.
- At the moment, it does not dynamically read the contents of the r53 directories. This is intentional but may change.
- Does not yet support ALIAS records (Ansible 1.9)
- Does not yet support private zones (within RFC1918 space in VPC) (Ansible 1.9)
- Does not yet support regional differences (Ansible 2.0)
- Does not support any operations around health checks
- Does not support any operations around dynamic facts for Route 53 (Ansible 2.0)

## Tests
This role includes Travis CI tests.