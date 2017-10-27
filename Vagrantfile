# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  # vagrant box name
  config.vm.box = "lupin/debian9"

  # disable automatic box update checking
  config.vm.box_check_update = false

  # disable key change
  config.ssh.insert_key = false

  # shared folder
  config.vm.synced_folder "./src", "/home/vagrant/src"

  # 1st interface NAT with port forwarding
  config.vm.network "forwarded_port", guest: 80, host: 8080, id: 'http'
  config.vm.network "forwarded_port", guest: 1935, host: 1935, id: 'rtmp'

  # 2nd interface hostonly
  # config.vm.network "private_network", type: "dhcp"

  # virtualbox specific configuration
  config.vm.provider "virtualbox" do |vb|
    vb.gui = false
    vb.name = "VideoTest"
    vb.memory = "1024"
    vb.cpus = 1
  end

  # SHELL provisioners
  config.vm.provision "install", type: "shell", inline: <<-SHELL
    sudo apt update && sudo apt -y upgrade
    sudo apt -y install build-essential libpcre3 libpcre3-dev libssl-dev zlib1g zlib1g-dev git
    git clone https://github.com/sergey-dryabzhinsky/nginx-rtmp-module.git
    # git clone https://github.com/arut/nginx-rtmp-module.git
    curl -s -C - -O http://nginx.org/download/nginx-1.12.1.tar.gz
    tar -xf nginx-1.12.1.tar.gz
    cd nginx-1.12.1
    ./configure --with-http_ssl_module --add-module=../nginx-rtmp-module
    sudo make -j 1
    sudo make install
    sudo ln -s /usr/local/nginx/sbin/nginx /usr/bin/nginx
  SHELL

  config.vm.provision "prepare", type: "shell", inline: <<-SHELL
    sudo cp /usr/local/nginx/conf/nginx.conf /usr/local/nginx/conf/nginx.conf.bak
    sudo cp /home/vagrant/src/nginx.conf /usr/local/nginx/conf/nginx.conf
    sudo cp /home/vagrant/src/*.html /usr/local/nginx/html/
    sudo chown root:staff /usr/local/nginx/conf/nginx.conf && \
      sudo chmod 0644 /usr/local/nginx/conf/nginx.conf
    sudo chown -R root:staff /usr/local/nginx/html && \
      sudo find /usr/local/nginx/html -type f | xargs chmod 0644
    sudo mkdir /tmp/hls && \
      sudo chown root:staff /tmp/hls && \
      sudo chmod 0777 /tmp/hls
  SHELL

  config.vm.provision "start", type: "shell", inline: <<-SHELL
    sudo nginx -t && sudo nginx -V
    sudo nginx
  SHELL

  config.vm.provision "stop", type: "shell", inline: <<-SHELL
    sudo nginx -s stop
  SHELL

  config.vm.provision "video", type: "shell", inline: <<-SHELL
    sudo cp /home/vagrant/src/demo_scaled.mp4 /usr/local/nginx/html/
    sudo cp /home/vagrant/src/demo_poster.png /usr/local/nginx/html/
    sudo cp /home/vagrant/src/output.m3u8 /usr/local/nginx/html/
    sudo cp /home/vagrant/src/*.ts /usr/local/nginx/html/
    sudo chown -R root:staff /usr/local/nginx/html && \
      sudo find /usr/local/nginx/html -type f | xargs chmod 0644
  SHELL

end
