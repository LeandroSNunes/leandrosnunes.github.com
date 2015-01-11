# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "/Users/leandronunes/VirtualBox\ VMs/20140110_dev.box"
  config.vm.network "forwarded_port", guest: 4000, host: 3000
  config.vm.network "private_network", ip: "192.168.50.19"
  config.ssh.forward_agent = true

  config.vm.provider "virtualbox" do |vb|
    vb.customize ["modifyvm", :id, "--memory", "1024"]
  end
end
