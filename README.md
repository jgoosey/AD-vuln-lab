# Active Directory Lab

This lab is made of three (3) virtual machines:
- **Domain controller** Windows Server 2016
- **Member server** Windows Server 2012r2
- **Windows workstation** Windows 10

### Requirements

So far the lab has only been tested on a linux machine, but it should work as well on macOS. Ansible has some problems with Windows hosts so I don't know about that.

For the setup to work properly you need to install:
- **vagrant** from their official site [vagrant](https://www.vagrantup.com/). The version you can install through your favourite package manager (apt, yum, ...) is probably not the latest one.
- **ansible** following the extensive guide on their website [ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html).

Vagrant will be needed to provision the virtual machines and ansible to automate their configuration.

### Setup

The default domain will be LAB.local on the subnet 192.168.56.1/24.
If you want to change some of these settings, some small modifications are required inside the configuration files (e.g. Vagrantfile, labsetup.yml, etc).

To get the machines up and running, the commands you need to run are as follows:
- `git clone repository`
- `cd AD-vuln-lab`
- `vagrant up`

Then to configure devices with ansible, I like to use a python virtual environment. To do that, run the following commands inside the repo folder:
- `virtualenv .`
- `source bin/activate`
- `python3 -m pip install ansible-core pywinrm`
- `ansible-galaxy install -r requirements.yml`
- `ansible-playbook -i hosts labsetup.yml`

If you run into any problems running the main playbook, you can also run the individual playbooks consecutively:
- `ansible-playbook -i hosts domain_controller.yml`
- `ansible-playbook -i hosts member_server.yml`
- `ansible-playbook -i hosts win_workstation.yml`


### Thanks to

This repo is forked from [alebob AD-lab](https://github.com/alebov/AD-lab) with major comfiguration and vulnerability inspiration from [GOAD](https://github.com/Orange-Cyberdefense/GOAD) and others.
