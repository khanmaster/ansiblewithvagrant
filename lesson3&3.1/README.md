# Ansible use cases in real life.   

## In this lacture we will learn:

-	What is a Ad-hoc command 
-	Use of Ad-hoc commands in real life
-	What is inventory file and use case
-	Ansible Dir/File tree
-	Playbook’s Tasks, Modules
-	Ad-hoc commands vs playbooks
-	Installing SQL, NGINX using playbooks

## Lets move on to Ansible Ad-hoc commands
- Ping all servers/target hosts Check the connection/status:
- command: ``` ansible all -m ping ```
![](https://github.com/spartaglobal/Ansible/blob/lesson1/images/ansible_ping_all.png)
- Amazing! We got a Pong in renponse of our Ping with a success message from all our servers
- -m is the module.
- How can we ping a particular server
- ``` ansible aws -m ping ```
![](https://github.com/spartaglobal/Ansible/blob/lesson1/images/ping_particular_server.png)

- To Check the name/version of all hosts OS machines
- Command: ```ansible all -a "uname -a"```
- -a should contain the command it should run which goes as an argument to command and shell.
![](https://github.com/spartaglobal/Ansible/blob/lesson1/images/ansible_all_uname.png)
- This is very useful command to find out the name and version of all Servers connected with Ansible, before performing any actions 

## So when do we actually need to utilise these quick Ad-hoc commands in Real life scenario?

### If we have multiple servers running in different parts of the world and you need to perform some tasks out of business hours in each country/location.

![](https://github.com/spartaglobal/Ansible/blob/master/Ansible_multi_server.png)

- for example: if you have 1 Server in USA, 1 in Singapore and 1 in Germany
- You will need to check few things before hand
- Such as date and time in each of the zone before start to perform any tasks
- Command to check date and time: ```ansible all -a "date"```
![](https://github.com/spartaglobal/Ansible/blob/lesson1/images/ansible_date.png)

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

