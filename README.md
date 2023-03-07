Role Name
=========

A brief role to configure a NFS server with some exports

Requirements
------------

No specific requirements, everything needed will be installed/configured.

Role Variables
--------------

The role uses these variables:
```
        all_same_options:
          base_dir: /exports/test
          allowed_address: "*"
          number_exports: 5
          export_options: "rw,sync,no_root_squash"
```
And
```
        list_of_shares:
          - exportdir: "/export/blah1"
            allowed_address: "*"
            export_options: "rw,async,no_root_squash"
          - exportdir: "/export/blah2"
            allowed_address: "192.168.0.1"
            export_options: "rw,async,no_root_squash"
          - exportdir: "/export/blah3"
            allowed_address: "10.0.0.1"
            export_options: "rw,async,no_root_squash"
```

The first dictionary "all_same_options" is used when you want to export shares that have the same options
and naming.
Note that these variables will be used for every export, so in this case it will end up with exports like:
```
# exportfs -v
/exports/test0  <world>(async,wdelay,hide,no_subtree_check,sec=sys,rw,secure,no_root_squash,no_all_squash)
/exports/test1  <world>(async,wdelay,hide,no_subtree_check,sec=sys,rw,secure,no_root_squash,no_all_squash)
/exports/test2  <world>(async,wdelay,hide,no_subtree_check,sec=sys,rw,secure,no_root_squash,no_all_squash)
```

The second dictionary "list_of_shares" is used instead when you just want to have a list of shares which 
have diverse options and names.
In the second case you will end up with this these exports:
```
# exportfs -v
/exports/blah1  <world>(async,wdelay,hide,no_subtree_check,sec=sys,rw,secure,no_root_squash,no_all_squash)
/exports/blah2  192.168.0.1(async,wdelay,hide,no_subtree_check,sec=sys,rw,secure,no_root_squash,no_all_squash)
/exports/blah3  10.0.0.1(async,wdelay,hide,no_subtree_check,sec=sys,rw,secure,no_root_squash,no_all_squash)
```

NOTE: you have to substitute all the entries of the dictionary and not only some (an error will be returned
      in case).

If you want to modify any of these, set the roles variables or the hosts/group vars.

Dependencies
------------

This role requires ansible.posix.firewalld. Depending where it's used, you may need to install the `ansible.posix`
collection. You can create a requirements file with:
```
collections:
  - name: ansible.posix
```
And run:
```
# ansible-galaxy install -r requirements.yml
```

Example Playbook
----------------

A possible playbook with custom options is:

```
---
- name: Configure nfs server
  hosts: all
  become: true
  roles:
    - role: nfs-server
      vars:
        all_same_options:
          base_dir: /exports/test
          allowed_address: "*"
          number_exports: 5
          export_options: "rw,sync,no_root_squash"
```

License
-------

BSD

Author Information
------------------

Pierguido Lambri
