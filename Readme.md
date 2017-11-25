
# openvpn_standalone

## Description

A set of ansible playbooks ran by Vagrant to setup two debian boxes: one running a openvpn server and one running a client.


## Usage: 

First clone the git repo:

    git clone https://github.com/kevinwaro/openvpn_standalone.git && cd openvpn_standalone

Then let Vagrant play the music:

    vagrant up
 
It's possible to add as many boxes as you want. Just be sure to run the right playbook on it. 

By default, the assigned addressed are via DHCP. But it's also possible to assign fixed ones. you will just need to add the following lines to the Vagrantfile in the client's ansible.extra_vars section like as followed for a box called 'client1' :

     config.vm.define "client1" do |client1|
       client1.vm.box ="debian/jessie64" 
       client1.vm.network "private_network", ip: "192.168.200.11"
       client1.vm.hostname = "client1"
       client1.vm.provider "virtualbox" do |vb|
         vb.memory = "512"
         vb.cpus = 1
       end
       client1.vm.provision "ansible/client1", type: "ansible" do |ansible|
         ansible.playbook = "ansible/openvpn_client.yml"
         ansible.verbose = "v"
         ansible.extra_vars = {
             "target" => "client1", 
             "ip_address" => "{{ ansible_eth1.ipv4.address }}",
             "hostname" => "{{ ansible_hostname }}",
             "ca_cert" => "ca.crt", 
             "client_cert" => "openvpnclient.local.crt",
             "client_key" => "openvpnclient.local.key",
             "vpn_client_vpn_ip" => "172.16.200.10",
         }
        end
     end

## Credits:

* author: kevinwaro 
* contact: kevinwaro@yahoo.fr

## Licence

Copyright © 2017 kevinwaro <kevinwaro@yahoo.fr>
This work is free. You can redistribute it and/or modify it under the
terms of the Do What The Fuck You Want To Public License, Version 2,
as published by Sam Hocevar. See the COPYING file for more details.
