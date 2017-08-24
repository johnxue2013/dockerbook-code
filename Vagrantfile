# -*- mode: ruby -*-

# vi: set ft=ruby :

Vagrant.configure("2") do |config|
    required_plugins = %w( vagrant-vbguest vagrant-disksize )
    required_plugins.each do |plugin|
      system "vagrant plugin install #{plugin}" unless Vagrant.has_plugin? plugin
    end
  
    config.vm.box = "ubuntu/xenial64"
    config.vm.box_check_update = false
    config.vbguest.auto_update = true
  
    if Vagrant.has_plugin?("vagrant-proxyconf")
      config.proxy.http = "http://180.166.223.108:10015/"
      config.proxy.https = "http://180.166.223.108:10015/"
      config.proxy.no_proxy = "127.0.0.1,localhost,.philips.com,.philips.com.cn"
    end

    #config.vm.provision "shell", path: "set-apt-mirror.sh"
  
    config.vm.provision "docker" do |d|
        d.post_install_provision "shell", path: "set-docker-mirror.sh"
    end

    config.vm.define "dockerbook" do | host |
      host.vm.hostname = "dockerbook"
      host.disksize.size = "100GB"
      host.vm.network :private_network, ip: "10.10.10.11"
  
      host.vm.provider "virtualbox" do |v|
        v.name = "dockerbook"
        v.memory = 1024
        v.cpus = 1
      end

      host.vm.provision "shell", path: "provision.sh"

      host.vm.provision "docker" do |d|
        #d.build_image "/vagrant/5/sinatra/redis", args: "-t redis --build-arg http_proxy=$http_proxy --build-arg https_proxy=$https_proxy"
        #d.run "redis", args: "-p 6379:6379"
        #d.build_image "/vagrant/5/sinatra", args: "-t sinatra --build-arg http_proxy=$http_proxy --build-arg https_proxy=$https_proxy"
        #d.run "sinatra", args: "-p 4567:4567 -v /vagrant/5/sinatra/webapp_redis:/opt/webapp --link redis:db"
        d.build_image "/vagrant/5/jenkins", args: "-t jenkins --build-arg http_proxy=$http_proxy --build-arg https_proxy=$https_proxy"
        d.run "jenkins", args: "-p 8080:8080 -v /data/jenkins:/var/jenkins_home --privileged -e TRY_UPGRADE_IF_NO_MARKER=true"
      end
    end
  end