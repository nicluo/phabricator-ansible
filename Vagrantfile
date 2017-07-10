# -*- mode: ruby -*-
# vi: set ft=ruby :

ANSIBLE_PATH = __dir__

require 'yaml'
vconfig = YAML.load_file("#{ANSIBLE_PATH}/vagrant.default.yml")

Vagrant.configure("2") do |config|
  config.vm.box = vconfig.fetch('vagrant_box')

  # Fix for: "stdin: is not a tty"
  # https://github.com/mitchellh/vagrant/issues/1673#issuecomment-28288042
  config.ssh.shell = %{bash -c 'BASH_ENV=/etc/profile exec bash'}

  config.vm.provider "virtualbox" do |vb|
    vb.name = vconfig.fetch('vagrant_hostname')
    vb.cpus = vconfig.fetch('vagrant_cpus')
    vb.memory = vconfig.fetch('vagrant_memory')
    vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    vb.customize ["modifyvm", :id, "--ioapic", "on"]
  end

  config.vm.network :private_network, ip: vconfig.fetch('vagrant_ip')
  config.vm.hostname = vconfig.fetch('vagrant_hostname')
  config.hostmanager.aliases = vconfig.fetch('vagrant_alt_file_domain')

  # Set the name of the VM. See: http://stackoverflow.com/a/17864388/100134
  config.vm.define :phabricator do |phabricator|
  end

  if Vagrant.has_plugin?('vagrant-hostmanager')
    config.hostmanager.enabled = true
    config.hostmanager.manage_host = true
  else
    fail_with_message "vagrant-hostmanager missing, please install the plugin with this command:\nvagrant plugin install vagrant-hostmanager"
  end

  config.vm.provision "shell" do |s|
    s.inline = "apt install -y python"
  end

  config.vm.provision "ansible" do |ansible|
    ansible.verbose = "v"
    ansible.playbook = "playbook.yml"
    ansible.inventory_path = "inventory"
    ansible.sudo = true
  end
end
