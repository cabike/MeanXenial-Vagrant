# Build a MEAN Stack Vagrant Box

An easily customized Vagrant box built around the MEAN stack. It's based on the Ubuntu Xenial (16.04) image, and includes Yeoman, Angular, Express, Gulp, Grunt, and Bower ready to go.

This is definitely the long/hard way to create this box, and many other (better, easier) options exist such as [PuPHPet](https://puphpet.com/) or [Packer](https://www.packer.io/).

### What's in the box?

This MEAN stack box is based on the Ubuntu Xenial (16.04) base box and currently includes:

- NVM - Node Version Manager
- Node 10 & NPM 6
- MongoDB 3.2
- Angular 6
- Express 4.16
- Yeoman
- Gulp
- Grunt
- Bower

## Getting Started

These instructions will get you a copy of the project up and running on your local machine for development and testing purposes.

### Prerequisites

- [Vagrant](https://www.vagrantup.com/)

### Using the Box

Follow these steps to begin using this box:

 1. `mkdir myMeanBox`
 2. `cd myMeanBox`
 3. `vagrant init cabike/MeanXenial`
 4. `vagrant up`
 5. `vagrant ssh`

When the box comes up it will create a new synced folder `myMeanApp` in `myMeanBox` which you can use for your app development purposes. You can also use `yo` to pull in new generators for your MEAN projects.

### Creating the Box

All the steps to actually create the MEAN Xenial box.

1.  `mkdir myMeanBox`
2.  `cd myMeanBox`
3.  `vagrant init ubuntu/xenial64`
4.  `vagrant up`
5.  `vagrant ssh`
6.  `sudo apt-get update`
7.  `sudo apt-get upgrade -y`
8.  Install nvm
	1.  [https://github.com/creationix/nvm](https://github.com/creationix/nvm)
	2.  `curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh | bash`
	3.  `exit`
	4.  `vagrant ssh`
	5.  `command -v nvm`
	6.  `nvm install node`
	7.  `node -v`
	8.  `npm -v`
9.  Install MongoDB
	1.  [https://www.digitalocean.com/community/tutorials/how-to-install-mongodb-on-ubuntu-16-04](https://www.digitalocean.com/community/tutorials/how-to-install-mongodb-on-ubuntu-16-04)
	2.  `sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv EA312927`
	3.  `echo "deb http://repo.mongodb.org/apt/ubuntu%20xenial/mongodb-org/3.2 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.2.list`
	4.  `sudo apt-get update`
	5.  `sudo apt-get install -y mongodb-org`
	6.  `sudo systemctl start mongod`
	7.  `sudo systemctl status mongod`
	8.  `sudo systemctl enable mongod`
	9.  `mongo`
	10.  `exit`
10.  Install NodeJS Tools (Angular, Express, Yeoman, Gulp, Grunt, Bower)
	1.  `npm install -g @angular/cli express yo grunt-cli gulp-cli bower`
	2.  `sudo apt-get update`
11.  Make the box as small as possible			
	1.  `sudo apt-get clean`
	2.  `sudo dd if=/dev/zero of=/EMPTY bs=1M`
	3.  `sudo rm -f /EMPTY`
	4.  `cat /dev/null > ~/.bash_history && history -c && exit`

### Customize the Box

Modify the Vagrantfile you'll package with the box

1.  customize Vagrantfile
	1.  Set a message for when the box comes up:
	`config.vm.post_up_message = "You can now vagrant ssh in and install whatever generators you need with yeoman"`
	2. Set provider specific options:
		`config.vm.provider "virtualbox"  do |v|` 
		`v.memory =  4096`
		`v.cpus =  4`
		`v.customize [ "modifyvm", :id, "--uartmode1", "disconnected" ]`
		`end`
	3. Set forwarded ports:
		```
		#MongoDB
		config.vm.network "forwarded_port", guest: 27017, host: 27017, host_ip: "127.0.0.1"
		#Node.js
		config.vm.network "forwarded_port", guest: 3000, host: 3000, host_ip: "127.0.0.1"
	4. Set up private network access:
		`config.vm.network "private_network", ip: "192.168.33.10"`
	5. Set up synced folder:
		`config.vm.synced_folder "/", "/vagrant/myMeanApp"`
	6. Let the box update and upgrade on `vagrant up`:
		```
		config.vm.provision "shell", inline: <<-SHELL
			sudo apt-get update
			sudo apt-get upgrade -y
		SHELL
### Package the Box

1.  Package the new box with the Vagrantfile
	1.  `vagrant package --vagrantfile Vagrantfile --output meanxenial.box`
2.  Add the new box to your Vagrant install
	1.  `vagrant box add meanxenial meanxenial.box`
3.  Remove original box
	1.  `vagrant destroy`
	2.  `rm Vagrantfile`
	3.  `rm -rf .vagrant`
4.  Initialize new box
	1.  `vagrant init meanxenial`

> Written with [StackEdit](https://stackedit.io/)
