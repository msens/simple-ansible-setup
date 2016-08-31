# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "ubuntu/trusty64"

    # ssh settings
  # config.ssh.insert_key = false
  # config.ssh.private_key_path = ["~/.ssh/id_rsa", "~/.vagrant.d/insecure_private_key"]
  # config.vm.provision "file", source: "~/.ssh/id_rsa.pub", destination: "~/.ssh/authorized_keys"
  # config.vm.provision "shell", inline: <<-EOC
  #   sudo sed -i -e "\\#PasswordAuthentication yes# s#PasswordAuthentication yes#PasswordAuthentication no#g" /etc/ssh/sshd_config
  #  sudo service ssh restart
  # EOC

#  config.vm.box = "bkyoung/centos7"
  config.vm.synced_folder ".", "/vagrant"
  config.vm.provider "virtualbox" do |v|
    v.memory = 1024
  end

(1..3).each do |i|
    config.vm.define "target0#{i}" do |node|
      node.vm.hostname = "target0#{i}"
      node.vm.network "private_network", ip: "10.100.199.20#{i}"
#      node.vm.network "forwarded_port", guest: 8080, host: 808#{i}
#     node.vm.provision :shell, path: "bootstrap.sh"

    end
  end




#  config.vm.provision "file", source: "~/.ssh/id_rsa.pub", destination: "~/.ssh/id_rsa.pub"
#  config.vm.provision "shell", inline: "cat ~vagrant/.ssh/id_rsa.pub >> ~vagrant/.ssh/authorized_keys"
#  config.vm.provision :shell, path: "bootstrap.sh"
#  config.vm.define "ansible-target" do |node|
#    node.vm.hostname = "ansible-target"
#     node.vm.provision :shell, path: "bootstrap.sh"
#    node.vm.network "private_network", ip: "10.100.199.200"
#    node.vm.network "forwarded_port", guest: 2376, host: 2376
#    node.vm.network "forwarded_port", guest: 2375, host: 2375
#  end


  if Vagrant.has_plugin?("vagrant-cachier")
    config.cache.scope = :box
  end
end