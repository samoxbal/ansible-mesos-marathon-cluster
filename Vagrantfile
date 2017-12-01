# -*- mode: ruby -*-
# vi: set ft=ruby :

groups = {
    "master" => ["master1", "master2", "master3"],
    "nodes" => ["node1","node2","node3"],
    "all_groups:children" => ["master", "nodes"]
}

cluster = {
    "master1" => { :ip => "192.168.33.10", :cpus => 1, :mem => 1024 },
    "master2" => { :ip => "192.168.33.11", :cpus => 1, :mem => 1024 },
    "master3" => { :ip => "192.168.33.12", :cpus => 1, :mem => 1024 },
    "node1" => { :ip => "192.168.33.20", :cpus => 1, :mem => 1024 },
    "node2" => { :ip => "192.168.33.21", :cpus => 1, :mem => 1024 },
    "node3" => { :ip => "192.168.33.22", :cpus => 1, :mem => 1024 }
}

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  config.hostmanager.enabled = true
  config.hostmanager.manage_host = true
  config.hostmanager.manage_guest = true
  config.hostmanager.ignore_private_ip = false
  config.hostmanager.include_offline = true

  config.vm.box = "ubuntu/trusty64"

  config.vm.provider "virtualbox" do |v|
    v.memory = 1536
    v.cpus = 1
  end

  cluster.each_with_index do |(hostname, info), index|
    config.vm.define hostname do |machine|
      machine.vm.hostname = hostname
      machine.vm.network "private_network", ip: "#{info[:ip]}"
      machine.hostsupdater.aliases = [hostname]

      machine.vm.provision "ansible" do |ansible|
        ansible.limit = "all"
        ansible.sudo = true
        ansible.playbook = "ansible/vagrant.yml"
        ansible.groups = groups
        ansible.extra_vars = {
            zoo1: "192.168.33.10",
            zoo2: "192.168.33.11",
            zoo3: "192.168.33.12",
            zoo_myid: index + 1
        }
    end
  end
end
end
