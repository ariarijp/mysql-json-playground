# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  config.vm.box = "ubuntu/trusty64"
  config.vm.network "private_network", ip: "192.168.33.10"

  config.vm.provider "virtualbox" do |vb|
    vb.cpus = 2
    vb.memory = 1024 * 4
  end

  config.vm.provision "shell", privileged: false, inline: <<-SHELL
    sudo apt-get update
    sudo apt-get install -y libaio1 libncurses5-dev cmake build-essential
    wget http://downloads.mysql.com/snapshots/pb/mysql-5.7.7-labs-json/mysql-5.7.7-labs-json.tar.gz
    tar xzf mysql-5.7.7-labs-json.tar.gz 
    cd mysql-5.7.7-labs-json/
    mkdir -p build && cd build/
    cmake .. -DDOWNLOAD_BOOST=1 -DWITH_BOOST=./my_boost
    make -j2
    sudo make install
    sudo groupadd mysql
    sudo useradd -r -g mysql mysql
    cd /usr/local/mysql
    sudo chown -R mysql.mysql .
    sudo bin/mysql_install_db --user=mysql --datadir=/usr/local/mysql/data
    sudo chown -R root .
    sudo chown -R mysql data
    sudo cp support-files/my-default.cnf /etc/my.cnf
    sudo cp support-files/mysql.server /etc/init.d/mysql.server
    sudo /etc/init.d/mysql.server start
  SHELL
end
