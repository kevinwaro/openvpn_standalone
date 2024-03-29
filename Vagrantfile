# -*- mode: ruby -*-
Vagrant.configure(2) do |config|

 config.vm.define "server" do |server|
   server.vm.box ="debian/bullseye64"
   server.vm.network "private_network", ip: "192.168.200.10"
   server.vm.hostname = "server"
   server.vm.provider "libvirt" do |vb|
     vb.memory = "512"
     vb.cpus = 1
   end
   server.vm.synced_folder "./", "/vagrant", type: "rsync"
   server.vm.provision "ansible/server", type: "ansible" do |ansible|
     ansible.playbook = "ansible/openvpn_server.yml"
     ansible.verbose = "v"
     ansible.extra_vars = {
         "target" => "server", 
         "ip_address" => "{{ ansible_eth1.ipv4.address }}",
         "hostname" => "{{ ansible_hostname }}",
         "ca_cert" => "ca.crt", 
         "server_cert" => "openvpnserver.local.crt",
         "server_key" => "openvpnserver.local.key",
         "dh_params" => "dh2048.pem"
     }
    end
 end

 config.vm.define "client" do |client|
   client.vm.box ="debian/bullseye64"
   client.vm.network "private_network", ip: "192.168.200.12"
   client.vm.hostname = "client"
   client.vm.provider "libvirt" do |vb|
     vb.memory = "512"
     vb.cpus = 1
   end
   client.vm.synced_folder "./", "/vagrant", type: "rsync"
   client.vm.provision "ansible/client", type: "ansible" do |ansible|
     ansible.playbook = "ansible/openvpn_client.yml"
     ansible.verbose = "v"
     ansible.extra_vars = {
         "target" => "client", 
         "ip_address" => "{{ ansible_eth1.ipv4.address }}",
         "hostname" => "{{ ansible_hostname }}",
         "ca_cert" => "ca.crt", 
         "client_cert" => "openvpnclient.local.crt",
         "client_key" => "openvpnclient.local.key",
         "eth_interface" => "eth0"
     }
    end
 end

end
