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
    win7.vm.box = "opentable/win-7-ultimate-amd64-nocm"
    win7.vm.communicator = "winrm"
  end

  config.vm.define "freebsd11" do |freebsd11|
    freebsd11.vm.box = "generic/freebsd11"
  end

  config.vm.define "freebsd12" do |freebsd12|
    freebsd12.vm.box = "generic/freebsd12"
  end

  config.vm.define "macos14" do |macos14|
    macos14.vm.box = "yzgyyang/macOS-10.14"
    macos14.vm.synced_folder ".", "/vagrant", disabled: true
  end

  config.vm.define "macos7" do |macos7|
    macos7.vm.box = "tuupola/osx-lion-10.7-xcode"
    macos7.vm.synced_folder ".", "/vagrant", disabled: true
    $script = <<-SCRIPT
      [ ! -f /etc/paths.orig ] \
        && sudo sed -i.orig '1s;^;/usr/local/sbin\'$'\n;' /etc/paths \
        && sudo sed -i.edit1 '1s;^;/usr/local/bin\'$'\n;' /etc/paths \
        && PATH="/usr/local/bin:/usr/local/sbin::${PATH}" \
        && export PATH;
      [ ! -f /usr/local/bin/brew ] && ruby \
        -e "$(curl -fsSkL raw.github.com/mistydemeo/tigerbrew/go/install)" \
        </dev/null \
      || echo "Tigerbrew is already installed...";
      brew doctor </dev/null;
      brew install curl && brew link --force curl || \
        echo "!!!! Something went wrong during install of curl, retrying... !!!!!" \
        && brew install curl && brew link --force curl \
        || echo "!!!! Unable to install curl !!!!!";
      brew install --build-from-source git;
      brew update;
    SCRIPT
    macos7.vm.provision "bootstrap", type: "shell", privileged: false, inline: $script
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
