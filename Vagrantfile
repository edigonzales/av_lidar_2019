# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://atlas.hashicorp.com/search.
  config.vm.box = "bento/ubuntu-18.04"

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false
  config.vm.hostname = "lidar-produkte-2019"
  config.ssh.forward_agent = true
  config.ssh.forward_x11 = true

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # config.vm.network "forwarded_port", guest: 80, host: 8080
  config.vm.network "forwarded_port", guest: 22, host: 2029, id: 'ssh'
  
  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"
  config.vm.network "private_network", ip: "192.168.50.9"
  
  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  config.vm.synced_folder "/Volumes/LiDAR2019/", "/lidar2019", :mount_options => ["ro"]
  config.vm.synced_folder "/Volumes/Samsung_T5/", "/Samsung_T5"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
    # Customize the amount of memory on the VM:
    vb.memory = "8096"
    vb.cpus = 4
    vb.customize ["modifyvm", :id, "--cableconnected1", "on"]
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
  config.vm.provision :docker
  config.vm.provision "shell", inline: <<-SHELL
    locale-gen de_CH.utf8
    echo "Europe/Zurich" | tee /etc/timezone
    dpkg-reconfigure --frontend noninteractive tzdata
    echo 'deb http://apt.postgresql.org/pub/repos/apt/ bionic-pgdg main' >> /etc/apt/sources.list.d/pgdg.list
    wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | apt-key add -    
    apt-get update
    add-apt-repository --yes ppa:ubuntugis/ubuntugis-unstable
    apt-key adv --keyserver hkp://keyserver.ubuntu.com:80/ --recv-key 073D307A618E5811
    apt-get update
    apt-get upgrade
    apt-get install -y gdal-bin python-gdal
    apt-get install -y xauth zip
    apt-get install -y ifupdown
    apt-get install -y fonts-liberation
    #echo 'deb     https://qgis.org/ubuntu bionic main' | tee /etc/apt/sources.list.d/qgis.list
    #apt-key adv --keyserver keyserver.ubuntu.com --recv-key CAEB3DC3BDF7FB45
    #apt-get update
    #apt-get install -y qgis python-qgis 
    apt-get install -y zip unzip
    SHELL
end
