# Ansible Role: qemu_img

An ansible role to create QEMU disk images.

## Requirements

qemu-img

## Role Variables

### Playbook related variables

* **qemu_img**: "name space"

The data can be in variables or a list (array). When a list is used, the role
will loop over the list and create all defined QEMU disk images.

  * **dest**: required  The destination image
  * **src**:  optional  The source image, a new image will be created if not defined. 
  * **size**: optional, required if no src is defined. The size of the destination image.
  * **owner**: uid default 0  The file owner of the destination image
  * **group**: gid default 0  The file group of the destination image 
  * **mode**:  mode default '0400'  The permissions of the destination image
  * **format**: format default: qcow2 The disk image format.
  * **overwrite**: boolean default: false Overwrite destination if already exists.

## Dependencies

None

## Example Playbooks

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

Created by Staf Wagemakers, email: staf@wagemakers.be, website: http://www.wagemakers.be.
