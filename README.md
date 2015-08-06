# Provision Ansible Tower using Vagrant

This is a simple Vagrantfile to provision a new Ansible Tower instance as a
Vagrant virtual machine. It has been tested with Tower version 2.2.0.

This uses the Vagrant Ansible provisioner to call the Tower installation
playbook directly, rather than calling the setup routines directly within the
VM. The parameters to the Tower installer are passed as Ansible 'extra
arguments'.

## Pre-requisites
This Vagrantfile expects there to be a directory called `installer` in the same
directory as the Vagrantfile itself, containing the contents of the Ansible
Tower [installation tarball](http://releases.ansible.com/ansible-tower/setup/).

You can copy-and-paste this command to make life easy:

`curl -s http://releases.ansible.com/ansible-tower/setup/ansible-tower-setup-latest.tar.gz | tar -C installer --strip-components=1 -zxvf -`

## Configuration Options
By default, the passwords for both the Tower and Munin "admin" users defaults
to 'admin'. You can override this by setting the `TOWER_ADMINPW` and
`TOWER_MUNINPW` environment variables before calling `vagrant up`.

## Automatic Licensing
If you have an [Ansible Tower license](http://www.ansible.com/license) you can
put this into a file named `tower-license` in the root directory of this git
checkout, and the license file will be automatically applied after the
installation.

### TODO
- [ ] Automatically download the correct Tower installation tarball
- [ ] Provide some "client" systems for Tower to talk to
- [ ] Setup a 'demo' environment directly in Tower using the API?
