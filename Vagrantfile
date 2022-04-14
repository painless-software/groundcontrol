# -*- mode: ruby -*-
# vi: set ft=ruby :
#
# Vagrantfile docs: https://docs.vagrantup.com
# For more boxes see https://vagrantcloud.com/search

# ENV['VAGRANT_DEFAULT_PROVIDER'] = 'libvirt'

ansible_roles = []
vagrant_plugins = [
  # 'vagrant-libvirt',
  # 'vagrant-mutate',
]

ansible_roles.each do |role|
  system("ansible-galaxy install #{role}")
end

vagrant_plugins.each do |plugin|
  system("vagrant plugin list | grep -q #{plugin} || vagrant plugin install #{plugin}")
end

Vagrant.configure("2") do |config|
  config.vm.define "identity" do |identity|
    identity.vm.box = "generic/centos8"
    identity.vm.hostname = "identity.painless.software"
    identity.vm.network :forwarded_port, host: 8444, guest: 443, auto_correct: true
    identity.vm.post_up_message = "Identity management is ready. FreeIPA: https://127.0.0.1:8444/"
    # identity.vm.provider :libvirt do |libvirt|
    #   libvirt.driver = "kvm"
    #   libvirt.memory = 1024
    #   libvirt.cpus = 1
    identity.vm.provider :virtualbox do |vb|
      vb.customize ["modifyvm", :id, "--name", "Groundcontrol IdentityManagement"]
      vb.customize ["modifyvm", :id, "--memory", "2048"]
      vb.customize ["modifyvm", :id, "--vram", "16"]
      vb.customize ["modifyvm", :id, "--ioapic", "on"]
      vb.customize ["modifyvm", :id, "--cpus", "4"]
    end
    identity.vm.provision "ansible" do |ansible|
      ansible.compatibility_mode = "2.0"
      ansible.playbook = "ansible/playbook-identity.yml"
      ansible.extra_vars = { ansible_python_interpreter: "/usr/bin/python3" }
    end
    identity.vm.synced_folder ".", "/vagrant", disabled: true
  end

  config.vm.define "enc" do |enc|
    enc.vm.box = "generic/centos8"
    enc.vm.hostname = "enc.painless.software"
    enc.vm.network :forwarded_port, host: 8443, guest: 443, auto_correct: true
    enc.vm.post_up_message = "ENC frontend is ready. The Foreman: https://127.0.0.1:8443/"
    # enc.vm.provider :libvirt do |libvirt|
    #   libvirt.driver = "kvm"
    #   libvirt.memory = 1024
    #   libvirt.cpus = 1
    enc.vm.provider :virtualbox do |vb|
      vb.customize ["modifyvm", :id, "--name", "Groundcontrol ENCfrontend"]
      vb.customize ["modifyvm", :id, "--memory", "1024"]
      vb.customize ["modifyvm", :id, "--vram", "16"]
      vb.customize ["modifyvm", :id, "--ioapic", "on"]
      vb.customize ["modifyvm", :id, "--cpus", "4"]
    end
    enc.vm.provision "ansible" do |ansible|
      ansible.compatibility_mode = "2.0"
      ansible.playbook = "ansible/playbook-enc.yml"
      ansible.extra_vars = { ansible_python_interpreter: "/usr/bin/python3" }
    end
    enc.vm.synced_folder ".", "/vagrant", disabled: true
  end
end
