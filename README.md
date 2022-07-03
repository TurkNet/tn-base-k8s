# TurkNet K8s Cluster

## Requirements

* [Vagrant](https://www.vagrantup.com/downloads)
    * [vagrant plugin install vagrant-hostsupdater](https://github.com/agiledivider/vagrant-hostsupdater)
* [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)
* [Virtualbox(If your processor is x86-64 or AMD based.)](https://www.virtualbox.org/wiki/Downloads)
* [Parallels(If your processor is ARM based. For example, if you have a computer with the Apple M1 series)](https://www.parallels.com/eu/products/desktop/trial/)


## Init cluster
```bash
vagrant up
```

## Shutdown cluster
```bash
vagrant halt
```

## Destroy cluster
```bash
vagrant destroy -f
```

## Get KUBECONFIG
```bash
vagrant ssh master -c "sudo cat ~/.kube/config" > ~/.kube/turknet-base-k8s
```

## Export KUBECONFIG
```bash
export KUBECONFIG=~/.kube/turknet-base-k8s
```
