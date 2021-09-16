# -*- mode: ruby -*-
# vim: set ft=ruby :

Vagrant.configure(2) do |config|
  # образ системы centos 7
  config.vm.box = "centos/7"

  # ПЕРВАЯ ВИРТУАЛЬНАЯ МАШИНА
  config.vm.define "ansible" do |subconfig|
    # имя виртуальной машины
    subconfig.vm.provider "virtualbox" do |vb|
      vb.name = "ansible"
    end
    # hostname виртуальной машины
    subconfig.vm.hostname = "ansible"
    # настройки сети
    subconfig.vm.network "private_network", ip: "192.168.11.150"
    # установка пакетов
    subconfig.vm.provision "shell", inline: <<-SHELL
      mkdir -p ~root/.ssh; cp ~vagrant/.ssh/auth* ~root/.ssh
      sed -i '65s/PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config
      systemctl restart sshd
    SHELL
    subconfig.vm.provision "shell", path: "server.sh"
    subconfig.vm.provision "file", source: "files/hosts", destination: "$HOME/staging/hosts"
    subconfig.vm.provision "file", source: "files/ansible.cfg", destination: "$HOME/ansible.cfg"
    subconfig.vm.provision "file", source: "files/epel.yml", destination: "$HOME/epel.yml"
    subconfig.vm.provision "file", source: "files/nginx.yml", destination: "$HOME/nginx.yml"
    subconfig.vm.provision "file", source: "files/nginx.conf.j2", destination: "$HOME/nginx.conf.j2"
  end

  # ВТОРАЯ ВИРТУАЛЬНАЯ МАШИНА
  config.vm.define "client" do |subconfig|
    # имя виртуальной машины
    subconfig.vm.provider "virtualbox" do |vb|
      vb.name = "client"
    end
    # hostname виртуальной машины
    subconfig.vm.hostname = "client"
    # настройки сети
    subconfig.vm.network "private_network", ip: "192.168.11.151"
    subconfig.vm.provision "shell", inline: <<-SHELL
      mkdir -p ~root/.ssh; cp ~vagrant/.ssh/auth* ~root/.ssh
      sed -i '65s/PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config
      systemctl restart sshd
    SHELL
  end
  
  # обновление системы (для первой и второй)
  #config.vm.provision "update", type: "shell", inline: "apt-get update && apt-get upgrade -y"
end