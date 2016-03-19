# MODE Packer - CentOS 7 minimal Vagrant Box using Ansible provisioner

**Current CentOS Version Used**: 7.2

This example build configuration installs and configures CentOS 7 x86_64 minimal using Ansible, and then generates two Vagrant box files, for:

  - VirtualBox
  - VMware Fusion

The example can be modified to use more Ansible roles, plays, and included playbooks to fully configure (or partially) configure a box file suitable for deployment for development environments.

## Requirements

The following software must be installed/present on your local machine before you can use Packer to build the Vagrant box file:

  - [Packer](http://www.packer.io/)
  - [Vagrant](http://vagrantup.com/)
  - [VirtualBox](https://www.virtualbox.org/) (if you want to build the VirtualBox box)
  - [VMware Fusion](http://www.vmware.com/products/fusion/) (or Workstation - if you want to build the VMware box)
  - [Ansible](http://docs.ansible.com/intro_installation.html)

I install everything via homebrew/cask

```
$ brew cask install virtualbox  vmware-fusion
$ brew install vagrant packer ansible
```
You will also need some Ansible roles installed so they can be used in the building of the VM. To install the roles:

  1. Run `$ ansible-galaxy install -r requirements.txt` in this directory.
  2. If your local Ansible roles path is not the (homebrewed) default (`/usr/local/etc/ansible/`), update the `role_paths` inside `centos7.json` to match your custom location.

If you don't have Ansible installed (perhaps you're using a Windows PC?), you can simply clone the required Ansible roles from GitHub directly (use [Ansible Galaxy](https://galaxy.ansible.com/) to get the GitHub repository URLs for each role listed in `requirements.txt`), and update the `role_paths` variable to match the location of the cloned role.

## Usage

Make sure all the required software (listed above) is installed, then cd to the directory containing this README.md file, and run:

    $ packer build centos7.json

After a few minutes, Packer should tell you the box was generated successfully.

If you want to only build a box for one of the supported virtualization platforms (e.g. only build the VMware box), add `--only=vmware-iso` to the `packer build` command:

    $ packer build --only=vmware-iso centos7.json

## License

MIT license.

## Author Information

Customized in 2016 by [Troy McCall](https://troymccall.com) for [MODE](http://madebymode.com).

Created in 2014 by [Jeff Geerling](http://jeffgeerling.com/), author of [Ansible for DevOps](http://ansiblefordevops.com/).
