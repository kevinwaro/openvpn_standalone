# -*- mode: ruby -*-
Vagrant.configure(2) do |config|
 config.vm.define "hostname" do |client|
   puppet.vm.box ="debian/jessie64" 
   puppet.vm.network "private_network", ip: "192.168.200.10"
   puppet.vm.hostname = "client"
   puppet.vm.provider "virtualbox" do |vb|
     vb.memory = "512"
     vb.cpus = 1
   end

 config.vm.define "hostname" do |server|
   puppet.vm.box ="debian/jessie64" 
   puppet.vm.network "private_network", ip: "192.168.200.11"
   puppet.vm.hostname = "server"
   puppet.vm.provider "virtualbox" do |vb|
     vb.memory = "512"
     vb.cpus = 1
   end

end
