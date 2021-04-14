## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

https://github.com/ritzmeya/ELK_2021/blob/main/Diagram/ELK_SERVER_DEPLOYMENT.pdf

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the filebeat-playbook.yml file may be used to install only certain pieces of it, such as Filebeat.

Install Elk.yml
https://github.com/ritzmeya/ELK_2021/blob/main/Ansible/install-elk.yml

filebeat-playbook.yml
https://github.com/ritzmeya/ELK_2021/blob/main/Ansible/filebeat-playbook.yml

filebeat-config.yml
https://github.com/ritzmeya/ELK_2021/blob/main/Ansible/filebeat-config.yml

Ansible-config.yml
https://github.com/ritzmeya/ELK_2021/blob/main/Ansible/ansible_config.yml
![image](https://user-images.githubusercontent.com/74203852/114756437-bb350980-9d28-11eb-8de4-a2c5ca95e15c.png)


This document contains the following details:-
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly efficient, available and reliable in addition to restricting traffic to the network.

A load balancer acts as the “traffic cop” sitting in front of the servers and routing client requests across all servers capable of fulfilling those requests in a manner that maximizes speed and capacity utilization and ensures that no one server is overworked, which could degrade performance. If a single server goes down, the load balancer redirects traffic to the remaining online servers. When a new server is added to the server group, the load balancer automatically starts to send requests to it.

What aspect of security do load balancers protect? 
Load Balancing plays an important security role as computing moves evermore to the cloud. The off-loading function of a load balancer defends an organization against distributed denial-of-service (DDoS) attacks. It does this by shifting attack traffic from the corporate server to a public cloud provider. In this manner, a load balancer performs the following functions:
-Distributes client requests or network load efficiently across multiple servers
-Ensures high availability and reliability by sending requests only to servers that are online
-Provides the flexibility to add or subtract servers as demand dictates

What is the advantage of a jump box?
Jump box is way to decrease the ability of hackers or their malware creations to steal admin credentials and take over a network environment. Over the years, concept of a traditional “jump box” has morphed into an even more comprehensive and locked-down “secure admin workstation” (or SAW). A SAW is a computer the admin must originate from before performing any administrative task or connecting to any other administered server or network.
The advantage of a jump box is to have a secure computer that all admins first connect to before launching any administrative task or use as an origination point to connect to other servers or untrusted environments. 

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the servers and system logs.

What does Filebeat watch for?
Filebeat monitors the log files or locations that you specify, collects log events, and forwards them either to Elasticsearch or Logstash for indexing.

What does Metricbeat record?
Metricbeat is a lightweight shipper that can be installed on servers to periodically collect metrics from the operating system and from services running on the server. Metricbeat takes the metrics and statistics that it collects and ships them to the specified output, such as Elasticsearch or Logstash.

The configuration details of each machine may be found below.

| Name        | Function | IP Address     | Operating System |
|-------------|----------|----------------|------------------|
| Jump Box    | Gateway  |13.82.110.28    |    Linux         |
| Web1        |  VM      |20.47.108.201   |    Linux         |
| Web2        |  VM      |20.47.108.201   |    Linux         |
| ELK-Machine |  VM      |104.42.63.135   |    Linux         |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the jump box machine (Red Team VM) can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- 45.72.xxx.xx (personal home IP address)

Machines within the network can only be accessed by Ansible Container.
- Red Team VM machine was allowed to access ELK VM. IP address 13.82.110.28

A summary of the access policies in place can be found in the table below.

| Name                   |Port | Publicly Accessible  | Allowed IP Addresses |
|------------------------|-----|----------------------|----------------------|
|Jump Box                |22   | Yes                  | 10.0.0.4             |
|Red Team Load Balancer  |     |Yes                   |                      |
|Web 1                   |80.  |yes. via load balancer|20.185.179.54         |
|Web 2                   |22.  |yes. Via load balancer|20.185.179.54         |
|Elk VM                  |5601.| No                   |                      |
|                        |     |45.72.230.103         |                      |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because
Ansible uses automated workflows, provisioning, and more to make orchestrating tasks easy. Further, once infrastructure has been defined using the Ansible playbooks, it is possible to use that same orchestration wherever required, because of the portability of Ansible playbooks.

The playbook implements the following tasks:
1. Config Web VM with Docker
2. Install pip3
3. Install Docker python module
4. Download and launch a docker web container
5. Enable docker service
The playbook implements the following tasks:

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.
https://github.com/ritzmeya/ELK_2021/blob/main/Screenshots/Screen%20Shot-AFTER-RUNNING-ANSIBLECONFIGFILE.png


### Target Machines & Beats
This ELK server is configured to monitor the following machines: 

20.47.108.201 - web 1
20.47.108.201 - web 2

We have installed the following Beats on these machines:
1. Filebeats
2. Metricbeats 

These Beats allow us to collect the following information from each machine:

Filebeat helps generate and organize log files to send to Logstash and Elasticsearch. Specifically, it logs information about the file system, including which files have changed and when. Filebeat is often used to collect log files from very specific files, such as logs generated by Apache, Microsoft Azure tools, the Nginx web server, or MySQL databases. We used filebeat to monitor the Apache server and MySQL database logs generated by DVWA. Since Filebeat is built to collect data about specific files on remote machines, it must be installed on the VMs we want to monitor. We installed Filebeat on the DVWA container that created.

Metricbeat is a lightweight shipper that you can install on your servers to periodically collect metrics from the operating system and from services running on the server. Metricbeat takes the metrics and statistics that it collects and ships them to the output that you specify, such as Elasticsearch or Logstash.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:

Copy the filebeat configuation file from Ansible Cpntainer to Web VMs where Filebeat was installed.
Update the filebeat-playbook.yml file to include 
(i) enable and configure system module
(ii) set up filebeat
(iii) start filebeat service
(iv) enable service filebeat on boot
Run the playbook, and navigate to ELK server GUI to check that the installation worked as expected.

https://github.com/ritzmeya/ELK_2021/blob/main/Screenshots/Screen%20Shot%20filebeat-successfully-sent-to-kibana.png

- _Which URL do you navigate to in order to check that the ELK server is running?
http://104.42.63.135:5601/app/kibana#/home/tutorial/systemLogs


