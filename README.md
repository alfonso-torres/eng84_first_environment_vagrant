# DevOps Introduction

## Dev Env

![ERD](./vagrant_diagram.png)

Let's set up all the software that we will need to prepar our environment.
Setup guide for Vagrant, VirtualBox and Ruby in this [link](https://github.com/khanmaster/vb_vagrant_installtion)

### Bash commands

- vagrant commands:

- `vagrant up` to spin up a virtual machine.
- `vagrant destroy` to destroy.
- `vagrant reload` to reload.
- `vagrant status` to check hoy many machines are running and if they all running.
- `vagrant shh` to ssh into virtual machine (VM)
- `vagrant halt` to pause


- apt-get in Linux is a package manager to install/update like windows installer, mac app store
- `apt-get`

### Linux commands
- who am I `uname`.
- list dir `ls` list all dir `ls -a`.
- make Dir `mkdir`.
- create file `nano file_name`.
- `sudo` used to run commands as an admin.
- `sudo su` to go into admin mode.
- to go back a dir from current location `cd ..`.
- Where am I `pwd`.
- update command `sudo apt-get update -y`.
- upgrade command `sudo apt-get upgrade -y`.
- `clear` to clear screen/terminal.
- renaming the README `mv README.md MYREADME.md
`.
- copy our README file from current location to devops folder `cp MYREADME.md devops/MYREADME.md
`. Need to entiry the whole path.
- detele a file `rm name_of_the_file`.
- move file with mv command is to change the path `mv MYREADME.md devops/
`
- nano provision.sh, and inside:

#!/bin/bash

sudo apt-get update -y

- chmod permission 700 400 u x w r `sudo chmod +x provision.sh
`
- chmod +rwx filename

+ means add permission
r read
w write
x execute
- check current permision `ll`
- `top` to check current process
- check process `ps` ps aux

- Let's install web server called NGINX `sudo apt-get install nginx`
- check if the nginx has been installed `systemctl status nginx`

### Let's run the tests on our hosts machine and pass the tests by installing the required dependencies

### Let's automate the installation of required dependecies in our vagrant file to run our script

- add shell script path to our Vagrantfile
- `config.vm.provision "shell", path: "environment/provision.sh"`

- Let's create our script:
```
provision.sh
#!/bin/bash

# run the update command
sudo apt-get update -y

# upgrade command
sudo apt-get upgrade -y

# install nginx
sudo apt-get install nginx -y

# install nodejs with required version and dependecies
sudo apt-get install python-software-properties

curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
sudo apt-get install nodejs -y

# install npm with pm2 -g
sudo npm install pm2 -g
```
### Linux variables and Env var

- How can we check the existing env variable in our system `env` `printenv`.
- Export is the keyword to create an env variable.
- As key=value, key="some other value"
- Key = value1:value2
- What are the system default env variables `USER`,`HOME`,`PATH`, `TERM`

## Activity

**Tasks**:

1. Testing the environment and launching nginx webserver.
2. Creating IP.
3. Aliases to link with IP.

**Solution**:

1. We proceed to create the file `Vagrantfile`, in which we define the following image of the virtual machine that we are going to deploy, specifying the ip that we want to use.

````ruby
Vagrant.configure("2") do |config|
  config.vm.define "app" do |app|
  	app.vm.box = "ubuntu/xenial64"
  	# creating a virtual machine ubuntu
  	# Linux ubuntu 16.-04 box

  	# Let's attach private network with IP
  	app.vm.network "private_network", ip: "192.168.10.100"
  end

end
````

2. Then we proceed to run the machine in the same folder where we have located the file:

`vagrant up`

3. We proceed to connect to the machine:

`vagrant ssh`

4. We proceed to install and run the `nginx` service inside the machine:

`sudo apt-get update`

`sudo apt-get install nginx`

`systemctl status nginx` : it should be running

We proceed to access our browser and enter the IP address that we have assigned. The nginx interface should appear.

5. We proceed to assign the alias in `Vagrantfile`. Use hostupdater:

````ruby
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

  	# Creating an alias to link this IP with a logical web address
  	app.hostsupdater.aliases = ["development.local"]

  end

end
````

6. Finally we will restart the machine so that all the changes have been saved correctly and then we will be able to enter the alias assigned in the browser to verify that it works correctly.

`vagrant reload`
