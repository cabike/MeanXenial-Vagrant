# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  
  config.vm.box = "cabike/MeanXenial"

  config.vm.post_up_message = "You can now vagrant ssh in and install whatever generators you need with yeoman"
  
  config.vm.provider "virtualbox" do |v| 
    v.memory = 4096 
    v.cpus = 4 
    v.customize [ "modifyvm", :id, "--uartmode1", "disconnected" ] 
  end

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine and only allow access
  # via 127.0.0.1 to disable public access
  #MongoDB
  config.vm.network "forwarded_port", guest: 27017, host: 27017, host_ip: "127.0.0.1"
  #Node.js
  config.vm.network "forwarded_port", guest: 3000, host: 3000, host_ip: "127.0.0.1"

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  config.vm.network "private_network", ip: "192.168.33.10"

  # Set up  a synced folder
  config.vm.synced_folder "/", "/vagrant/myMeanApp"

  #Let the box update and upgrade
  config.vm.provision "shell", inline: <<-SHELL
    sudo apt-get update
    sudo apt-get upgrade -y
  SHELL
end
