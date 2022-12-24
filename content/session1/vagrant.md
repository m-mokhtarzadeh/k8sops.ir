---
date: 2022-12-11T15:26:15Z
lastmod: 2022-12-11T15:26:15Z
publishdate: 2022-12-11T15:26:15Z

title: Create Lab with Vagrant
description: Kubernetes Operations Fundamentals
images:
- images/kops.jpg
---

[comment]: <> (# Create Lab with Vagrant)
### Step 0: You need virtualbox installed on your system
### Step 1: Download and install Vagrant
You can download vagrant from hashicorp.  
[Click Here to Go to Download Page](https://developer.hashicorp.com/vagrant/downloads)  

### Step 2: Download k8sops vagrant box
Download box file from following link.  
[Click Here to Go to Start Download](https://s3.ir-thr-at1.arvanstorage.ir/k8sops/package.box)

### Step 3: Import downloded box to box cache
```bash
vagrant box add package.box --name oracle_kube
```

### Step 4(Optional): Verify box imported successfully
```bash
vagrant box list
```

### Step 5: Create instance from box
Create new empty folder for lab and create file with 'Vagrant' name and insert following Content into it.
```ruby
Vagrant.configure("2") do |config|
  config.vm.box = "oracle_kube"

  config.vm.define "k8sops" do |k8sops|
	  k8sops.vm.network "private_network", ip:"192.168.57.50"
	  k8sops.vm.hostname = "k8sops"
	  k8sops.vm.provider "virtualbox" do |vm1|
      vm1.name = "k8sops"
      vm1.memory = "4096"
      vm1.cpus = 2
      vm1.customize ["modifyvm", :id, "--audio", "none"]
    end
  end
end
```
After create file open command prompt and then enter following command.
```bash
vagrant up
```
After successfully machine is up and running, you can use following command to ssh to the machine.
```bash
vagrant ssh
```