# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

MASTER_IP       = "10.0.0.10"

WORKER1_IP      = "10.0.0.11"

WORKER2_IP      = "10.0.0.12"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  boxes = [
    { :name => "master",      :ip => MASTER_IP,      :cpus => 2,  :memory => 4096  },
    { :name => "worker1",     :ip => WORKER1_IP,     :cpus => 1,  :memory => 2048  },
    { :name => "worker2",     :ip => WORKER2_IP,     :cpus => 1,  :memory => 2048  },
  ]

  config.vm.provision "shell", inline: <<-SHELL
      apt-get update -y
      echo "10.0.0.10  master" >> /etc/hosts
      echo "10.0.0.11  worker" >> /etc/hosts
      echo "10.0.0.12  worker" >> /etc/hosts
  SHELL

  config.vm.box = "turknet-devops/ubuntu-20.04"
  config.vm.box_check_update = true

  boxes.each do |opts|
    config.vm.define opts[:name] do |box|
      box.vm.hostname = opts[:name]
      box.vm.network "private_network", ip: opts[:ip]

      box.vm.provider "virtualbox" do |vb|
        vb.cpus = opts[:cpus]
        vb.memory = opts[:memory]
      end
      
      box.vm.provision "shell", path: "scripts/common.sh"
      
      if opts[:name] == "master"
        box.vm.provision "shell", path: "scripts/master.sh"
      else
        box.vm.provision "shell", path: "scripts/worker.sh"
      end
    end
  end
end
