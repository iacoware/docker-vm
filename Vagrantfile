# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  config.vm.box = "ubuntu/trusty64"
  config.vm.box_check_update = false
  config.vm.hostname = "my-docker-vm"

  config.vm.network "private_network", ip: "192.168.91.99"
  config.vm.network "forwarded_port", guest: 27017, host: 27017 #mongo
  config.vm.network "forwarded_port", guest: 6379, host: 6379 #redis data

  config.vm.provider "virtualbox" do |vb|
    vb.name = "my-docker-vm"
    vb.memory = 2048
    vb.cpus = 2

    vb.customize ["modifyvm", :id, "--clipboard", "bidirectional"]
    vb.customize ["modifyvm", :id, "--acpi", "on"]
    vb.customize ["modifyvm", :id, "--ioapic", "on"]
  end

  config.vm.provision "shell", inline: <<-SHELL.gsub(/^ +/, '')
    echo "Install and update basic packeges"
    sudo apt-get update --fix-missing >/dev/null
    sudo apt-get install -y curl wget whois unzip tree \
                            >/dev/null

    echo "Configure system settings"
    sudo timedatectl set-timezone Europe/Rome
    sudo update-locale LANG=en_US.UTF-8 LC_ALL=en_US.UTF-8
    echo "%sudo ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers

    echo "Install Docker and tools"
    sudo -i <<HEREDOC
        if ! docker version &>/dev/null; then
            wget -qO- https://get.docker.com/ | bash
            sudo usermod -aG docker vagrant
        fi
        if ! docker-compose --version &>/dev/null; then
            curl -L https://github.com/docker/compose/releases/download/1.7.1/docker-compose-`uname -s`-`uname -m` \
                    > /usr/local/bin/docker-compose
            chmod +x /usr/local/bin/docker-compose
        fi
    HEREDOC
    echo "That's all, rock on!"
  SHELL
end