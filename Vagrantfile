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
    display.vm.network "forwarded_port", guest: 5000, host: 5000
    display.vm.network "private_network", ip: "192.168.50.20"
    display.vm.provision "shell", name: "basic_provision", inline: <<-SHELL
         apt-get update
         apt-get install -y python3-pip
         apt-get install -y nginx
         apt-get install -y git
    SHELL
    display.vm.provision "shell", name: "basic_provision", privileged: false, inline: <<-SHELL
        pip3 install pipenv
        pip3 install python-dotenv
      SHELL

      display.vm.provision "shell", name: "basic_provision", inline: <<-SHELL
        mkdir -p /var/www/app
        chown -R $USER:www-data /var/www/app
        chmod -R 775 /var/www/app
        cp /vagrant/.env /var/www/app
        cd /var/www/app
        pipenv shell
        pipenv install flask gunicorn
        cp -r /vagrant/app /var/www/app
        systemctl start nginx
        cp -r /vagrant/flask_app.service /etc/systemd/system/flask_app.service
        systemctl daemon-reload
        systemctl enable flask_app
        systemctl start flask_app
        cp -r /vagrant/app.conf /etc/nginx/sites-available/app.conf
        ln -s /etc/nginx/sites-available/app.conf /etc/nginx/sites-enabled/
        cp /vagrant/hosts /etc/hosts

        cd /var/www
        sudo git clone https://github.com/Azure-Samples/msdocs-python-flask-webapp-quickstart
        chown -R $USER:www-data /var/www/msdocs-python-flask-webapp-quickstart/
        chmod -R 775 /var/www/msdocs-python-flask-webapp-quickstart/

        cd /var/www/msdocs-python-flask-webapp-quickstar
        pipenv shell

  SHELL
  end #display

  
end
