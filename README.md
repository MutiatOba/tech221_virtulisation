### 4 pillars of devops

1. cost - this is often ovelooked.Need to make sure the company is being as efficient as possible in its tech dealings. For example, scaling and how power a machine we need to complete a task, how many servers do we actually need.

2. flexibilty - its easy to get locked in to a specific prouduc. this is known as 'vendor lock-in'. this makes it harder to keep up with industry changes. everything the company uses should be easily changed or updtted as the business needs change

3 Ease of use - make things as easy as possible for other to do their jobs. people wont used the tools provided it they arent easy to use. if they dont use our tools then there will be delays

4. robustness - we need as close to 100% uptime of company services as possible. we as devops engineers are responsible for this.

<img width="575" alt="image" src="https://user-images.githubusercontent.com/118978642/232737052-e33159fc-2988-411f-ab84-809bea4edeb8.png">

monolith architecture - entire architeture on one pysical machine. better to seperate out into components working seperatley. split architecture into two pieces which communicate with each other. that's how you get 2 tier application. can break it down even further to microservices

<img width="485" alt="image" src="https://user-images.githubusercontent.com/118978642/232739150-16751849-b428-4449-b242-68e901c0f78b.png">



### What is a virtual machine?

A vitual machine is the emulation of a computer system. Virtual machines are based on computer architecture and provides the functionality of a physical machine. It creates a virtualized environment that can run an operating system and execute software applications as if they were running on a physical computer. Virtual machines allow multiple operating systems to run on a single physical computer.

### What is a development enviroment?

A set of tools, software, and configurations used by developers to create, test, and debug software applications. It usually includes programming languages, integrated development environments (IDEs), text editors, version control systems, testing frameworks, and other tools that help developers to write, test, and deploy code. 

<img width="572" alt="image" src="https://user-images.githubusercontent.com/118978642/232735867-fb0b0ef3-82eb-4bac-a75b-0d1608e9f60f.png">

1. user friendly, fast and robust - if not people would end up usinig something else

2. as close to the production enviroment as possible. this will do away with future errors when we come to deploy

3. support one application. say appliction 1 requires version 1.1 and then you have a second app that requiores version 1.5. then this could lead to comflicts. App1 might need a program that conflicts with App2

4. it should be the same for everyone everywhere 

5. it should be easy to update - e.g using a shell script.

Vagrant allows us to achieve all 5 of these points. 

### What is the purpose of the dev env?

It provide a separate and controlled environment for software developers to write, test, and debug their code without affecting the production environment.  It also enables them to catch and fix errors and bugs before they make it to the production environment.
image.png

### Vagrant

<img width="574" alt="image" src="https://user-images.githubusercontent.com/118978642/232735775-016f0584-18c6-42d6-b8c2-1f1504a6d03d.png">

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


### Installing an app on VM using vagrant

Need to get app folder into vagrant file so can run on vm.
so need to sync the app folder with vagrant file - so a change in one it will lead to a change in the other 

in vagrant file type the following code: ```config.vm.synced_folder "app", "/home/vagrant/app"```

the file we are targeting: app
where in the vm we want the app file to be stored: ```/home/vagrant/app```

total content of vagrant file:
---------------------------------------------------
```
Vagrant.configure("2") do |config|

  config.vm.box = "ubuntu/xenial64"
  config.vm.network "private_network", ip: "192.168.10.100"
  config.vm.provision "shell", path: "provision.sh"

  #syncing the app folders
  config.vm.synced_folder "app", "/home/vagrant/app"

end
```
----------------------------------

Head over to Git Bash and type ```vagrant ssh``` (make sure that you are in the correct folder). Need to make sure app folder is synched to virtual machine.

Once the VM is running, type ```ls``` to make sure the folders is in virtual machine. Then cd to app folder and then ls to see what's inside the app folder.


