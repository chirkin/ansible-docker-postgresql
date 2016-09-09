# -*- mode: ruby -*-
# vi: set ft=ruby :

ENV['ANSIBLE_ROLES_PATH'] = "../"

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "chirkin/boot2docker-py"
  config.vm.hostname = "ansible-docker-postgresql"

  config.vm.provider "virtualbox" do |v|
    v.customize ["modifyvm", :id, "--cpuexecutioncap", "50"]
    v.customize ["modifyvm", :id, "--memory", "1024"]
  end

  config.vm.synced_folder ".", "/vagrant", disabled: true

  config.vm.define "primary" do |primary|
    primary.vm.network :private_network, ip: "10.44.32.11"
  end

  config.vm.define "standby" do |standby|
    standby.vm.network :private_network, ip: "10.44.32.12"

    standby.vm.provision "ansible" do |ansible|
      ansible.playbook = "tests/vagrant/vagrant.yml"
      ansible.verbose = ""
      ansible.limit = "all"
      ansible.groups = {
        "primary" => ["primary"]
        # "standby" => ["standby"]
      }
    end
  end
end
