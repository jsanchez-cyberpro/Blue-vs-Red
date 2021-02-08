## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![alt text](https://github.com/cyberprotocols/cyberpro_inc/blob/main/Diagrams/Virtual_Network_Map.JPG)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the filebeat-playbook.yml file may be used to install only certain pieces of it, such as Filebeat.
- install-elk.yml 
- filebeat-playbook.yml
- metricbeat-playbook.yml 

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available and redundent, in addition to restricting unwanted traffic to the network.
-Load Balancers offers off-loading function defending an organization against distributed denial-of-service (DDoS) attacks. 
-Software load balancers with cloud offload provide efficient perimeter firewall functionalities at an efficient and cost effictive solution. 

-A JumpBox server is a hardened and monitored device that spans two dissimilar security zones and provides a controlled means of access between them. 
-The JumpBox server acts as a single audit point for traffic and also a single place where user accounts can be managed. 
-It streamlines maitainance since its maintained on a single system. Therefore, when an update to the SAN management software is available, only one single 
system is requiered. 

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the system logging and system Services.
- Filebeat is a lightweight shipper that forwards and centralizes log data. 
- Filebeat inputs looks in the location you've specified for log data. For each log that filebeat locates, filebeat starts a harvester. 
- Metricbeat is a lightweight shipper that is install on the elk server to collect metrics from the OS and from services running on the server.
- Metricbeat takes the metrics and statistics that it collects and ships them out to the output that yoou specify, such as Elasticsearch or logstash.

The configuration details of each machine may be found below.
| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.0.0.4   | Linux            |
| Web-1    | webserver| 10.0.0.5   | Linux            |
| Web-2    |webserver2| 10.0.0.6   | Linux            |
|Elk-Server| ElkDocker| 10.1.0.4   | Linux/Docker     |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the JumpBox machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- 98.252.34.13

Machines within the network can only be accessed by Jumpbox .
- JumpBox 10.0.0.4

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | Yes                 | 98.252.34.13 13.82.149.52         |
| elkserver| yes                 | 10.0.0.4             |
|          |                     |                      |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because
It's a free open source tool that is simple to set up and use. It's poweful yet flexible way to model and automate complex IT workflows.
 
In this case ansible allowed us to easily deploy our elk machine without the need to write custom code, all by listing the tasks required
to be done by just writing a playbook. Ansible was able to figure our how to get the system to the state we wanted to be in. This can be
very easily repeated with the ansible playbook. Allowing constant and congruent deployment.

The playbook implements the following tasks:
- Set the vm.max_count to 262144 *This configures the machine being configured to use more memory.
Also sysctl module and configure it so that the setting is automatically run if the VM has been
restarted.
- install apt packages:
	a. docker.io: The Docker engine, used for running containers.
	b. Python3-pip: Package used to install python software. 
- install the following pip packages:
	a. docker python client for docker. Required by Ansible to control the state of docker containers. 
- Download the docker container:
	a. sebp/elk:761
-Configure the container to start with the following port mapping:
	a. 5601:5601
	b. 9200:9200
	c. 5044:5044

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![alt text](https://github.com/cyberprotocols/cyberpro_inc/blob/main/Ansible/docker_ps_screenshot.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- Web-Server1 10.0.0.5 
- Web-Server2 10.0.0.6

We have installed the following Beats on these machines:
- Web-Server1 10.0.0.5 
- Web-Server2 10.0.0.6

These Beats allow us to collect the following information from each machine:
- periodically collect metrics from the operating system and from services running on the server. Metricbeat takes the metrics and statistics that it collects and ships them to the output that you specify, such as Elasticsearch or Logstash.
  Metricbeat helps you monitor your servers by collecting metrics from the system and services running on the server, such as:
	1.Apache
	2.HAProxy
	3.MongoDB
	4.MySQL
	5.Nginx
	6.PostgreSQL
	7.Redis
	8.System
	9.Zookeeper
Beat will collect logs which we use to track user logon events, etc. We are also collecting network data and Linux audit framework data.

### Using the Playbook
In order to use the playbook, you will need to have an Anscible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the filebeat configuration file to Web-vms where we installed filebeat.
/etc/filebeat/filebeat.yml
- Update the filebeat-playbook to include:
a Download the .deb file from artifacts.elastic.com
b Run filebeat modules enable system
c filebeat setup
d service metricbeat start

- Run the playbook, and navigate to the filebeat installation page on the ELK serve GUI. 
a Verify that the playbook is completing step 1-4 
b on the same page, scroll to step 5: Module status and click check data.
C scroll to the bottom and click on verify data. 
D if the ELK stack was successfully receiving logs, you would have seen:
‘data successfully received from this module.  

- Filebeat-playbook. They are located in /etc/filebeat/filebeat.yml

- To run Ansible in a specific machine, we need to edit the host file first and create a specific group. To install ELK in a specific machine, edit the install-elk.yml file. Under host specify the elk server that was created in the host file. This will prevent the elk server installation in unwanted machines. Likewise in the filebeat-playbook.yml file, under the hosts: specify the targeted hosts, which was created in the hosts file. 

- To check that the ELK server is running open your browser and navigate to http://13.83.48.12:5601//app/kibana. 


