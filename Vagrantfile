# -*- mode: ruby -*-
# vi: set ft=ruby :
#
# Vagrantfile docs: https://docs.vagrantup.com

ansible_roles = [
]
vagrant_plugins = [
  'vagrant-triggers',
]

ansible_roles.each do |role|
  system("ansible-galaxy install #{role}")
end

vagrant_plugins.each do |plugin|
  system("vagrant plugin list | grep -q #{plugin} || vagrant plugin install #{plugin}")
end

Vagrant.configure("2") do |config|

  # for more boxes see https://vagrantcloud.com/search
  config.vm.box = "centos/7"
  config.vm.hostname = "groundcontrol.painless.software"
  config.vm.network :forwarded_port, host: 8080, guest: 80, auto_correct: true
  config.vm.network :forwarded_port, host: 8443, guest: 443, auto_correct: true
  config.vm.post_up_message = "Groundcontrol virtual machine is ready
    The Foreman:
        https://127.0.0.1:8443/
    Control commands:
        vagrant ssh    ..... enter VM
        vagrant halt   ..... stop VM
        vagrant destroy  ... remove VM
    "
  config.vm.provider :virtualbox do |vb|
    vb.customize ["modifyvm", :id, "--name", "Groundcontrol Genesis"]
    vb.customize ["modifyvm", :id, "--memory", "1024"]
    vb.customize ["modifyvm", :id, "--vram", "16"]
    vb.customize ["modifyvm", :id, "--ioapic", "on"]
    vb.customize ["modifyvm", :id, "--cpus", "4"]
    vb.gui = false  # show VirtualBox Manager when starting up
  end

  # clean up on destroy
  config.trigger.after :destroy do
    run "rm -rf .vagrant"
  end

  config.vm.provision "ansible" do |ansible|
    ansible.compatibility_mode = "2.0"
    ansible.playbook = "ansible/playbook.yml"
  end
end
