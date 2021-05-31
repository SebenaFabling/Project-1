## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![Network Diagram](https://raw.githubusercontent.com/SebenaFabling/Project-1/main/Network%20Diagram.png "Network Diagram")

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the filebeat_playbook.yml file may be used to install only certain pieces of it, such as Filebeat.

  -  elk_playbook.yml
  - filebeat_playbook.yml
  - metricbeat_playbook.yml

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly avaliable, in addition to restricting traffic to the network.
### What aspect of security do load balancers protect?
- Load balancers distribute any traffic that comes in and distributes across multiple servers, which mitigates overloading the machine. Load balancers are used to mitigate DoS (Denial of Service) attacks and havingload balancers limit downtime, if one machine does go down the load balancers redirect the traffic to the other machine. 
 ### What is the advantage of a jump box?
- The Jump Box limits the access to virtual machines, as only certain Ip's can access the Jumpbox and all traffic must go through the Jump box before it can access the vm, this prevents the vm's from being exposed to the public internet

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the logs and system traffic.
#### What does Filebeat watch for?
- Filebeat helps create and organise log files thats can be sent to Logstash and Elasticsearch. Filebeat specifically logs information about the file systems, this includes the information about which files have changed and when. Filebeat should be installed on the VM you are trying to monitor.

#### What does Metricbeat record?
- Metricbeat monitors your servers and the services they host by collecting metrics from the operating system and services

The configuration details of each machine may be found below.


| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.0.0.4   | Linux            |
| Web 1   |  Load Balancing Web Server     |  10.0.0.5          |     Linux             | 
| Web 2     |  Load Balancing Web Server    |   10.0.0.6         |   Linux               |
| ElkVM     |  Host Elk Container/ Contains and Stores Logs         | 10.1.0.4            |   Linux               |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump Box Provisioner machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- Personal IP

Machines within the network can only be accessed by _____.
- 10.0.0.5
- 10.0.0.6
- 10.1.0.4

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | No                  | Personal IP   |
|Web 1          |     No                |     10.0.0.4                 |
|     Web 2    | No         | 10.0.0.4
|Elk VM     |   No                  | 10.0.0.4                     |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- This eliminiates human error as no configuration is performed manually, this is also a quicker and more cost effective process as it does not require each machine to be manually processed.

The playbook implements the following tasks:

- Install docker.io
- Install pip3 
- Install Docker module
- Increase virtual memory
- Download and launch a docker elk container
- Enable service docker on boot

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

Screenshot of docker ps output 
![Screenshot of docker ps Output](https://raw.githubusercontent.com/SebenaFabling/Project-1/main/container_running.PNG "docker ps output")

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- Web 1 | 10.0.0.5 
- Web 2 | 10.0.0.6

We have installed the following Beats on these machines:
- Filebeat
- Metricbeat

These Beats allow us to collect the following information from each machine:
- _TODO: In 1-2 sentences, explain what kind of data each beat collects, and provide 1 example of what you expect to see. E.g., `Winlogbeat` collects Windows logs, which we use to track user logon events, etc._
- Filebeat collects data about the file system such as if a file has been changed and when. Filebeat is often used to collect log files from specific files, such as those generated by APache Microsoft Azure tools, Nginx web servers and the MySQL  databases.
- Metricbeat is a lightweight shipper that collects metrics from the operating systems and services, such as uptime.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the filebeat-config.yml and metricbeat-config.yml file to /etc/ansible/files.
- Update the host file to include  IP addresses of the webservers and Elk server VM.
- Run the playbook, and navigate to  http://[your.ELK-VM.External.IP]:5601/app/kibana to check that the installation worked as expected.


## Which file is the playbook? Where do you copy it?
The filebeat configuration
  - filebeat-config.yml
     - copied to /etc/ansible/files/filebeat_playbook.yml
 
## Which file do you update to make Ansible run the playbook on a specific machine? 
- Update the filebeat-config.yml, specify which machine by updating the hosts files with the IP addresses of the webservers and elk. 
How do I specify which machine to install the ELK server on versus which to install Filebeat on?
## Which URL do you navigate to in order to check that the ELK server is running?
 http://[your.ELK-VM.External.IP]:5601/app/kibana

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._
- ssh azadmin@jumpboxip
- sudo docker start fervant_hermann
- sudo docker attach fervant_hermann 
- cd /etc/ansible
- ls
- nano /etc/ansible/hosts
- add group elk and add IP address of VM
- cd /etc/ansible/  
- nano /etc/ansible/elk_playbook.yml
- Configure Playbook
  - Download and lauch a docker web container
    - docker_container:
       - name: dvwa
      -  image: cyberxsecurity/dvwa 
      -  state: started
       - restart_policy: always
       - published port: 80:80