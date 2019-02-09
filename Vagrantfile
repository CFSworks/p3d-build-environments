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

  config.vm.define "fedora" do |fedora|
    fedora.vm.box = "generic/fedora29"
  end

  config.vm.define "alpine" do |alpine|
    alpine.vm.box = "generic/alpine38"
    alpine.vm.provision "bootstrap", type: "shell", preserve_order: true, inline: "apk add python"
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

  config.vm.provision "bootstrap", type: "shell", preserve_order: true, inline: ""
  config.vm.provision :ansible do |ansible|
    ansible.compatibility_mode = "2.0"
    ansible.groups = {
      "windows" => [
        "win7",
      ],
      "windows:vars" => {
        "ansible_winrm_scheme" => "http",
        "ansible_winrm_operation_timeout_sec" => "60",
        "ansible_winrm_read_timeout_sec" => "120",
        "ansible_become_password" => "vagrant",
        "ansible_become_user" => "vagrant",
      },

      "freebsd" => [
        "freebsd11", "freebsd12",
      ],
      "freebsd:vars" => {
        "ansible_python_interpreter" => "/usr/local/bin/python2.7",
      },
    }

    ansible.playbook = "vagrant-playbook.yml"
  end
end
