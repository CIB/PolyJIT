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
  config.vm.box = "bheegu1/xenial-64"

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  config.vm.synced_folder "polli/", "/polli"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  config.vm.provider "virtualbox" do |vb|
     # Display the VirtualBox GUI when booting the machine
     # vb.gui = true
  
     # Customize the amount of memory on the VM:
     vb.memory = "4096"
     vb.cpus = 4
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

  # Install likwid
  config.vm.provision "shell", inline: <<-SHELL
    sudo apt-get update
    sudo apt-get install -y make gcc g++
    
    mkdir -p /tmp/likwid/
    cd /tmp/likwid/
    wget http://ftp.fau.de/pub/likwid/likwid-4.2.0.tar.gz
    tar zxvf likwid-4.2.0.tar.gz
    cd likwid-4.2.0
    make
    sudo make install
  SHELL
  
  # Install papi
  config.vm.provision "shell", inline: <<-SHELL
    cd /tmp/
    mkdir papi
    cd papi/
    wget http://icl.utk.edu/projects/papi/downloads/papi-5.5.1.tar.gz
    tar zxvf papi-5.5.1.tar.gz 
    cd papi-5.5.1/src/
    ./configure
    make
    sudo make install
  SHELL
  
  # Install clang and ccache into system
  config.vm.provision "shell", inline: <<-SHELL
    sudo apt-get update
    sudo apt-get install -y clang ccache ninja-build
    
    
    # Install dependencies
    sudo add-apt-repository -y ppa:jtv/ppa
    sudo apt-get update
    sudo apt-get install -y libboost-dev libboost-system-dev libboost-thread-dev libpqxx-dev libisl-dev
  
	# Install newer cmake version than ubuntu LTS supports (meh)
	cd /tmp/
	wget https://cmake.org/files/v3.8/cmake-3.8.2.tar.gz
	tar xzvf cmake-3.8.2.tar.gz
	cd cmake-3.8.2
	
	./bootstrap
	make -j4
	make install
  SHELL
  
  # Install llvm+polly into mounted dir
  config.vm.provision "shell", inline: <<-SHELL
    apt-get update
    apt-get install -y git
    cd /polli
    git clone https://github.com/PolyJIT/PolyJIT.git
    cd PolyJIT
    mkdir build
    cd build
    
    cmake -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=/polli/install/ -G Ninja ..
    ninja -j1
    ninja install
    
    # Make build accessible without root
    chown -R vagrant /polli
  SHELL

end

# 
