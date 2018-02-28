# Installing linux from linux
This role triggers a reinstallation of the operating system.

Currently, only MBR-systems are supported (..). First, kernel and initrd are copied to the remote host, then the grub.cfg is edited to start the installation on reboot.
 
# Usage:
First, create a directory holding the playbook (example see below) you're going to use to trigger a reinstallation and enter it.
Then get the files neccessary for installation:
```mkdir boot
repo="http://ftp.gwdg.de/pub/linux/fedora/linux/releases/27/Server/x86_64/os"
wget $repo/images/pxeboot/vmlinuz -O boot/vmlinuz-install
wget $repo/images/pxeboot/initrd.img -O boot/initrd-install
```
You will need a kickstart- (fedora)/preseed- /... file to automate your installation. Currently this needs to be available on a webserver (see inst.ks - value ).

Then, execute the following:
## Example playbook:
```- name: Install new os
  hosts: {{target}}
  vars:
    - kparams:
      - name: inst.ks
        value: "http://laptop.local/fc27.ks"
      - name: inst.stage2
        value: "http://ftp.gwdg.de/pub/linux/fedora/linux/releases/27/Server/x86_64/os"
      - name: inst.repo
        value: "http://ftp.gwdg.de/pub/linux/fedora/linux/releases/27/Everything/x86_64/os"
      - name: inst.text 
  roles: [{'role': os-install}]```
