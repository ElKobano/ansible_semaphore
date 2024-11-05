# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
    config.vm.provider "virtualbox" do |v|
      v.memory = 8192
      v.cpus = 4
    end
	config.vm.boot_timeout = 300
    config.vm.define "controlnode" do |controlnode|
      controlnode.vm.box = "ubuntu/jammy64"
      controlnode.vm.hostname = "PracticumControl"
      controlnode.vm.network "private_network", ip: "192.168.56.4"
      controlnode.vm.synced_folder "./ansible","/home/vagrant/ansible"
      controlnode.vm.provision "shell", inline: <<-SHELL
        sed -i 's/ChallengeResponseAuthentication no/ChallengeResponseAuthentication yes/#g' /etc/ssh/sshd_config
		rm -rf /etc/ssh/sshd_config.d/*
        service ssh restart
		apt install ansible sshpass -y
		cp ansible/ansible.cfg .ansible.cfg
		chmod 644 .ansible.cfg
      SHELL
    end
    config.vm.define "managednode" do |managednode|
      managednode.vm.box = "ubuntu/jammy64"
      managednode.vm.hostname = "PracticumServer"
      managednode.vm.network "private_network", ip: "192.168.56.5"
      managednode.vm.provision "shell", inline: <<-SHELL
        sed -i 's/ChallengeResponseAuthentication no/ChallengeResponseAuthentication yes/#g' /etc/ssh/sshd_config
		rm -rf /etc/ssh/sshd_config.d/*
        service ssh restart
		apt install ansible -y
      SHELL
    end
   end
