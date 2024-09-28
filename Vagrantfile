# -*- mode: ruby -*-
# vi: set ft=ruby :
$ip_vm1 = "172.20.0.10"
$ip_vm3 = "172.20.0.11"

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/jammy64"
  config.vm.box_check_update = false

  config.vm.define "vm1" do |vm1|
    vm1.vm.hostname = "vm1"

    vm1.vm.network "forwarded_port", guest: 80, host: 8080
    vm1.vm.network "private_network", ip: $ip_vm1

    vm1.vm.provider "virtualbox" do |vb|
      vb.name = "vm1"
    end

    vm1.vm.provision "shell", inline: <<-SHELL
      sudo apt-get update
      sudo apt-get upgrade -y
      sudo apt-get install nginx -y
      sudo systemctl start nginx
      sudo systemctl enable nginx
    SHELL
  end

  config.vm.define "vm2" do |vm2|
    vm2.vm.hostname = "vm2"

    vm2.vm.disk :disk, size: "60GB", primary: true

    vm2.vm.network "public_network"

    vm2.vm.provider "virtualbox" do |vb|
      vb.name = "vm2"
    end

    vm2.vm.provision "shell", inline: <<-SHELL
      sudo apt-get update
      sudo apt-get upgrade -y
      curl -fsSL https://get.docker.com -o docker.sh
      sudo sh docker.sh
    SHELL
  end

  config.vm.define "vm3" do |vm3|
    vm3.vm.hostname = "vm3"

    vm3.vm.network "private_network", ip: $ip_vm3

    vm3.vm.provider "virtualbox" do |vb|
      vb.name = "vm3"
    end

    vm3.vm.provision "shell", inline: <<-SHELL
      sudo apt-get update
      sudo apt-get upgrade -y
      sudo apt-get install zsh -y
      sudo useradd -m -d /home/adam -s /bin/zsh -G sudo adam
      echo "adam ALL=(ALL) NOPASSWD:ALL" | sudo tee /etc/sudoers
    SHELL
  end
end
