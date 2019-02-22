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
        && sudo cp /etc/paths /etc/paths.orig \
        && echo "/usr/local/sbin" | cat - /etc/paths > pathtemp1 \
        && echo "/usr/local/bin" | cat - pathtemp1 > pathtemp2 \
        && sudo mv pathtemp2 /etc/paths && rm pathtemp1 \
        && PATH="/usr/local/bin:/usr/local/sbin:${PATH}" \
        && export PATH \
        && echo "PATH=${PATH}";
      [ ! -f /usr/local/bin/brew ] && echo "Installing Tigerbrew..." && ruby \
        -e "$(curl -fsSkL raw.github.com/mistydemeo/tigerbrew/go/install)" \
        </dev/null \
      || echo "Tigerbrew is already installed...";
      brew doctor;
      echo "Installing and linking curl...";
      brew install curl && brew link --force curl || \
        echo "!!!! This somehow only works on the 2nd try, retrying... !!!!!" \
        && brew install curl && brew link --force curl \
        || echo "!!!! Unable to install curl !!!!!";
      echo "Building git from source...";
      brew install --build-from-source git;
      brew update;
      [ ! -f /Users/vagrant/python-2.7.pkg ] \
      && echo "Downloading Python 2.7.15..." \
      && curl -sSo /Users/vagrant/python-2.7.pkg https://www.python.org/ftp/python/2.7.15/python-2.7.15-macosx10.6.pkg;
      [ ! -f /usr/local/bin/python ] && echo "Installing Python 2.7.15..." \
      && sudo installer -pkg /Users/vagrant/python-2.7.pkg -target /;
      [ ! -f /Users/vagrant/python-3.7.pkg ] \
      && echo "Downloading Python 3.7.2..." \
      && curl -o /Users/vagrant/python-3.7.pkg https://www.python.org/ftp/python/3.7.2/python-3.7.2-macosx10.6.pkg;
      [ ! -f /usr/local/bin/python3 ] && echo "Installing Python 3.7.2..." \
      && sudo installer -pkg /Users/vagrant/python-3.7.pkg -target /;
      echo "MacOS10.7 Bootstrap successful!"
      SCRIPT
    macos7.vm.provision "bootstrap_user", type: "shell", preserve_order: true, privileged: false, inline: $script
  end


  config.vm.provider "virtualbox" do |vb|
    # Customize the amount of memory on the VM:
    vb.memory = "4096"
  end

  config.vm.provision "bootstrap", type: "shell", preserve_order: true, inline: ""
  config.vm.provision "bootstrap_user", type: "shell", preserve_order: true, privileged: false, inline: ""
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

      "macos" => [
        "macos7",
      ],
      "macos:vars" => {
        "ansible_python_interpreter" => "/usr/local/bin/python",
      },
    }

    ansible.playbook = "vagrant-playbook.yml"
  end
end
