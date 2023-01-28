VAGRANT_BOX = "generic/ubuntu2004"
VAGRANT_BOX_VERSION = "4.0.2"
MASTER_NODE_COUNT = 1
SLAVE_NODE_COUNT = 2

Vagrant.configure("2") do |config|
  config.vm.provision "shell", inline: "echo Hello"
  if Vagrant.has_plugin?("vagrant-proxyconf")
    config.proxy.http  = "http://digipay:jmTn23pqPQMhLPgU@10.198.13.10:7777"
    config.proxy.https = "http://digipay:jmTn23pqPQMhLPgU@10.198.13.10:7777"
    config.proxy.no_proxy = "localhost,127.0.0.1,10.198.0.0/16,repo.mydigipay.info,registry.mydigipay.info"
  end

  (1..MASTER_NODE_COUNT).each do |i|
    config.vm.define "pg-master-#{i}" do |node|
      node.vm.box = VAGRANT_BOX
      node.vm.box_check_update = true
      node.vm.box_version = VAGRANT_BOX_VERSION
      node.vm.hostname = "pg-master-#{i}"
      node.vm.network "private_network", ip: "192.168.56.10#{i}"
      node.vm.provider :virtualbox do |v|
        v.name = "pg-master-#{i}"
        v.memory = 1024
        v.cpus = 3
      end
      node.vm.provider :libvirt do |v|
        v.memory  = 1024
        v.nested = true
        v.cpus = 3
      end
    end
  end
  
  (1..SLAVE_NODE_COUNT).each do |i|
    config.vm.define "pg-slave-#{i}" do |node|
      node.vm.box = VAGRANT_BOX
      node.vm.box_check_update = true
      node.vm.box_version = VAGRANT_BOX_VERSION
      node.vm.hostname = "pg-slave-#{i}"
      node.vm.network "private_network", ip: "192.168.56.20#{i}"
      node.vm.provider :virtualbox do |v|
        v.name = "pg-slave-#{i}"
        v.memory = 1024
        v.cpus = 3
      end
      node.vm.provider :libvirt do |v|
        v.memory  = 1024
        v.nested = true
        v.cpus = 3
      end
    end
  end
end