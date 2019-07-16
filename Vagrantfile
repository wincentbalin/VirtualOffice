# -*- mode: ruby -*-
# vi: set ft=ruby :

# Configure Vagrant environment with desktop environment, office environment and remote access
#
# Copyright 2019 Wincent Balin
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

Vagrant.configure("2") do |config|
  config.vbguest.auto_update = false
  config.vm.box = "ubuntu/bionic64"
  config.vm.network "forwarded_port", guest: 3389, host: 33389, protocol: "tcp", auto_correct: true
  config.vm.synced_folder ".", "/home/vagrant/Documents", mount_options: ["dmode=775,fmode=664"]
  config.vm.provider "virtualbox" do |vb|
    vb.gui = false
    vb.cpus = "1"
    vb.memory = "8192"
    vb.customize ["modifyvm", :id, "--ioapic", "on"]
  end

  config.vm.provision "shell", inline: <<-SHELL
    apt-get update
    DEBIAN_FRONTEND=noninteractive apt-get -y upgrade
    DEBIAN_FRONTEND=noninteractive apt-get -y install xubuntu-desktop libreoffice xrdp
    DEBIAN_FRONTEND=noninteractive apt-get -y clean
    systemctl enable xrdp
    ufw allow 3389
    echo "xfce4-session" > ~vagrant/.xsession
    chown vagrant.vagrant ~vagrant/.xsession
SHELL
end
