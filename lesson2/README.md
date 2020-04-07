# Infra Structure as Code - Ansible Configuration Management

### Understanding concepts with Ansible Provisioning

Timings 90-120 Minutes

## This Lesson Includes

- Multi server environment
- Creating Multiple servers/VMs using vagrant on Windows 
- Ansible Installation
- Setting up SSH keys to access servers/VMs
- Networking
- Vagrant VMs touble shooting


# Installing Ansible on Ubuntu
## Now Lets start from scratch 
- ``` sudo apt-get update ```
- ``` sudo apt-get install software-properties-common ```
- ``` sudo apt-add-repository ppa:ansible/ansible ```
- ``` sudo apt-get update ```
- ``` sudo apt-get install ansible ```
- press Y (yes) when prompted
- Complete the installation and check with below command
- ``` ansible –version ```
![](https://github.com/spartaglobal/Ansible/blob/lesson1/images/Ansible_version.jpg)

## Above screen shows it has been installed successfully! 

- We will come back to Ansible once we have our Multi VMs environment ready.

## Moving onto creating Multi Server/VMs on Windows using Vagrant:

You have practiced creating 1 VM and now lets take it to next level and use Ansible as provisioning and configuration management tool in multi server environment  
- We will now create 3 Servers/VMs, web , db and aws using vagrant on windows
- We will use Ubuntu as an ansible controller

## Open Git Bash and run it as an administrator
In the same vagrantfile that you used to create 1 VM,  add the following code and comment out the previous code
```ruby
# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what

# MULTI SERVER/VMs environment 
#
Vagrant.configure("2") do |config|

# creating first VM called web  
  config.vm.define "web" do |web|
    
    web.vm.box = "bento/ubuntu-18.04"
   # downloading ubuntu 18.04 image

    web.vm.hostname = 'web'
    # assigning host name to the VM
    
    web.vm.network :private_network, ip: "192.168.33.10"
    #   assigning private IP
    
    config.hostsupdater.aliases = ["development.web"]
    # creating a link called development.web so we can access web page with this link instread of an IP   
        
    end
  
# creating second VM called db
  config.vm.define "db" do |db|
    
    db.vm.box = "bento/ubuntu-18.04"
    
    db.vm.hostname = 'db'
    
    db.vm.network :private_network, ip: "192.168.33.11"
    
    config.hostsupdater.aliases = ["development.db"]     
    end

# creating third VM called aws 
  config.vm.define "aws" do |aws|
    
    aws.vm.box = "bento/ubuntu-18.04"
    
    aws.vm.hostname = 'aws'
    
    aws.vm.network :private_network, ip: "192.168.33.12"
    
    config.hostsupdater.aliases = ["development.aws"] 
   end
end
```
- Save the code in vagrantfile and run below command
- ```vagrant up```
- It will take few minutes to create 3 VMs
- check the stastus with below command 
- ```vagrant status``` 
![](images/vagrant_status.jpg)
![](images/VB_status.jpg)

## Setup SSH keys/password in the host file.
There are few different ways to set up connection and we will use the fast and easy set up to get going with actual Ansible provisioning.
- Bit of a theory about SSH to have clear concept 
- ```ssh-keygen``` command will generate rsa key pair
- copy the key to server with below command
- ```ssh-copy-id``` root@host
- ```ssh-copy-id``` uses the SSH protocol to connect to the target host and upload the SSH user key.
The command edits the authorised keys file on the server. It creates the .ssh directory if it doesn't exist. It creates the authorised keys file if it doesn't exist. Effectively, ssh key copied to server.

- SSH into our VMs 
- ssh vagrant@ip – ``` ssh vagrant@192.168.33.10 or ssh vagrant@192.168.33.11 ```
- ``` ssh vagrant@db ```  ``` ssh varant@web ```
- password: ```vagrant```
![](images/vagrant_ssh.jpg)
- ```ssh vagrant@aws```
![](images/ssh_aws.png)

- WOW! We have successfully created multi-server/client environment!

# Trouble-Shooting Tips

# Let us touch base on some common trouble shooting issues before we jump back to Ansible
### Vagrant VM rebooting failure or unable to find private key etc.
- Run this command  and reboot the VM ```echo "nameserver 8.8.8.8" | sudo tee /etc/resolv.conf > /dev/null```
### You can also add following code to your vagrantfile to resolve the issue.
``` 
 config.vm.provider "virtualbox" do |v|
    v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    v.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
end
```
# Back to Ansible now
- back to Ubuntu shell 
- install tree with ```sudo apt install tree ```

- Now go to ``` cd /etc/ansible$```
- Type ```tree``` to see the file tree of ansible directory
![](images/tree.jpg)

- Ansible Inventory file - In the Ansible terminology, an inventory is a file where the target hosts of our actions are specified.
- Its default path is ```/etc/ansible/hosts```
- Open hosts file and add following entry 
``` bash 
[web]
192.168.33.10 ansible_connection=ssh ansible_ssh_user=vagrant ansible_ssh_pass=vagrant
[db]
192.168.33.11 ansible_connection=ssh ansible_ssh_user=vagrant ansible_ssh_pass=vagrant
[aws]
192.168.33.12 ansible_connection=ssh ansible_ssh_user=vagrant ansible_ssh_pass=vagrant
 ```
- In the Ansible hosts file we have three servers web, db and aws
- The IPs must match the hardcoded IPs defined in our vagrantfile to connect via SSH
- ``` ansible_connection=ssh ``` enables ansible to connect to the server with ssh key or password
- ``` ansible_ssh_user=vagrant``` ```ansible_ssh_pass=vagrant``` enable you to connect to VMs with user ID vagrant and password vagrant
![](images/ansible_hosts.jpg)

## Now let us check if we can ping our servers
- Use this Command: ``` ping 192.168.33.10 ```
- Use this Command: ``` ping 192.168.33.11 ```

![](images/ping_command.jpg)
- Great!

## Lets move on to Ansible Ad-hoc commands
- Ping all servers/target hosts Check the connection/status:
- command: ``` ansible all -m ping ```
![](images/ansible_ping_all.png)
- Amazing! We got a Pong in renponse of our Ping with a success message from all our servers
- -m is the module.
- How can we ping a particular server
- ``` ansible aws -m ping ```
![](images/ping_particular_server.png)

- To Check the name/version of all hosts OS machines
- Command: ```ansible all -a "uname -a"```
- -a should contain the command it should run which goes as an argument to command and shell.
![](images/ansible_all_uname.png)
- This is very useful command to find out the name and version of all Servers connected with Ansible, before performing any actions 

## So when do we actually need to utilise these quick Ad-hoc commands in Real life scenario?

### If we have multiple servers running in different parts of the world and you need to perform some tasks out of business hours in each country/location.

![](https://github.com/spartaglobal/Ansible/blob/master/Ansible_multi_server.png)

- for example: if you have 1 Server in USA, 1 in Singapore and 1 in Germany
- You will need to check few things before hand
- Such as date and time in each of the zone before start to perform any tasks
- Command to check date and time: ```ansible all -a "date"```
![](images/ansible_date.png)

- if you have been tasked to install mysql on all servers or 1 server
- What pre checks would be required to get done ``` Memory availability``` ```disk available space``` etc.
## How to Check the memory on host servers with Ad-hoc commands
- The following ansible ad-hoc command would help you get the desired outcome of all the hosts in the single or all hosts/servers .
- As you could see we are running the free -m command on the remote hosts and collecting the information.
- This is ```best practice``` to check the available space before installing any packages.
- command: ```ansible all -a "free -a"```
![](images/free_memory.png)
- How to check existing directories in all our servers using command inside the VMs shell from Ansible machine 
- command: ``` ansible all -m shell -a "ls -a" ```
![](images/check_dir.png)

## Amazing isn’t it! How smartly we have been communicating with our VMs with Ansible using Ad-hoc commands
- There are plenty more Ad-hoc commands available on Ansible doc page
- ```https://docs.ansible.com/ansible/latest/user_guide/intro_adhoc.html``` 

## The best advantage of Ansible is that it is agentless, therefore we did not need to install Ansible or any dependencies on our servers/VMs and we still successfully ran various different commands on the servers from our Ansible machine.
- Now we have good understanding of Ansible and the ad-hoc commands

### 	Show time! quick exercise to practice some more Ad-hoc commands before we move onto Ansible Playbooks
-	Time 5-10 minutes
-	Find out the ```UPTIME``` of db server using Ansible Ad-hoc command.
-	```update and upgrade all packages``` using Ansible Ad-hoc command.
-	TOP TIP- You can get help from above Ansible Doc link or any other source from internet if required. 

## Now its time to move onto next level to create and use Ansible Playbooks

# Ansible Playbooks

## Playbooks are a completely different way to use ansible than in ad-hoc task execution mode, and are particularly powerful. 
- Playbooks are written in ```YAML Yet Another Mark up Language```, which is easier to understand than a JSON or XML file.
- Each task in the playbook is executed sequentially for each host in the inventory file before moving on to the next task.
- Ansible Playbooks are composed of one or more plays and offer more advanced functionality for sending tasks to managed host compared  to running many ad-hoc commands. 
- Let’s create a simple Ansible playbook example that will installMySQL on the managed hosts that we had already defined in the host file.
``` 
# This is an example of ansible playbook written in YMAL

# YAML FILE STARTS WITH THREE --- DASHES(---)
---
# In this example we are targeting server called db

- hosts: db
# hosts is to define the name of your host machine, group or all servers
 
 gather_facts: yes
# gethering facts before performing any tasks
 
  become: true
 # become is used to get root permission to perform any tasks that may require admin access
 
 #Tasks are executed in order, one at a time, against all Servers matched by the host pattern
# Every task should have a name, which is included in the output from running the playbook

  tasks:
  - name: Install mysql
  
    # installing Mysql on db
    apt: pkg=mysql-server state=present

# The goal of each task is to execute a module, with very specific arguments.

- # TOP TIP: Be mindful of the INDENTATION of syntax in YML
  

```
- Save the file as playbook_installmysql.yml in ```/etc/ansible```
- Run the playbook with this command:``` ansible-playbook playbook_installmysql.yml ```
![](images/playbook_mysql.png)

- Mysql has been installed on db server, now lets login to db server from our Ansible machine via ssh
- ```ssh vagrant@db```
- Enter the passord: ```vagrant```
- Verfiy if mysql has been installed with this command: ```mysql --version```
![](images/verify_mysql.png)
## Great! 

# Its show time
- Playbooks are reusable with simple modification.

## Exercise: 20 minutes
- Create 1 playbook to install:
- Nginx on db server
- Mysql on web server
- Both packages on aws server
- Update and upgrade the packages on all the servers

