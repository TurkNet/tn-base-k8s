# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "turknet-devops/ubuntu-20.04"
  config.vm.box_version = "0.0.5"
  config.vm.box_check_update = true
  config.vm.network :private_network, ip: "192.168.2.10"


  config.vm.provider "virtualbox" || "parallels" do |vm|
    vm.cpus = 2
    vm.memory = "4096"
  end

  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "playbook.yml"
  end
end
