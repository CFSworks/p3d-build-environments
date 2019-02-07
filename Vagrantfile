# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  config.vm.define "trusty" do |trusty|
    trusty.vm.box = "ubuntu/trusty64"
  end

  config.vm.define "win7" do |win7|
    win7.vm.box = "ferventcoder/win7pro-x64-nocm-lite"
  end

  config.vm.define "freebsd11" do |freebsd11|
    freebsd11.vm.box = "generic/freebsd11"
  end

  config.vm.define "freebsd12" do |freebsd12|
    freebsd12.vm.box = "generic/freebsd12"
  end



  config.vm.provider "virtualbox" do |vb|
    # Customize the amount of memory on the VM:
    vb.memory = "4096"
  end

  config.vm.provision :ansible do |ansible|
    ansible.compatibility_mode = "2.0"
    ansible.raw_arguments = [
      "-e ansible_winrm_scheme=http",
      "-e ansible_winrm_operation_timeout_sec=60",
      "-e ansible_winrm_read_timeout_sec=120",
      "-e ansible_become_password=vagrant",
      "-e ansible_become_user=vagrant"
    ]

    ansible.playbook = "vagrant-playbook.yml"
  end
end
