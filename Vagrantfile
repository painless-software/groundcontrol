# -*- mode: ruby -*-
# vi: set ft=ruby :
#
# Vagrantfile docs: https://docs.vagrantup.com

# ensure Ansible plugins are installed
['nickjj.docker'].each do |role|
  system("ansible-galaxy install #{role}")
end

Vagrant.configure("2") do |config|

  # for more boxes see https://vagrantcloud.com/search
  config.vm.box = "ubuntu/xenial64"

  config.vm.provision "ansible" do |ansible|
    ansible.compatibility_mode = "2.0"
    ansible.extra_vars = { ansible_python_interpreter: "/usr/bin/python3" }
    ansible.playbook = "playbook.yml"
  end
end
