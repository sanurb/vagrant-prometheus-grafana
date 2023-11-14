# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  if Vagrant.has_plugin?("vagrant-vbguest")
    config.vbguest.auto_update = false
  end
  config.vm.define :cliente do |cliente|
    config.vm.box = "bento/ubuntu-22.04"
    cliente.vm.network :private_network, ip: "192.168.50.9"
    cliente.vm.hostname = "cliente"
    cliente.vm.provider "virtualbox" do |v|
  end
  end
  config.vm.define :servidor do |servidor|
    config.vm.box = "bento/ubuntu-22.04"
    servidor.vm.network :private_network, ip: "192.168.50.8"
    servidor.vm.hostname = "servidor"
    servidor.vm.provider "virtualbox" do |v|
  end
  end
end
