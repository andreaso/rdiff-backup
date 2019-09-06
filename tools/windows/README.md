# Windows development environment

Starting from https://github.com/redhat-cop/automate-windows/tree/master/vagrant-libvirt-image, create a Windows VM
usable by Ansible (any other alternative approach to a such VM is of course valid).

Then apply the playbook playbook-provision-windows.yml using Ansible to the Windows you've just created to create the necessary development environment.
The current state of the automation isn't very satisfying, the more complex packages need to be installed manually from the command line
with something like `choco install <packagename>` and the playbook restarted.

TODO continue and refine

## Miscellaneous considerations

### Vagrant and Ansible

Vagrant manages an inventory file for Ansible, so that commands like the following ones are possible:

```
ansible -i .vagrant/provisioners/ansible/inventory/vagrant_ansible_inventory -m win_reboot default
ansible-playbook -i .vagrant/provisioners/ansible/inventory/vagrant_ansible_inventory playbook-provision-windows.yml -vvvvv
```

### Libvirt

After the installation of the virtio drivers, shutdown the VM and go to the virtmanager and change following parameters to get more performance:

* OS information -> Operating System -> Microsoft Windows 10

Those two settings were overwritten at next `vagrant up` and led to some issues:

* Video _whatever_ -> Model: Virtio, 3D acceleration: set
* Display VNC -> Type: Spice server, Listen type: none and OpenGL: set

Those two settings didn't work as expected and made boot resp. network fail:

* SATA Disk 1 -> Advanced options -> Disk bus: VirtIO
* NIC _whatever_ -> Device model: virtio

### Chocolatey

The logs of Chocolatey are available under `C:\ProgramData\chocolatey\logs` and `C:\Users\IEUser\AppData\Local\Temp\chcolatey`.
