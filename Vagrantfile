# -*- mode: ruby -*-
# vi: set ft=ruby :


Vagrant.configure("2") do |config|


    config.vm.box = "lourranio/ubuntu-20.04"
    config.vm.box_version = "1"
    
    config.vm.network "public_network", ip: "192.168.0.64"
    config.vm.hostname = "k8s-master-santos"
    #config.vm.network "public_network"
  
   config.vm.provider "virtualbox" do |vb|
    #   # Display the VirtualBox GUI when booting the machine
    #   vb.gui = true
    #
    #   # Customize the amount of memory on the VM:
       vb.memory = "2100"
       vb.cpus = 2
     end
        
  
      if Vagrant.has_plugin?('vagrant-hostmanager')
          config.hostmanager.enabled = true
          config.hostmanager.manage_host = true # => Manages local machine's /etc/hosts
          config.hostmanager.ignore_private_ip = false
          config.hostmanager.include_offline = true
          # => config.hostmanager.aliases = ["#{config.vm.hostname}.bdwyertech.net"]
        end
    
        if Vagrant.has_plugin?("vagrant-vbguest") then
            config.vbguest.auto_update = false
        end
     
  
      config.vm.provision "shell", inline: <<-SHELL
      echo " --------- "
      sudo apt install unzip
      SHELL
    
      config.vm.post_up_message = "*******  Tudo instalado   *******"
  
    end
    