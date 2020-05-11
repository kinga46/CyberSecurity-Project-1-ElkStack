## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.
![Network Diagram](https://github.com/kinga46/CyberSecurity/blob/master/Diagrams/Network_Diagram.png)
These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either 
recreate the entire deployment pictured above or specific files can be used as a guide to install only 
components of the deployment such as Filebeats. 

[Playbook for installing elk](https://github.com/kinga46/CyberSecurity-Project-1-ElkStack/blob/master/Files/install-elk.yml) 

This document contains the following details: 

- Description of the Topology
    
- Access Policies
    
- ELK Configuration
    
- Beats in Use
    
- Machines Being Monitored
    
- How to Use the Ansible Build
    
### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the Damn 
Vulnerable Web Application. Load balancing ensures that the application will be highly stable, in addition to 
distributing demand across secure networks. Load balancers also provide security for the system, keeping the 
networks protected from the open web and balancing the incoming traffic preventing DDOS attacks, thereby 
providing a more efficient application. Integrating an ELK server allows users to easily monitor the vulnerable 
VMs for changes to the log files and system performance with the use of Filebeat and Metricbeat. The 
configuration details of each machine may be found below.

|	Name | Function | IP Address Public | IP Address Private | Operating System| 
| ----------|:------------:|:--------------:|:----------:|-----------:|
|JumpBoxProvisioner | Gateway | 138.91.83.253 | 10.0.0.4 | Linux Ubuntu |
| DVWA-VM1 | WebApp | 104.40.68.140 | 10.0.0.9 | Linux Ubuntu|
| DVWA-VM2 | WebApp | 10.42.41.38 | 10.0.0.6 | Linux Ubuntu |
| RedTeamELKEast | ELK | Server | 40.71.28.227 | 10.0.1.4 | Linux Ubuntu|
  
Access Policies The machines on the internal network are not exposed to the public Internet. Only the 
JumpBoxProvisioner machine can accept connections from the Internet. Access to this machine is only allowed from 
the following IP addresses: 

- Public IP: Personal PC Public IP
    
Machines within the network can only be accessed by JumpBoxProvisioner. 

- Public IP: 138.91.83.253   
- Private IP: 10.0.0.4
    
A summary of the access policies in place can be found in the table below.
| Name | Publicly Accessible | Allowed IP Addresses|
| ------|:------:|------:|
| JumpBoxProvisioner |No | Personal PC Public IP|
| RedTeamELKEast | No | Personal PC Public IP, 38.91.83.253, 104.40.68.140, 104.42.41.38| 
| RedTeamELKEast| Yes | 40.118.206.133|
|DVWA-VM1 | No | 10.0.0.4 40.118.206.133|
| DVWA-VM2 | No | 10.0.0.4 40.118.206.133|


### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is 
advantageous because of the simple syntax used in creating the playbooks in YAML. The playbook implements the 
following tasks: 

- Increasing the memory of the machine
    
- Install pip
    
- Install docker python module
    
- Download and launch docker web container
    
The following screenshot displays the result of running docker ps after successfully configuring the ELK 
instance.

![docker ps](https://github.com/kinga46/CyberSecurity-Project-1-ElkStack/blob/master/Images/docker_ps_image.PNG)
  
### Target Machines & Beats

This ELK server is configured to monitor the following machines: 

- DVMA-VM1    
- DVMA-VM2
    
We have installed the following Beats on these machines: 

- Filebeat
    
Filebeats allow us to collect the following information from each machine: 

- Filebeat collects and forwards log data from the machines it is installed on and sends this information to the ELK stack server for processing. 
This information can then be viewed through Kibana through customizable charts and tables.
    
### Using the Playbook

In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you 
have such a control node provisioned: 

 SSH into the control node and follow the steps below: 

- Copy the filebeatconfig.yml file to /etc/ansible/files/.
    
- Update the /etc/ansible/host file to include the private IP addresses of the DVWAs
    
- Run the playbook, and navigate to [http://40.118.206.133:5601](http://40.118.206.133:5601) to check that the 
installation worked as expected.
   
 ![Kibana](https://github.com/kinga46/CyberSecurity-Project-1-ElkStack/blob/master/Images/Kibana_image.png)
 
### Downloading the playbook and updating files

SSH into JumpBoxProvisioner from Local Desktop: 

- ssh sysadmin@138.91.83.253
    
View list of docker containers: 
- sudo docker container list -a
    
  
Start docker: 
- sudo docker start upbeat_morse
    
  
Attach to docker: 
- sudo docker attach upbeat_morse
    
  
Add DVWAs to hosts file: 
- nano /etc/ansible/hosts
- add additional private IPs under [webservers]
    
  
Run playbook to update elk: 
- ansible-playbook /etc/ansible/install-elk.yml
    
  
Copy filebeat configuration file: 
- copy filebeatconfig.yml file to /etc/ansible/files/
    
Run playbook for filebeat: 
- ansible-playbook /etc/ansible/roles/filebeatplaybook.yml
    
Open Kibana in browser:
-   [http://40.71.28.227:5601/](http://40.71.28.227:5601/)
