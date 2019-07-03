# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
ENV['VAGRANT_DEFAULT_PROVIDER'] = 'libvirt'
Vagrant.configure("2") do |config|

  config.vm.provider :libvirt do |libvirt| 
    libvirt.uri = "qemu:///system"
    libvirt.storage_pool_name = "vagrant"
  end 

  num_nodes = 3 
  nodes = []
  primary = []
  replicas = [] 
  (1..num_nodes).each do |i|
    nodes.push("galera-#{i}")
    if num_nodes == 1
      primary.push("galera-#{i}")
    else
      replicas.push("galera-#{i}")
    end
  end

  groups = {
    "nodes" => nodes , 
    "primary" => primary , 
    "replicas" => replicas
  }

  (1..num_nodes).each do |i| 
    config.vm.define "galera-#{i}" do |node| 
      node.vm.box = "centos/7"
      node.vm.hostname = "galera-#{i}"
      node.vm.network :private_network, 
                      :libvirt__network_name =>  "galera-cluster" , 
                      :ip =>  "192.168.200.#{ i + 10 }"
    end
  end
  config.vm.provision :ansible do |ansible|
    ansible.groups = groups
    ansible.playbook = "provisioning/playbook.yml"
  end
end