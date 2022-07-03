# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "turknet-devops/ubuntu-20.04"
  config.vm.box_version = "0.0.4"
  config.vm.box_check_update = true

  # Define three VMs with static private IP addresses.
  boxes = [
    { :name => "master",  :ip => "192.168.33.71",  :cpus => 2,  :memory => 4096 },
    { :name => "node1",   :ip => "192.168.33.72",  :cpus => 1,  :memory => 2048 },
    { :name => "node2",   :ip => "192.168.33.73",  :cpus => 1,  :memory => 2048 }
  ]

  config.hostsupdater.aliases = {
    '192.168.33.71' => [
      'prometheus.turknet.dev',
      'alertmanager.turknet.dev',
      'grafana.turknet.dev',
      'tekton.turknet.dev'
    ]
  }

  # Provision each of the VMs.
  boxes.each do |opts|
    config.vm.define opts[:name] do |config|
      config.vm.hostname = opts[:name]
      config.vm.network :private_network, ip: opts[:ip]

      config.vm.provider "virtualbox" || "parallels" do |vm|
        vm.cpus = opts[:cpus]
        vm.memory = opts[:memory]
      end

      # Provision all the VMs in parallel using Ansible after last VM is up.
      if opts[:name] == "node2"
        config.vm.provision "ansible" do |ansible|
          ansible.compatibility_mode = "2.0"
          ansible.playbook = "playbook.yml"
          ansible.limit = "all"
          ansible.become = true
          ansible.groups = {
            "kubernetes" => ["master", "node1", "node2"],
            "kubernetes_master" => ["master"],
            "kubernetes_master:vars" => {
              kubernetes_role: "master",
              swapfile_path: "/dev/mapper/vagrant--vg-swap_1",
              kubernetes_apiserver_advertise_address: "192.168.33.71"
            },
            "kubernetes_node" => ["node1", "node2"],
            "kubernetes_node:vars" => {
              kubernetes_role: "node",
              swapfile_path: "/dev/mapper/vagrant--vg-swap_1"
            }
          }
        end
      end
    end
  end
end
