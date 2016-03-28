# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  config.vm.box = "chef/centos-6.5"
  config.vm.provision "shell", :path => 'scripts/provision-vm'
  config.vm.network "forwarded_port", guest: 50000, host: 50000
  config.vm.network "forwarded_port", guest: 4567, host: 4567 # For Java Spark if using the spark-db2 example on GitHub
  config.vm.network "private_network", :ip => "192.168.50.4"
  config.vm.host_name = "db2-dev-env"
  config.ssh.forward_agent = true
  config.vm.provider "virtualbox" do |vb|
    vb.name = "db2-dev-env"
    vb.customize ['modifyvm', :id, '--memory', ENV['VM_MEMORY'] || 2048]
    vb.customize ['modifyvm', :id, '--natdnshostresolver1', 'on']
    vb.customize ['modifyvm', :id, '--natdnsproxy1', 'on']
    vb.customize ["modifyvm", :id, "--cpus", ENV['VM_CPUS'] || 2]
  end
  # After the box is up, run the connect scripts to start the instance with sample tables on
  config.trigger.after :up do
    run_remote  "bash /vagrant/scripts/connect"
  end
end
