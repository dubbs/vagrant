# Vagrant

Vagrant is used to *create* and *configure* 
lightweight, reproducible, and portable (Linux, Mac OS X or Windows) development environments


## Setup

A `Vagrantfile` is used to describe your project's environment, specifically:

1. root directory
2. box or machine type, allocated resources and how it is accessed
3. software installed

Software installation can be deferred to provisioning tools such as **shell scripts**, **Chef**, or **Puppet**.

To create a new Vagrantfile, run:

	vagrant init

## Boxes

Vagrant uses as a box (base image) to clone a virtual machine.

So, the first step is to add a box from the [Vagrant Cloud](https://www.vagrantcloud.com/discover/):

	vagrant box add chef/ubuntu-13.10

Inside your `Vagrantfile`, reference this box:

	Vagrant.configure("2") do |config|
		config.vm.box = "chef/ubuntu-13.10"
	end

Verify

	vagrant up
	vagrant ssh

## Provisioning

Vagrant has built in provisioning via the **shell** provisioner.

First create a public directory to house our web files:

	mkdir public
	echo "HI" > public/index.html

Simply create a bootstrap.sh file at the root of our project:

	#!/usr/bin/env bash

	apt-get update
	apt-get install -y apache2
	rm -rf /var/www
	ln -fs /vagrant/public /var/www

Add this script reference to our Vagrantfile:

	Vagrant.configure("2") do |config|
		config.vm.provision :shell, :path => "bootstrap.sh"
	end

Then reload and run the provisoner

	vagrant reload --provision

Verify

	vagrant ssh
	wget -qO- 127.0.0.1


## Networking

Vagrant supports port forwarding, public and private networks

To setup port forwarding for our apache server simply:

	Vagrant.configure("2") do |config|
		config.vm.network :forwarded_port, host: 4567, guest: 80
	end

Then reload

	vagrant reload

Verify

	http://localhost:4567


## Teardown

Vagrant supports a number of teardown options:

- **Suspend** saves VM RAM and contents to disk, fast boot
- **Halt** saves VM contents to disk, cold boot
- **Destroy** removes VM from disk




