Vagrant.configure(2) do |config|

config.vm.define "controller" do |controller|
  controller.vm.box = "ubuntu/trusty64"
  controller.vm.box_check_update = false
  #	controller.vm.synced_folder "../data", "/vagrant_data"
  controller.vm.network "private_network", ip: "192.168.50.10"
  controller.vm.provision :hosts, :sync_hosts => true
  controller.vm.hostname = 'controller.local'
  controller.vm.provider "virtualbox" do |v|
     v.memory = 256
     v.cpus = 1
     v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
     v.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
end

#below is a fix for no-tty problem
controller.vm.provision "fix-no-tty", type: "shell" do |s|
     s.privileged = false
     s.inline = "sudo sed -i '/tty/!s/mesg n/tty -s \\&\\& mesg n/' /root/.profile"
end

controller.vm.provision "shell", inline: <<-SHELL
    apt-add-repository ppa:ansible/ansible
    apt-get update
    apt-get upgrade -y
    apt-get install software-properties-common ansible git zip unzip mc -y
    apt-get autoremove
    ssh-keygen -t rsa -q -N '' -f /home/vagrant/.ssh/id_rsa
    cp /home/vagrant/.ssh/id_rsa.pub /vagrant/ssh/id_rsa.pub
    cp /vagrant/ssh/config /home/vagrant/.ssh/config
    chown vagrant:vagrant /home/vagrant/.ssh/*
 SHELL
end


(0..8).each do |i|
  config.vm.define "node#{i}" do |node|
     node.vm.box = "ubuntu/trusty64"
     node.vm.box_check_update = false
     #	node#{i}.vm.synced_folder "../data", "/vagrant_data"
     node.vm.network "private_network", ip: "192.168.50.2#{i}"
     node.vm.provision :hosts, :sync_hosts => true
  	   node.vm.hostname = "node#{i}.local"
	   node.vm.provider "virtualbox" do |v|
	       v.memory = 256
	       v.cpus = 1
	       v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
         v.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
	       end

     #below is a fix for no-tty problem
     node.vm.provision "fix-no-tty", type: "shell" do |s|
        s.privileged = false
        s.inline = "sudo sed -i '/tty/!s/mesg n/tty -s \\&\\& mesg n/' /root/.profile"
        end

	   node.vm.provision "shell", inline: <<-SHELL
         apt-get update
         apt-get upgrade -y
         apt-get install zip unzip mc -y
         apt-get autoremove -y
         cat /vagrant/ssh/id_rsa.pub >> /home/vagrant/.ssh/authorized_keys
     SHELL
    end

end #(0..8).each do |i|

end
