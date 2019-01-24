# -*- mode: ruby -*-
# vi: set ft=ruby :

# Basic Setup
Vagrant.configure("2") do |config|
  config.vm.box = "bento/ubuntu-16.04"
  config.vm.hostname = "ansible1"
  config.vm.network "private_network", ip: "10.0.0.6"
  #config.vm.network "private_network", type: "dhcp"  
  
  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  config.vm.provider "virtualbox" do |vb|
	vb.name = "vagrant-ansible1"
  end

############################################################
  # Oh My ZSH Install section

  # Install git and zsh prerequisites 
  config.vm.provision "shell", inline: "apt-get update && apt-get -y install git zsh"

  config.vm.provision "shell", privileged: false, inline:
  <<-SHELL
    echo "Installing ZShell"
    git clone https://github.com/robbyrussell/oh-my-zsh.git ~/.oh-my-zsh
    cp ~/.oh-my-zsh/templates/zshrc.zsh-template ~/.zshrc
    sed -i 's/robbyrussell/agnoster/g' ~/.zshrc
  SHELL
  # Change the vagrant user's shell to use zsh
  config.vm.provision "shell", inline: "chsh -s /bin/zsh vagrant"

############################################################
  # Ansible Installation section (Requires Ubuntu 16.04+)

  config.vm.provision "shell", inline:
  <<-SHELL
    sudo apt-get update
    sudo apt-get install -y \
      build-essential \
      libpq-dev \
      libssl-dev \
      openssl \
      libffi-dev \
      zlib1g-dev \
      software-properties-common \
      sshpass \
      python3-pip \
      python3-venv
    sudo apt-add-repository --yes --update ppa:ansible/ansible
    sudo apt-get update
    sudo apt-get install -y ansible
    yes | pip3 install --upgrade setuptools 
    yes | pip3 install pywinrm    
  SHELL
  config.vm.provision "shell", privileged: false,
    inline: "git clone https://github.com/fervartel/ansible.git"
############################################################
end
