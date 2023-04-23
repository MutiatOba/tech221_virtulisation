### 4 pillars of devops

1. cost: this is often ovelooked. It is best to make sure that the company is being as efficient as possible in its tech dealings. For example, not over provisioning servers when it isnt required.

2. flexibilty: it's easy to get locked in to a specific product. This is known as 'vendor lock-in'. This makes it harder to keep up with industry changes. Everything the company uses should be easily changed or updated as the business needs change.

3. ease of use: make things as easy as possible for other to do their jobs. People wont used the tools provided it they aren't easy to use. If they don't use our tools then there will be delays.

4. robustness: we need as close to 100% uptime of company services as possible. We as devops engineers are responsible for this.

<img width="575" alt="image" src="https://user-images.githubusercontent.com/118978642/232737052-e33159fc-2988-411f-ab84-809bea4edeb8.png">

Monolith architecture - entire architeture on one pysical machine. It's better to seperate out the architecture into components working seperatley. You can split architecture into two pieces which communicate with each other. That's how you get a 2 tier application. The architecture can be broken down even further into microservices.

<img width="485" alt="image" src="https://user-images.githubusercontent.com/118978642/232739150-16751849-b428-4449-b242-68e901c0f78b.png">



### What is a virtual machine?

Virtualisation allows you to run multiple "virtual" machines or environments on a single physical machine.

A vitual machine is the emulation of a computer system. Virtual machines are based on computer architecture and provides the functionality of a physical machine. It creates a virtualized environment that can run an operating system and execute software applications as if they were running on a physical computer. Virtual machines allow multiple operating systems to run on a single physical computer.

### What is a development enviroment?

A set of tools, software, and configurations used by developers to create, test, and debug software applications. It usually includes programming languages, integrated development environments (IDEs), text editors, version control systems, testing frameworks, and other tools that help developers to write, test, and deploy code. 

<img width="572" alt="image" src="https://user-images.githubusercontent.com/118978642/232735867-fb0b0ef3-82eb-4bac-a75b-0d1608e9f60f.png">

1. user friendly, fast and robust - if not people would end up usinig something else

2. as close to the production enviroment as possible. this will do away with future errors when we come to deploy

3. support one application. say appliction 1 requires version 1.1 of a software and then you have a second app that requiores version 1.5. then this could lead to comflicts. App1 might need a program that conflicts with App2

4. it should be the same for everyone everywhere 

5. it should be easy to update - e.g using a shell script.

Vagrant allows us to achieve all 5 of these points. 

### What is the purpose of the dev env?

It provide a separate and controlled environment for software developers to write, test, and debug their code without affecting the production environment.  It also enables them to catch and fix errors and bugs before they make it to the production environment.
image.png

Vagrant is a tool that allows you to create and configure development environments. It automates the process of setting up a virtual machine and configuring it with the necessary software and settings, making it easy to create and share development environments with others.

Vagrant uses a virtualization provider, such as VirtualBox, to create and manage the virtual machines. VirtualBox is a free and open-source virtualization software that allows you to create and run virtual machines on your computer. Vagrant is designed to work seamlessly with VirtualBox.

When you use Vagrant, you can define a virtual machine configuration in a Vagrantfile, which is a simple text file that specifies the settings and software that should be installed on the virtual machine. Vagrant then uses VirtualBox (or another virtualization provider) to create the virtual machine based on the configuration specified in the Vagrantfile. This allows you to easily and quickly set up and manage development environments, without having to manually configure and manage virtual machines.

To share a dev enviroment created with vagrant, you can use Vagrant share.

### Vagrant

<img width="574" alt="image" src="https://user-images.githubusercontent.com/118978642/232735775-016f0584-18c6-42d6-b8c2-1f1504a6d03d.png">

#### launch multiple vms

<img width="608" alt="image" src="https://user-images.githubusercontent.com/118978642/233355477-cde9db33-d813-4408-a00c-f71b7fea1346.png">


Virtual box is what we use to make virtual machines.

Vagrants gives the instructions to virtual box and standardises what is in the virtual box. it sends instruction to the virtual box about the type of machine that we want.

### creating virtual machines with vagrant

You will need to use both the terminal in your visual studio code and your Git Bash app.

You need to install the following:
- Vagrant
- Virtual box
- ruby
- git bash 
- virtual studio code

1. Cd to the correct folder in your file system. Then, in your virtual studio code type ```vagrant init``` this will create a vagrant file. this file is used to config the virtual machines on virtual box. the vagrant file is wriiten in ruby. below is the intial content of a vagrant file. note we changed the vm.box to ubuntu as this is the operating system we want to use.
```
Vagrant.configure("2") do |config|

  config.vm.box = "ubuntu/xenial64"

end
```
2. type ```vagrant up``` in virtual studio code. this uses the instructions in your vagrant file to create the virtual machine in virtual box 

3. to access your virtual machine, go to your Git Bash, cd to the relevant folder and type ```vagrant ssh``. This takes you to your virtual machine. 

4. update and upgrade your virual machine using the following linux commands (can do this in Git Bash app - can run as a script or run the individual commands):
```
#!/bin/bash
sudo apt update -y
sudo apt upgrade -y
sudo apt install nginx -y
sudo systemctl restart nginx
sudo systemctl enable nginx
```
We first update the list of packages, then upgrade downloads the packages. the -y flag is used to automatically answer yes to any prompts that may appear. the restart command restarts NGINX service after it has been installed ensuring that any changes to the cnfiguration files or software itself takes effect. the enable command eneables nginx service to start automatically when the system boots up. so dont have to manually start the service everytime you start your machine.  

NGINX is a powerful and flexible web server that has become a popular choice for serving web content, proxying requests, and load balancing traffic due to its high performance and advanced features. Think of a web server as a virtual waiter in a restaurant. When a user types a web address (URL) into their browser, it's like a customer placing an order. The browser sends a request to the web server, just as a customer would place an order with a waiter. The web server then processes the request and sends the requested web page back to the user's browser, just as a waiter would bring the requested dish to a customer's table.

5. you need an ip address for your vm, to get this, you need to update your vagrant file (done in visual studio code). below we have used a private ip address. then type ```vagrant reload``` in your visual studio code then ```vagrant ssh``` in your Git Bash app. 

```
Vagrant.configure("2") do |config|

  config.vm.box = "ubuntu/xenial64"
  config.vm.network "private_network", ip: "192.168.10.100"

end
```
If you have made changes to the Vagrantfile after the virtual machine has already been created and provisioned, you can apply those changes to the running virtual machine without having to stop and restart it by using the vagrant reload command. Once the virtual machine has finished reloading, you can connect to it via SSH using the command vagrant ssh.

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
To effect these changes, first destroy your vm: ```vagrant destroy``` then relauch the vm: ```vagrant up```. Then ```vagrant ssh``` to access the virtual machine. 

7. to terminate your vagrant virtual machine type ```vagrant destroy```. Alternatively, to pause it type ```vagrant halt```. This can be typed in your virtual studio code


### Installing an app on VM using vagrant

To determine what development environment is required for a simple app created by a developer using Vagrant and VirtualBox, you can start by looking at the application's dependencies.

The developer should provide a list of the application's dependencies, including the programming language and any libraries or frameworks required to run the application. You can use this information to identify the necessary software components that need to be installed on the virtual machine.

In addition to the application's dependencies, you should also consider the operating system and other software requirements. For example, if the application is designed to run on a Linux operating system, you will need to install a Linux distribution on the virtual machine.

Once you have identified the required software components, you can create a Vagrantfile that specifies the necessary configuration for the virtual machine. This may include specifying the operating system, installing software packages, setting up network interfaces, and configuring shared folders.

Finally, you can use the Vagrantfile to create the virtual machine and provision it with the necessary software components. The developer can then use the virtual machine to develop and test the application in an environment that closely matches the production environment.

STEPS: 

Need to get app folder into vagrant file so can run on vm.

so need to sync the app folder with vagrant file - so a change in one file will lead to a change in the other 

in vagrant file type the following code: ```config.vm.synced_folder "app", "/home/vagrant/app"```

the file we are targeting: ```app```
where in the vm we want the app file to be stored: ```/home/vagrant/app```

content of vagrant file:
```
Vagrant.configure("2") do |config|

  config.vm.box = "ubuntu/xenial64"
  config.vm.network "private_network", ip: "192.168.10.100"
  config.vm.provision "shell", path: "provision.sh"

  #syncing the app folders
  config.vm.synced_folder "app", "/home/vagrant/app"

end
```
Type ``vagrant up``` in your visual studio code.

Head over to Git Bash and type ```vagrant ssh``` (make sure that you are in the correct folder where your code is saved). Need to make sure app folder is synched to virtual machine.

Once the VM is running, type ```ls``` to make sure the app folder is in virtual machine. Then cd to app folder and then ```ls``` to see what's inside the app folder.

We need to ask the following questions as a devops:
- what framework
- what language
- what version of packages
- what wil the app look like

This is because, we need to make sure that our enviroment is the correct one needed for the app.

On visual studio code, check the enviroment folder as we need to run a test to check our enviromenet is correct. The test is in spec test folder.

The test will be run using ruby, so need to make sure ruby is installed.

Whilst in visual studio code, cd into spec test folder in our enviroment folder.

Type the following commands in visual studio terminal: 

- ```gem install bundler``` - allows us to bundle all test together
- ```bundle``` - bundles all the test
- ```rake spec``` - this is the command developers created, which will run test. it starts all the tests. if doesnt work then type in ```gem install serverspec```

if the test is run successfully it will show that we need the following:

- need nodejs 
- need nodejs version 6
- need pm2

Node.js well-suited for building real-time applications such as chat applications, gaming servers, and streaming applications.

go to git bash and type the following commands: 

- ```sudo apt-get install nodejs -y``` - installs nodejs
- ```sudo apt-get install python-software-properties``` - The "python-software-properties" package provides a tool called "add-apt-repository" which is used to add new software repositories to the system's package sources list.
- grabs the version of nodejs that we want: ```curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -```
- puts version 6 in affect: ```sudo apt-get install nodejs -y```
- checks the version of node: ```nodejs --version```

pm2 - used to unpackage the app. its package manager for nodejs. type the following in git bash app to install it:
 
```sudo npm install pm2 -g``` - when you run this command, you are telling npm to install the PM2 process manager globally on your system, making it available to all Node.js applications you run. The "-g" flag ensures that the package is installed globally, rather than just for the current project.

Whilst in your git bash app, cd into app then run the following commands:

- ```npm install``` - used to install Node.js packages or dependencies for a Node.js projec
- ```node app.js``` -  is used to run a Node.js application called "app.js

Go to webbrowers and type in ip_address:3000
port - allows communicaiton between 2 computers using different protocols



#### provision the app

Alternatively to manually updating the enviroment for the app. You can do the following:

1. update the provision.sh file (this must be saved in the same folder as the app and vagrant file for the below code to work). Alternatively use script in step 6:
```
#!/bin/bash
sudo apt update -y
sudo apt upgrade -y
sudo apt install nginx -y
sudo systemctl restart nginx
sudo systemctl enable nginx

sudo apt-get install nodejs -y
sudo apt-get install python-software-properties
curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
sudo apt-get install nodejs -y
sudo npm install pm2 -g
```
2. make sure your vagrant file has been updated to reference the script and to sync with the app. The final file should look like so:
```
Vagrant.configure("2") do |config|

  config.vm.box = "ubuntu/xenial64"
  config.vm.network "private_network", ip: "192.168.10.100"
  config.vm.provision "shell", path: "provision.sh"

  #syncing the app folders
  config.vm.synced_folder "app", "/home/vagrant/app"

end
```
3. type ```vagrant up``` in the vs code and then ```vagrant ssh``` in git bash

4. make sure you cd to the app folder and then type the following two commands to launch the app: 
- ```npm install``` - used to install Node.js packages or dependencies for a Node.js projec
- ```node app.js``` -  is used to run a Node.js application called "app.js

5. head over to a web brower and type your ipadress:3000 to see your new app, like so:

```http://192.168.10.100:3000/```

6. Alternativelty, skip step 4 by using the following bash script in step 1

```
#!/bin/bash
sudo apt update -y
sudo apt upgrade -y
sudo apt install nginx -y
sudo systemctl restart nginx
sudo systemctl enable nginx

sudo apt-get install nodejs -y
sudo apt-get install python-software-properties
curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
sudo apt-get install nodejs -y
sudo npm install pm2 -g
cd app
npm install
node app.js
```

### Nginx Reverse Proxy

#### what is a port?
 a port is a communication endpoint which is used to identify an application running on a computer. Data sent over a network is broken down into packets each with a destination ip address and port number. the destination port numner is used to send the packet to the correct application or service running in the destination computers. The 2 types of ports are TCP(reliable connection) and UDP(faster connection).
 
 #### What is a reverse proxy? How is it different to a proxy?
 
 A proxy server is like a middleman between your computer and the internet. It takes your requests and sends them to the destination server on your behalf, then sends the response back to your computer.

A reverse proxy is a similar middleman, but instead of sending your requests to the internet, it sends them to one or more servers on a private network. The response from the server is then sent back to you through the reverse proxy server.

The main difference between a regular proxy and a reverse proxy is the direction of the traffic flow. A regular proxy sits between you and the internet, while a reverse proxy sits between you and the private network.

Reverse proxies are commonly used to make web applications faster and more secure. They can help distribute incoming requests across multiple servers, reducing the load on each server and improving the overall performance of the application. They can also cache frequently requested content, reducing the time it takes to load pages and improving your experience as a user.

<img width="482" alt="image" src="https://user-images.githubusercontent.com/118978642/233035064-ab0dc3ae-482c-431d-989b-fac7a1cdba0d.png">


 #### What is Nginx's default configuration
 
 Nginx's default configuration includes a directory called sites-available, which contains configuration files for different websites or web applications that are hosted on the server. This directory is usually located at /etc/nginx/sites-available/ on Unix-based systems, such as Linux.
 
 #### steps to install reverse proxy for nginx
 
 1. make sure nginx is installed ```sudo apt-get install nginx``` and your server is up and running.
 2. cd to the nginx configuration file: ```cd /etc/nginx/sites-available/```
 3. create a reverse proxy file in the folder: ```sudo nano reverse-proxy``` and include following code:
 ```
 server {
    listen 80;
    server_name 192.168.10.100;

    location / {
        proxy_pass http://192.168.10.100:3000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```
This configuration will listen on port 80 for requests to your domain and forward them to the local port 3000

4. Enable the configuration: Create a symbolic link from the sites-available directory to the sites-enabled directory to enable the new configuration file:
```
sudo ln -s /etc/nginx/sites-available/reverse-proxy /etc/nginx/sites-enabled/
```

5. check configuration file for errors

```
sudo nginx -t
```
6. relaod your nginx

```
sudo systemctl restart nginx

```
7. Can now access your app from webbrowers using ip address.

### Naming the VM

Need to name the VM: go to vagrant file and add this line:

```config.vm.define "app" do |app|```

everything under the above code is defined under app. see full content of vagrant file below:

```
Vagrant.configure("2") do |config|

  config.vm.define "app" do |app|
    app.vm.box = "ubuntu/xenial64"
    app.vm.network "private_network", ip: "192.168.10.100"
    app.vm.provision "shell", path: "provision.sh"
    #syncing the app folders
    app.vm.synced_folder "app", "/home/vagrant/app"
  end
end 
```

Then type ```vagrant up``` to create the vm.

### create a second vm - for our database

Update vagrant file as follows:

```
Vagrant.configure("2") do |config|

  config.vm.define "app" do |app|
    app.vm.box = "ubuntu/xenial64"
    app.vm.network "private_network", ip: "192.168.10.100"
    app.vm.provision "shell", path: "provision.sh"
    #syncing the app folders
    app.vm.synced_folder "app", "/home/vagrant/app"
  end

  config.vm.define "db" do |db|
    db.vm.box = "ubuntu/xenial64"
    db.vm.network "private_network", ip: "192.168.10.150"
  end

end
```

before launching thr vm make sure to remove the following from your provision file as this will prevent the second vm from setting up:
```
#cd app
#npm install
#node app.js

```

Then write ```vagrant up``` to create the two VMs

The benefit of vagrant file. create a test enviroment when it is needed. if we give dev vagrant file then all devs are working on the same enviroment. if we make the production the same as dev enviroment then unlikely to have as many errors. 

to get into the first vm type ```vagrant ssh app``` in git bash. if you do ```ls``` you should see your app

open a second git bash to login into database (make sure you are in the correct folder where your vagrant file is) and type ```vagrant ssh db```


### setting up mongodb

to launch your db vm only type: ```vagrant up db```

Once you have ssh'ed in db vm (doing ```vagrant ssh db```), type the following commands:

- ```sudo apt update -y``` - to update the vm
- ```sudo apt upgrate -y``` - to install all updates 
- ```sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv D68FA50FEA312927``` - its adding a key for mongodb as mongdo requires a key to be downloaded 

- ```echo "deb https://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/3.2 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.2.list``` - basically saying that you have the key and know where to go and use it

now that we have the key, need to get packages we need for mongodb so run the following commands:
- ```sudo apt update -y``` 
- ```sudo apt upgrate -y``` 


to install mongodb: ```sudo apt-get install -y mongodb-org=3.2.20 mongodb-org-server=3.2.20 mongodb-org-shell=3.2.20 mongodb-org-mongos=3.2.20 mongodb-org-tools=3.2.20```


start mongodb: ```sudo systemctl start mongod```
check status: ```sudo systemctl status mongod```

### automate process of launch mongdo when launching vm

1. create a provision.sh file. make sure if it is saved in the same location as any other provision files that it is given a different name. Alternatively, it is best practice to save a second provision file in an alternative location.
The content of the provision.sh file for the db is as follows:

```
#!/bin/bash
sudo apt update -y
sudo apt upgrade -y
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv D68FA50FEA312927
echo "deb https://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/3.2 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.2.list
sudo apt update -y
sudo apt upgrade -y
sudo apt-get install -y mongodb-org=3.2.20 mongodb-org-server=3.2.20 mongodb-org-shell=3.2.20 mongodb-org-mongos=3.2.20 mongodb-org-tools=3.2.20
sudo systemctl start mongod
``` 

2. update your vagrant file to include the following:
```
db.vm.provision "shell", path: "~/Documents/tech221_virtualisation/environment/provision.sh"

```
your vagrant file should look like so:

```
Vagrant.configure("2") do |config|

  config.vm.define "app" do |app|
    app.vm.box = "ubuntu/xenial64"
    app.vm.network "private_network", ip: "192.168.10.100"
    app.vm.provision "shell", path: "provision.sh"
    #syncing the app folders
    app.vm.synced_folder "app", "/home/vagrant/app"
  end

  config.vm.define "db" do |db|
    db.vm.box = "ubuntu/xenial64"
    db.vm.network "private_network", ip: "192.168.10.150"
    db.vm.provision "shell", path: "~/Documents/tech221_virtualisation/environment/provision.sh"
  end

end
```

3. launch the vm for the db by typing: ```vagrant up db```

4. head to git bash and cd to the relevant folder, then type ```vagrant ssh db```

5. check that mongodb is running by typing the following: ```sudo systemctl status mongod```

### running processes in the background on linux

To run a process in the backgound, type the command followed by &, so for example:
```
npm install &
node app.js &
```
NOTE: it is better to use pm2 for this.

The provision.sh file for your app can therefore be updated as follows:

```
#!/bin/bash
sudo apt update -y
sudo apt upgrade -y
sudo apt install nginx -y
sudo systemctl restart nginx
sudo systemctl enable nginx
# to install dependencies for app
sudo apt-get install nodejs -y
sudo apt-get install python-software-properties
curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
sudo apt-get install nodejs -y
sudo npm install pm2 -g
cd app
npm install &
node app.js &
```
To use pm2 to run the nodejs app as a background process [PROCESS TO BE REVISED AS NOT WORKING YET!]:

1. make sure that you have pm2 installed: ```sudo npm install pm2 -g```
2. [install dependencies for node.js ```npm install```] NOT REQUIRED
3. start the app process with pm2: 
```pm2 start app.js```
4. To check the status of your PM2 processes: ```pm2 status```. To stop a PM2 process: ```pm2 stop your_process_name``` .

```
provision.sh file for app:
#!/bin/bash
sudo apt update -y
sudo apt upgrade -y
sudo apt install nginx -y
sudo systemctl restart nginx
sudo systemctl enable nginx
# to install dependencies for app
sudo apt-get install nodejs -y
sudo apt-get install python-software-properties
curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
sudo apt-get install nodejs -y
sudo npm install pm2 -g
cd app
#npm install &
#node app.js &
pm2 start app.js

```

### connecting app to db
1. Make sure your provision file for your app is like so:
```
#!/bin/bash
# Provisioning for app vm
# Update the sources list
sudo apt-get update -y
# upgrade any packages available
sudo apt-get upgrade -y
# install nginx
sudo apt-get install nginx -y
# install git
sudo apt-get install git -y
# install nodejs
sudo apt-get install python-software-properties
curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -
sudo apt-get install nodejs -y
# install pm2
sudo npm install pm2 -g
```
2. launch the app and db vms in two different git bash:

- ```vagrant ssh db```
- ```vagrant ssh app```

3. check app and db vms are properly installed 

To check that your app vm is running correctly, check the following:

- ```ls```: to check  that the app folder is there
- ```nodejs --version ``` : to check that node has been properly installed
- ```pm2 --version```: to check that pm2 has been properly installed

On db vm check that mongodb is up and running:
```sudo systemctl status mongod```

4. we need to connect db vm with app vm. 

go to db vm, and type: ```sudo nano /etc/mongod.conf```

In the file get to #network interfaces section then change bindIP:0.0.0.0 (saying that all ips can access our database, fine to do this as we are working in dev enviroment). Then save changes. 

As mongodb is running we need to restart it so it can implement the changes.

```sudo systemctl restart mongod```
```sudo systemctl enable mongod```: if we restart vm it will put changes into effect

to check that the changes have been made do ```sudo systemctl status mongod```

ctrl+z to get out of it.

5. make changes in app vm

head over to app vm.

need to create a variable to connect db to app

type: ```sudo nano .bashrc``` this is to create an enviroment variable

scroll to bottom of the file. Note: the developers have create an enviroment variable called db_host. after == specify where we want to conenct to the db

insert the following into the bottom of the file:
```export DB_HOST=mongodb://192.168.10.150:27017/posts```

to check if the variable has been created: ```printenv DB_HOST```

```source .bashrc``` (re run to reflect the new changes)

```printenv DB_HOST```: then you should see the newly created variable.

6. install the relevant dependancies for the app

- ```cd app```
- ```npm install```
- ```node seeds/seed.js```: this puts all the data into mongodb. asking nodejs to run the seed.js file
- ```start app: node app.js```

go to webbrower and type the following: ip:3000/posts

#### edit the mongod.conf via your db provision file

Update the db provision file to include the following commands:
```
sudo sed -i 's/bindIp: 127.0.0.1/bindIp: 0.0.0.0/' /etc/mongod.conf
sudo systemctl restart mongod
sudo systemctl enable mongod
```
Then continue with the above steps starting from and including step 5.


