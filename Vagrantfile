# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

NODE_IP = "10.0.0.10"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  boxes = [
    { :name => "node",      :ip => NODE_IP,      :cpus => 2,  :memory => 4096  }
  ]

  config.vm.box = "turknet-devops/ubuntu-20.04"
  config.vm.box_check_update = true

  boxes.each do |opts|
    config.vm.define opts[:name] do |box|
      box.vm.hostname = opts[:name]
      box.vm.network "private_network", ip: opts[:ip]

      box.vm.provider "virtualbox"  do |vb|
        vb.cpus = opts[:cpus]
        vb.memory = opts[:memory]
      end

      box.vm.provider "parallels"  do |prl|
        prl.cpus = opts[:cpus]
        prl.memory = opts[:memory]
      end
      
      config.vm.provision "ansible" do |ansible|
        ansible.playbook = "playbook.yml"
      end
    end
  end
end
