# CybersecurityProject1
## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

<img src="Diagrams/Project1.drawio.png">

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the yml file may be used to install only certain pieces of it, such as Filebeat.

  - Install-elk.yml

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly reliable and available, in addition to restricting unauthorized access to the network.
- _What aspect of security do load balancers protect? It adds resiliency by shifting traffic from one server to another, which defends against DDoS attacks. It also helps prevent overloading of servers. What is the advantage of a jump box?_ Jump box ensures security. They are highly secure computers that admin connect to before launching any administrative tasks.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the network and system logs.
- _What does Filebeat watch for?_ It monitors the log files and locations that you specify and collects the log events. It watches for any changes in the file system.
- _What does Metricbeat record?_ It records metrics and stats that it collects and forwards them to which ever output you specify.

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.





| Name      | Function | IP Address         | Operating System |
|-----------|----------|--------------------|------------------|
| JumpBox   | Gateway  | 10.0.0.4 Private   | Linux            |
|           |          | 20.121.14.98 Public|                  |
| Web1      |   VM     | 10.0.0.5           | Linux            |
| Web2      |  VM      | 10.0.0.6           | Linux            |
| Web3      |  VM      | 10.0.0.10          | Linux            |
| Web4 (Elk)|Monitoring| 10.1.0.4 Private   | Linux            |
|           |          | 13.64.67.132 Public|                  |


### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump Box Provisioner machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- Public IP 20.121.14.98

Machines within the network can only be accessed by SSH from the Jump Box.
-  Which machine did you allow to access your ELK VM?  Jump Box Provisioner What was its IP address?_ 10.0.0.4

A summary of the access policies in place can be found in the table below.

| Name               | Publicly Accessible | Allowed IP Addresses |
|--------------------|---------------------|-------------------------|
| JumpBoxProvisioner | Yes                 | 10.0.0.4/ 20.121.14.98  |
| Web1               | No                  | 10.0.0.5                |
| Web2               | No                  | 10.0.0.6                |
| Web3               | No                  | 10.0.0.10               |
| Web4(elk)          | Yes                 | 10.1.0.4/ 13.64.67.132  | 

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- What is the main advantage of automating configuration with Ansible?_ Automation with Ansible simplifies complex IT tasks. It allowed us to deploy multiple VMs using the same playbooks.

The playbook implements the following tasks:
- Install docker.io – Docker engine is used to run containers on all   network machines.
- Install Python-pip – Package used to install Python software on all network machines.
- Install docker container: elk
- Launch docker container: elk
- Install ‘Beats’ containers on the VMs – Web1 Web2 Web3.


The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

<img src="Desktop/CybersecurityProject1/Ansible/sudo_docker_ps.png">

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- 10.0.0.5  Web1
- 10.0.0.6  Web2
- 10.0.0.10 Web3

We have installed the following Beats on these machines:
- Filebeat
- Metricbeat

These Beats allow us to collect the following information from each machine:
- Filebeat monitors logs files or locations, collects log events from specific files, and forwards them to Elasticsearch for indexing. It will monitor the web log data. 
- Metricbeat collects metrics and stats from the operating system and services running on the server. It will detect any SSH log in attempts.
 
### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the install-elk.yml file to /etc/ansible
- Copy the filebeat-playbook.yml file to /etc/ansible/roles
- Copy the metricbeat-playbook.yml file to /etc/ansible/roles
- Update the hosts file to include the webservers and elk machine
 /etc/ansible/hosts should have the following:
  - [elk]
   - 10.1.0.4 ansible_python_interpreter=/usr/bin/python3
  - [webservers]
   - 10.0.0.5 ansible_python_interpreter=/usr/bin/python3
   - 10.0.0.6 ansible_python_interpreter=/usr/bin/python3
   - 10.0.0.10 ansible_python_interpreter=/usr/bin/python3

- Run the playbooks and SSH into Web4 (elk machine), then run docker ps to check that the installation worked.



- _Which file is the playbook? install-elk.yml, filebeat-playbook.yml, and metricbeat-playbook.yml  
- Where do you copy it?_ /etc/ansible and /etc/ansible/roles
- _Which file do you update to make Ansible run the playbook on a specific machine? Ansible hosts file at /etc/ansible/hosts to configure specific machines or groups. 
-How do I specify which machine to install the ELK server on versus which to install Filebeat on?_The playbook will specify which machine to install to based on the hosts file.
- _Which URL +do you navigate to in order to check that the ELK server is running? http://13.64.67.132:5601/app/kibana

The specific commands the user will need to run to download the playbook, update the files, etc._

- SSH into JumpBoxProvisioner: sudo AdminGrant@20.121.14.98
- sudo docker start kind_mclean
- sudo docker attach kind_mclean

- cd /etc/ansible/
- Create or update playbook: nano install-elk.yml
- Run: ansible-playbook install-elk.yml

- cd /etc/ansible/roles
- Create or update playbooks: nano filebeat-playbook.yml   nano metricbeat-playbook.yml
- Run: ansible-playbook filebeat-playbook.yml
- Run: ansible-playbook metricbeat-playbook.yml
