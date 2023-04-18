### What is a virtual machine?

A vitual machine is the emulation of a computer system. Virtual machines are based on computer architecture and provides the functionality of a physical machine. It creates a virtualized environment that can run an operating system and execute software applications as if they were running on a physical computer. Virtual machines allow multiple operating systems to run on a single physical computer.

### What is a development enviroment?

A set of tools, software, and configurations used by developers to create, test, and debug software applications. It usually includes programming languages, integrated development environments (IDEs), text editors, version control systems, testing frameworks, and other tools that help developers to write, test, and deploy code. 

image.png

### What is the purpose of the dev env?

It provide a separate and controlled environment for software developers to write, test, and debug their code without affecting the production environment.  It also enables them to catch and fix errors and bugs before they make it to the production environment.
image.png

### Vagrant

image.png

Virtual box is what we use to make virtual machines.

Vagrants gives the instructions to virtual box and standardises what is in the virtual box. it sends instrction to the virtual box about the type of machine that we want.

### creating virtual machines with vagrant

You will need to use both the terminal in your visual studio code and your Git Bash app.

You need to install the following:
Vagrant
Virtual box
ruby
bash 
virtual studio code

1. in your virtual studio code type ```vagrant init``` this will create a vagrant file. this file is used to config the virtual machines on virtual box. the vagrant file is wriiten in ruby. below is the intial content of a vagrant file. note we changed the vm.box to ubuntu as this is the operating system we want to use.
```
Vagrant.configure("2") do |config|

  config.vm.box = "ubuntu/xenial64"

end
```
2. type ```vagrant up``` in virtual studio code. this uses the instructions in your vagrant file to create the virtual machine in virtual box 

3. to access your virtual machine, go to your Git Bash, cd to the relevant folder and type ```virtual ssh``. This takes you to your virtual machine. 

4. update and upgrade your virual machine using the following linux commands (do this in Git Bash app):
```
#!/bin/bash
sudo apt update -y
sudo apt upgrade -y
sudo apt install nginx -y
sudo systemctl restart nginx
sudo systemctl enable nginx
```
5. you need an ip address for your vm, to get this, you need to update your vagrant file (done in visual studio code). below we have used a private ip address. then type ```vagrant reload``` in your visual studio code then ```vagrant ssh``` in your Git Bash app. 

```
Vagrant.configure("2") do |config|

  config.vm.box = "ubuntu/xenial64"
  config.vm.network "private_network" ip: "192.168.10.100"

end
```
To check if your ip is working properly, type your ip address in a webbrowser. if it is working properly then you should see the nginx brand. 

6. Alternatively to manual inputting the linux commands, to create a script that runs when spinning up a Vagrant virtual machine (VM), you can follow these steps (https://developer.hashicorp.com/vagrant/docs/provisioning/shell):

- create a script file in the same folder as your vagrant file. make sure to save it as a .sh file:
```
#!/bin/bash
sudo apt update -y
sudo apt upgrade -y
sudo apt install nginx -y
sudo systemctl restart nginx
sudo systemctl enable nginx
```

- add the following line to the vagrant file:
config.vm.provision "shell", path: "provision.sh"

below is how your vagrant file should look
```
Vagrant.configure("2") do |config|

  config.vm.box = "ubuntu/xenial64"
  config.vm.network "private_network", ip: "192.168.10.100"
  config.vm.provision "shell", path: "provision.sh"

end
```

- type ```vagrant up``` in your visual studio code terminal

7. to terminate your vagrant virtual machine type ```vagrant destroy```. Alternatively, to pause it type ```vagrant halt```. This can be typed in your virtual studio code
