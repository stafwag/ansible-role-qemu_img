# Ansible Role: qemu_img

An ansible role to create QEMU disk images.

## Requirements

This role use the ```qemu-img``` commmand.

### Supported GNU/Linux Distributions

This role will install ```qemu_img``` package for your GNU/Linux distribution.
This roles is tested on the following GNU/Linux distributions.

* Archlinux
* AlmaLinux
* Debian
* Centos
* Fedora
* RedHat
* Rocky
* Suse
* Ubuntu

### Installation

#### Ansible galaxy

The role is available on [Ansible Galaxy](https://galaxy.ansible.com/ui/standalone/roles/stafwag/qemu_img/).

To install the role from Ansible Galaxy execute the command below.

```bash
$ ansible-galaxy role install stafwag.qemu_img
```

#### Source Code

If you want to use the source code directly.

Clone the role source code.

```bash
$ git clone https://github.com/stafwag/ansible-role-qemu_img stafwag.qemu_img
```

and put into the [role search path](https://docs.ansible.com/ansible/2.4/playbooks_reuse_roles.html#role-search-path)

## Role tasks, tags, variables and templates

### Tasks

* **install**

    All installation-related tasks are defined in the ```install``` playbook. This allows you to install the
    required packages and start/enable the required service with ```tasks_from``` in the ```include_role```,
    ```import_role```, … ansible modules.

    See example below.

### Tags

* **install**

  Install the required packages.

### Variables

* **qemu_img**: "name space"

The data can be in variables or a list (array). When a list is used, the role
will loop over the list and create all defined QEMU disk images.

  * **dest**: required. The destination image.
  * **src**: optional. The source image, a new image will be created if not defined.
  * **size**: optional. required if no src is defined. The size of the destination image.
  * **owner**: uid, default 0. The file owner of the destination image.
  * **group**: gid, default 0. The file group of the destination image.
  * **mode**: mode, default '0400'. The permissions of the destination image.
  * **remote_src**: boolean, default: false. When the source file is in remote host.
  * **format**: format, default: qcow2. The disk image format.
  * **overwrite**: boolean, default: false. Overwrite destination if already exists.

## Dependencies

None

## Example Playbooks

### Install the required packages

```
---
- name: Install libvirt & co
  gather_facts: true 
  hosts: localhost
  become: true
  tasks:
    - name: Install the requirements
      include_role:
        name: "{{ _role }}"
        tasks_from:
          install
      with_items:
        - stafwag.libvirt 
        - stafwag.qemu_img
      loop_control:
        loop_var: _role
```

### Create a new qemu image
 
```
---
- name: Create a new disk image
  gather_facts: no 
  become: true
  hosts: localhost
  roles:
    - role: stafwag.qemu_img
      vars:
        qemu_img:
          dest: datadisk.qcow2 
          size: 20G 
          format: qcow2
```

### Copy a disk image and resize it

```
---
- name: Copy a disk image and resize it
  gather_facts: no 
  become: true
  hosts: localhost
  roles:
    - role: stafwag.qemu_img
      vars:
        qemu_img:
          dest: /var/lib/libvirt/images/tstdebian.qcow2 
          format: qcow2
          size: 50G
          src: /home/staf/Downloads/isos/debian/arm64/cloud/debian-10-openstack-arm64.qcow2
```

### Create multiple disk images

```
---
- name: Create multiple disk images
  gather_facts: no 
  become: true
  hosts: localhost
  roles:
    - role: stafwag.qemu_img
      vars:
        qemu_img:
          - dest: /var/lib/libvirt/images/tstdebian.qcow2 
            src: /home/staf/Downloads/isos/debian/arm64/cloud/debian-10-openstack-arm64.qcow2
          - dest: /var/lib/libvirt/images/tstdebian_data001.qcow 
            size: 20G
          - dest: /var/lib/libvirt/images/tstdebian_data002.qcow 
            size: 20G
```

## License

MIT/BSD

## Author Information

Created by Staf Wagemakers, email: staf@wagemakers.be, website: https://www.wagemakers.be, my company: https://mask27.dev
