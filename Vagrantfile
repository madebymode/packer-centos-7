# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.ssh.insert_key = false
  config.vm.synced_folder '../', '/vagrant', type: 'nfs'

  # VMware Fusion.
  # `vagrant up vmware --provider=vmware_fusion`
  config.vm.define "vmware" do |vmware|
    # vmware.vm.hostname = "centos7vmware"
    vmware.vm.box = "file://builds/vmware-centos7.box"
    vmware.vm.network :private_network, ip: "192.168.3.2"

    config.vm.provider "vmware_fusion" do |v|
      host = RbConfig::CONFIG['host_os']
      # Give VM 1/4 system memory & access to 1/2 the cpu cores on the host
      if host =~ /darwin/
        cpus = `sysctl -n hw.ncpu`.to_i / 2
        # sysctl returns Bytes and we need to convert to MB
        mem = `sysctl -n hw.memsize`.to_i / 1024 / 1024 / 4
      elsif host =~ /linux/
        cpus = `nproc`.to_i
        # convert to MB
        mem = `grep 'MemTotal' /proc/meminfo | sed -e 's/MemTotal://' -e 's/ kB//'`.to_i / 1024 / 4
      else
        cpus = 2
        mem = 1024
    end

    # debug
    # v.gui = true
    v.vmx["numvcpus"] = cpus
    v.vmx["memsize"] = mem
  end

    config.vm.provision "ansible_local" do |ansible|
      ansible.playbook = "/vagrant/vagrant-centos7-ansible-config/provision.yml"
    end

  end

  # VirtualBox.
  # `vagrant up virtualbox --provider=virtualbox`
  config.vm.define "virtualbox" do |virtualbox|
    virtualbox.vm.hostname = "centos7virtualbox"
    virtualbox.vm.box = "file://builds/virtualbox-centos7.box"
    virtualbox.vm.network :private_network, ip: "172.16.3.2"

    config.vm.provider "virtualbox" do |v|
      host = RbConfig::CONFIG['host_os']
      # Give VM 1/4 system memory & access to all cpu cores on the host
      if host =~ /darwin/
        cpus = `sysctl -n hw.ncpu`.to_i
        # sysctl returns Bytes and we need to convert to MB
        mem = `sysctl -n hw.memsize`.to_i / 1024 / 1024 / 4
      elsif host =~ /linux/
        cpus = `nproc`.to_i
        # convert to MB
        mem = `grep 'MemTotal' /proc/meminfo | sed -e 's/MemTotal://' -e 's/ kB//'`.to_i / 1024 / 4
      else
        cpus = 2
        mem = 1024
      end

      v.customize ["modifyvm", :id, "--memory", mem]
      v.customize ["modifyvm", :id, "--cpus", cpus]
      v.customize ["modifyvm", :id, "--cpuexecutioncap", "90"]
      v.customize ["modifyvm", :id, "--natdnsproxy1", "off"]
      v.customize ["modifyvm", :id, "--natdnshostresolver1", "off"]
    end

    config.vm.provision "ansible_local" do |ansible|
      ansible.playbook = "/vagrant/vagrant-centos7-ansible-config/provision.yml"
    end
    
  end

end
