# Install required plugins
required_plugins = ["vagrant-hostsupdater"]
required_plugins.each do |plugin|
    exec "vagrant plugin install #{plugin}" unless Vagrant.has_plugin? plugin
end

Vagrant.configure("2") do |config|
  config.vm.define "app" do |app|
  	app.vm.box = "ubuntu/xenial64"
  	# creating a virtual machine ubuntu

  	# Linux ubuntu 16.-04 box
  	# Let's attach private network with IP
  	app.vm.network "private_network", ip: "192.168.10.100"
    
    #app.vm.network "hostonly", "192.168.10.100"
  	#app.vm.network "forwarded_port", guest: 80, host: 8080
  	#config.vm.network "private_network", ip: "192.168.10.100", auto_config: false
  	# config.vm.network "forwarded_port", guest: 80, host: 8080
  	# config.vm.network "forwarded_port", guest: 3000, host: 8081

  	# Creating an alias to link this IP with a logical web address
  	app.hostsupdater.aliases = ["development.local"]

  	# to transfer files/folder data from our OS to VM vagrant has an option os synced_folder
  	# config.vm.synced_folder ".", "/home/vagrant/app"
  	# config.vm.provision "shell", path: "environment/provision.sh"
  end

end