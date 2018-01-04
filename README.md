[![Scrutinizer Code Quality](https://scrutinizer-ci.com/g/jordimolesblanco/ansible-vmware/badges/quality-score.png?b=master)](https://scrutinizer-ci.com/g/jordimolesblanco/ansible-vmware/?branch=master)
[![Build Status](https://scrutinizer-ci.com/g/jordimolesblanco/ansible-vmware/badges/build.png?b=master)](https://scrutinizer-ci.com/g/jordimolesblanco/ansible-vmware/build-status/master)

# Ansible automation to clone a vm hosted in VMware

For now, this is a very simple playbook (plus roles) to clone a vm that we already have in VMware, but other
playbooks and roles can be easily added by copying the structure you will see in this repo.

It basically does...

1. Takes two arguments:
    - Name of the new vm.
    - Name of the machine we want to clone from.
2. Checks if the given name for the new vm is allowed (we only allow items in the inventory)
3. Stops and destroys the "new vm" if it's already created (it destroys the vm that matches the name). This is because
in my use case all the vms in the list are dev boxes that get broken for several reasons and this automation allows
devs to recreate their box easily. Your use case might be different and you may want to add a "are you sure?" message.
4. It creates the new vm.
5. It waits until the new vm is reachable via ssh.

All you need to do to test this playbook is:

1. Copy your ssh public key in the box you are going to clone from.
2. Populate the inventory file with the hostnames and ips of the machines you want to allow to be created. I placed it
in ansible/inventory.yml, just add the info relevant in your env.
3. Populate the the group_vars/all file with the info relevant in your env.
4. Run the playbook like this:
```bash
ansible-playbook clonevm.yml -i inventory.yml -vvv -e "source_vm=my-parent-vm dest_vm=my-new-vm"
```