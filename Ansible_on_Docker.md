# <h1> Ansible_Installation_on_Docker_Container
########################################

Below I have documented the steps to install Ansible master on a docker container.
Alternatively you can pull an ansible Image from docker registry and start using it.

Pre-requisites - Docker to be installed already

# <h3> Steps for Ansible Master :
1) Pull Ubuntu from docker registry.
You can skip if you already have an Ubuntu image locally.

	docker pull ubuntu
 
	![ansible1](/images/ansible1.png) 

2) Run Ubuntu in interactive and detached mode

	docker run -itd --name ansible_master ubuntu /bin/bash
	


given name for the container - ansible_master
which image to use for this container - Ubuntu
what to execute in container - /bin/bash

3) To Check the Status

	docker ps

 
	![ansible2](/images/ansible_on_docker/ansible2.png)

4) To run access the container run,
	docker attach <CONTAINER ID>
	docker attach ec31b87d80ae

5) Run update command for OS

	apt update

6) On this container we need to install following software  python ansible openssh-client vim iputils-ping -y

	apt install python ansible openssh-client vim iputils-ping -y



# <h3> Steps for Ansible Slave:

1)	Spawn ansible slave with Ubuntu image with below command – 

	docker run -itd --name ansible_slave ubuntu /bin/bash

given name for the container - ansible_master 
which image to use for this container - Ubuntu
what to execute in container - /bin/bash


2)	Run below command to confirm if the new container is spawned.

	docker ps
 


3)	Run attach command to access the ansible_slave container

	docker attach <CONTAINER ID>
	docker attach 83634b7b980a


4)	Run update and install python, ssh 

	apt update ; apt install python ssh vim -y

5)	Set root password

	passwd root 

 


6)	View ssh configurations

	vi /etc/ssh/sshd_config 

	Set PermitRootLogin: yes

 

7) Start the ssh service

	Service ssh restart


 



# <h3> Other Steps on the Master Container


1)	Run below command to get the IP’s of the containers.

	Docker network inspect bridge

2)	Copy the IP address of the ansible_slave and and ping from master

	ping 172.17.0.3

 


3)	Generate ssh key

	Ssh-keygen 
	
	ssh-copy-id root@172.17.0.3


 

4)	Add slave IP in ansible hosts file at the end and save the file

 

5)	Now test ansible ping command as below. 

	ansible -m ping 172.17.0.3

 
If the result is SUCCESS (as in above screenshot), then we can run any task on the slave machine from master.



