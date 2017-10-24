#Chef 12 on VirtualBox and Vagrant 
Spin up VirtualBox VM and install Chef server 12 in it using Vagrant and Ansible

##Install Virtual Box on MAC:

http://download.virtualbox.org/virtualbox/5.1.30/VirtualBox-5.1.30-118389-OSX.dmg

##Install Vagrant:

https://releases.hashicorp.com/vagrant/2.0.0/vagrant_2.0.0_x86_64.dmg


##Install Ubuntu/Xenial 16.04 Virtual VM using Vagrant:

```
$ mkdir my_vagrant
$ cd my_vagrant
$ vagrant init
$ vagrant box add ubuntu/xenial64
==> box: Loading metadata for box 'ubuntu/xenial64'
    box: URL: https://vagrantcloud.com/ubuntu/xenial64
==> box: Adding box 'ubuntu/xenial64' (v20171011.0.0) for provider: virtualbox
    box: Downloading: https://vagrantcloud.com/ubuntu/boxes/xenial64/versions/20171011.0.0/providers/virtualbox.box
==> box: Successfully added box 'ubuntu/xenial64' (v20171011.0.0) for 'virtualbox'!
```
Check if Vagrant Box was downloaded
```
$ vagrant box list
ubuntu/xenial64 (virtualbox, 20171011.0.0)
```

##Install Ansible on MAC (where vagrant / virtualbox is running):

###Install Homebrew on MAC:
```
$ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```
###Install ansible:
```
$ brew install ansible
```
##Modify Vagrantfile to add ansible:

```
$ egrep -v "^$|^#| #" Vagrantfile 
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/xenial64"
    config.vm.provider "virtualbox" do |vb|
      vb.gui = true
   
      vb.memory = "2048"
    end
    config.vm.provision :shell, path: "install_ansible.sh"
    config.vm.provision "ansible" do |p|
      p.playbook = "playbook.yml"
      p.verbose        = true
    end
    config.vm.network "forwarded_port", guest: 80, host: 8080, id: "chef"
    config.vm.network "forwarded_port", guest: 443, host: 8443, id: "chefssl"
end
```
##Boot up VM and Install Chef 12 in it using Vagrant

```
$ vagrant up
```

Once The VM has finished configuring with Chef you may connect to it using Chef-Manage UI on 
https://localhost:8443 from your Mac.

