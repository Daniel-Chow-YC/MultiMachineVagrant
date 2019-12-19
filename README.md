# Multi Machine Vagrant

## Summary
- The sample application has the ability to connect to a database. We need to provision our development environment with a vm for the app and one for the database.
- Vagrant is capable of running two or more virtual machines at once with different configurations.

## Tasks
Research how to create a multi machine vagrant environment
	- Add a second virtual machine called "db" to your Vagrant file
	- Configure the db machine with a different IP from the app
	- Provision the db machine with a MongoDB database
	- You can test your database is working correctly by running the test suite in the test folder. There are two sets of tests. One for the app VM and one for the db VM. Make them all pass.
	``cd test rake spec``

## Creating a multi machine vagrant environment
- Multiple machines are defined within the same project Vagrantfile using the config.vm.define method call.
- This configuration directive creates a Vagrant configuration within a configuration.
- For example, in this situation:
````
Vagrant.configure("2") do |config|

  config.vm.define "app" do |app|
    app.vm.box = "ubuntu/xenial64"
    app.vm.network "private_network", ip: "192.168.10.100"
    app.hostsupdater.aliases = ["development.local"]
    app.vm.synced_folder "app", "/home/ubuntu/app"
    app.vm.provision "shell", path: "environment/app/provision.sh", privileged: false
  end

  config.vm.define "db" do |db|
    db.vm.box = "ubuntu/xenial64"
    db.vm.network "private_network", ip: "192.168.10.150"
    db.hostsupdater.aliases = ["database.local"]
    db.vm.provision "shell", path: "environment/db/provision.sh", privileged: false
    db.vm.synced_folder "environment/db", "/home/ubuntu/environment"
  end

end
````
## Provision the DB machine
- Create another provision.sh file
- Make sure it is created in the correct location in accordance with the path laid out in the Vagrantfile: `` db.vm.provision "shell", path: "environment/db/provision.sh" ``
