# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure(2) do |config|
    # The most common configuration options are documented and commented below.
    # For a complete reference, please see the online documentation at
    # https://docs.vagrantup.com.

    # Every Vagrant development environment requires a box. You can search for
    # boxes at https://atlas.hashicorp.com/search.
    config.vm.box = "ubuntu/trusty64"


    # vagrant cachier plugin
    if Vagrant.has_plugin?("vagrant-cachier")
        # Configure cached packages to be shared between instances of the same base box.
        # More info on http://fgrehm.viewdocs.io/vagrant-cachier/usage
        config.cache.scope = "box"
        # For more information please check http://docs.vagrantup.com/v2/synced-folders/basic_usage.html
    end

    # Vagrant vagrant-vbguest plugin
    # set auto_update to false, if you do NOT want to check the correct
    # additions version when booting this machine
    config.vbguest.auto_update = true

    # Disable automatic box update checking. If you disable this, then
    # boxes will only be checked for updates when the user runs
    # `vagrant box outdated`. This is not recommended.
    # config.vm.box_check_update = false

    # Create a forwarded port mapping which allows access to a specific port
    # within the machine from a port on the host machine. In the example below,
    # accessing "localhost:8080" will access port 80 on the guest machine.
    # config.vm.network "forwarded_port", guest: 8080, host: 80
    # config.vm.network "forwarded_port", guest: 5432, host: 5432
    # config.vm.network "forwarded_port", guest: 8080, host: 8080
    # config.vm.network "forwarded_port", guest: 8000, host: 8000
    # config.vm.network "forwarded_port", guest: 1337, host: 1337

    # Create a private network, which allows host-only access to the machine
    # using a specific IP.
    config.vm.network "private_network", ip: "192.168.20.128"

    # Create a public network, which generally matched to bridged network.
    # Bridged networks make the machine appear as another physical device on
    # your network.
    # config.vm.network "public_network"

    # Share an additional folder to the guest VM. The first argument is
    # the path on the host to the actual folder. The second argument is
    # the path on the guest to mount the folder. And the optional third
    # argument is a set of non-required options.
    # config.vm.synced_folder "../data", "/vagrant_data"

    config.vm.synced_folder ".", "/vagrant.sync"

    # Provider-specific configuration so you can fine-tune various
    # backing providers for Vagrant. These expose provider-specific options.
    # Example for VirtualBox:
    #
    config.vm.provider "virtualbox" do |vb|
        # Display the VirtualBox GUI when booting the machine
        # vb.gui = true
        # Customize the amount of memory on the VM:
        # vb.memory = "2048"
        # vb.cpus = 2
    end


    #
    # View the documentation for the provider you are using for more
    # information on available options.

    # Define a Vagrant Push strategy for pushing to Atlas. Other push strategies
    # such as FTP and Heroku are also available. See the documentation at
    # https://docs.vagrantup.com/v2/push/atlas.html for more information.
    # config.push.define "atlas" do |push|
    #   push.app = "YOUR_ATLAS_USERNAME/YOUR_APPLICATION_NAME"
    # end

    # Enable provisioning with a shell script. Additional provisioners such as
    # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
    # documentation for more information about their specific syntax and use.
    # config.vm.provision "shell", inline <<-SHELL
    #   sudo apt-get install apache2
    # SHELL

    # install docker
    config.vm.provision "docker", images: ["ubuntu", "colch/devbox:master", "postgres:9.4.0"]

    # make dev dir
    config.vm.provision "shell", inline: <<-SHELL
        echo "Set encoding to CP-1251"
        localedef  -c -i en_US -f CP1251 en_US.CP1251
        echo "LANG=en_US.CP-1251" > /etc/default/locale
    SHELL

    # make dev dir
    config.vm.provision "shell", inline: <<-SHELL
    echo "Creating dev div"
        rm -rf /vagrant
        mkdir -m 777 /vagrant
        chown -R vagrant /vagrant
        chgrp -R vagrant /vagrant
    SHELL

    # apt-get update
    config.vm.provision "shell", inline: <<-SHELL
        apt-get update -q=2
    SHELL

    # install docker-compose
    config.vm.provision "shell", inline: <<-SHELL
        curl -s -L https://github.com/docker/fig/releases/download/1.1.0-rc1/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose; chmod +x /usr/local/bin/docker-compose
    SHELL

    # install htop
    config.vm.provision "shell", inline: <<-SHELL
        apt-get install -q=2 htop
    SHELL

    # export env
    config.vm.provision "shell", inline: <<-SHELL
        echo "Setting ENV now"
        echo '[ -f /vagrant.sync/.env-dev ] && source /vagrant.sync/.env-dev' > /etc/profile.d/vagrant.sh
        chmod +x /etc/profile.d/vagrant.sh
    SHELL

    # install unison
    config.vm.provision "shell", inline: <<-SHELL
        apt-get install -q=2 unison
    SHELL

    # copy unison config
    config.vm.provision "shell", inline: <<-SHELL
        echo "Creating unison config"
        rm -rf /home/vagrant/.unison
        mkdir -m 777 /home/vagrant/.unison
        chown -R vagrant /home/vagrant/.unison
        chgrp -R vagrant /home/vagrant/.unison
        cp /vagrant.sync/unison-config.prf /home/vagrant/.unison/default.prf
    SHELL

    end
