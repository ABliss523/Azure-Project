# Azure-Project
Rice University CyberSecurity Azure Project
## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![My Azure Infrastructure](https://user-images.githubusercontent.com/85312602/142678015-4fd84a4e-ccaf-4eca-892d-bdce2d58dcf0.PNG)

These files have been tested and used to generate a live ELK deployment on Azure. Which can be used to recreate the entire deployment pictured above or select portions of the playbook file, such as Filebeat.
- https://github.com/ABliss523/Azure-Project/blob/main/Ansible/FileBeatPlaybook.yml

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancers protect the system from DDoS attacks through evenly shifting traffic amoung the servers, this ensures the application will be highly responsive in addition to restricting access to the network.
- The Jump Box only gives access to the user with the correct SSH key connecting from the IP address 20.119.48.89, that can be monitored and secured, restricting it's access to the public.

Integrating an ELK server allows users to easily monitor the vulnerability of the Virtual Machines for changes within the logs and system traffic.
- Installed within the servers is a Filebeat, which monitors specified information in the file system that has been changed and is forwarded to Elasticsearch or Logstash for indexing.
- Metricbeat is also installed to collect the proper metrics and statistics within the system and then produces visualization of their performance to the output specified.

The configuration details of each machine may be found below.

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.0.0.4   | Linux Ubuntu     |
| Web-1    | Server   | 10.0.0.7   | Linux Ubuntu     |
| Web-2    | Server   | 10.0.0.8   | Linux Ubuntu     |
| ELK      | Server   | 10.1.0.4   | Linux Ubuntu     |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump-Box machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- 20.119.48.89

Machines within the network can only be accessed by the Jump-Box Provisioners Internal IP Address: 10.0.0.4.
- The ELK VM can be accessed by the Jump-Box-Provisioner 10.0.0.4:22 and the Workstation IP 20.119.48.89:5601

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | Yes                 | 20.119.48.89         |
| Web-1    | No                  | 10.0.0.4/20.119.48.89| 
| Web-2    | No                  | 10.0.0.4/20.119.48.89|  
| ELK      | No                  | 10.0.0.4/20.119.48.89| 

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because it reduces the chances of "human error" and simplifies configuring multiple machines that allow full automation within a server.

The playbook implements the following tasks:
- Install docker.io: Which is the docker engine that will be used for running the containers.
- Install python 3_pip: Allows for additional docker modules and containers to be installed efficiently.
- Increase memory/ Use more memory: Increases the memory of the VM to 262144 for Elk to operate effectivley.
- Download and launch Elk container: Downloads and launches the Elk docker on boot

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![ELK_Docker](https://user-images.githubusercontent.com/85312602/142748151-d97b0346-3982-41aa-8588-04f2b066769c.PNG)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- Web-1: IP address 10.0.0.7
- Web-2: IP address 10.0.0.8

We have installed the following Beats on these machines:
- Filebeat
- Metricbeat

These Beats allow us to collect the following information from each machine:
- Filebeat collects system log information from Web-1 and Web-2 specifying any changes within the file and is reported to Elasticsearch or Logstash. Metricbeat collects and reports the metrics and statistics of the operating system and processes running into a specified output.

### Using the Playbook
In order to use the playbook, an Ansible control node needs to already be configured. Assuming the control node is provisioned: 

SSH into the control node and follow the steps below:
- Copy the filebeat-config.yml file to /etc/ansible/files/filebeat-config.yml
- Update the filebeat-config.yml file to include host 10.1.0.4:9200 with username "elastic" and password "changeme" and setup.kibana host to 10.1.0.4:5601
- Run the playbook, and navigate to http://52.232.134.134/app/kibana to check that the installation worked as expected.


#### Which file is the playbook? Where do you copy it?
- filebeat-playbook.yml
- Copy to /etc/ansible/roles/filebeat-playbook.yml
#### Which file do you update to make Ansible run the playbook on a specific machine?
- The /etc/ansible/hosts file
#### How do I specify which machine to install the ELK server on versus which to install Filebeat on?
- By adding the private IP address of Web-1 and Web-2 under [webservers] and the private IP address of ELK under [elk] within the hosts file.
#### Which URL do you navigate to in order to check that the ELK server is running?
- http://52.232.134.134/app/kibana
#### Commands used to run, download, and update the playbook
- Download: curl https://gist.githubusercontent.com/slape/5cc350109583af6cbe577bbcc0710c93/raw/eca603b72586fbe148c11f9c87bf96a63cb25760/Filebeat > /etc/ansible/filebeat-config.yml
- Update: nano into the hosts and configuration file and replace the localhost IP address with the ELK Server's IP address
- Run: ansible-playbook playbook_file.yml
