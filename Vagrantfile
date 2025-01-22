# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  config.vm.box = "debian/bullseye64"
  config.vm.define "display" do |display|
    display.vm.hostname = "display"
    display.vm.network "forwarded_port", guest: 8080, host: 8080
    #display.vm.network "private_network", ip: "192.168.50.20"
    display.vm.provision "shell", name: "basic_provision", inline: <<-SHELL
        sudo apt-get update
        sudo apt-get install -y python3-pip
        apt-get install -y nginx
    SHELL
    display.vm.provision "shell", name: "basic_provision", privileged: false, inline: <<-SHELL
        pip3 install pipenv
        pip3 install python-dotenv
      SHELL
  end #display
end
