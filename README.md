## Scripts

# Cloud Network
This is a collection of Linux Scripts and Ansible Scripts from my CyberClass.

Most of the scripts are used to configure cloud servers with differnt docker containers.

The final setup was 4 servers running vulnerable DVWA containers along with a jump box and a server running an ELK stack container.

## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![NETWORK TOPOLOGY Diagram](https://drive.google.com/file/d/19ymN3wtI1f2qgB4f8Kp2x_-IhvDYlhxt/view?usp=sharing)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the _install_elk.yml Playbook____ file may be used to install only certain pieces of it, such as Filebeat.

  - _TODO: Enter the playbook file._
---
# install_elk.yml
- name: Configure Elk VM with Docker
  hosts: elkservers
  remote_user: azureuser
  become: true
  tasks:
    # Use apt module
    - name: Install docker.io
      apt:
        update_cache: yes
        name: docker.io
        state: present

      # Use apt module
    - name: Install pip3
      apt:
        force_apt_get: yes
        name: python3-pip
        state: present

      # Use pip module
    - name: Install Docker python module
      pip:
        name: docker
        state: present

      # Use command module
    - name: Increase virtual memory
      command: sysctl -w vm.max_map_count=262144

      # Use sysctl module
    - name: Use more memory
      sysctl:
        name: vm.max_map_count
        value: "262144"
        state: present
        reload: yes

      # Use docker_container module
    - name: download and launch a docker elk container
      docker_container:
        name: elk
        image: sebp/elk:761
        state: started
        restart_policy: always
        published_ports:
          - 5601:5601
          - 9200:9200
          - 5044:5044



This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly ___Available__, in addition to restricting __Access___ to the network.
- _TODO: What aspect of security do load balancers protect? What is the advantage of a jump box?_
Accessibilty and jump box is a single point of entry
Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the __files ___ and system __metrics ___.
- _TODO: What does Filebeat watch for?_ any changes to the file system
- _TODO: What does Metricbeat record?_any metric i configured for this network

The configuration details of each machine may be found below.

NAME/FUNCTION/IP ADDRESS(ES)/OPERATING SYSTEM

Jump Box/Gateway/10.0.0.4 & 104.209.173.174/Linux
WebWM1/Ansible Container VM/10.0.0.6 & 52.184.193.139/Linux
WebWM2/Ansible Container VM/10.0.0.7 & 52.184.193.139/Linux
WebWM3/Ansible Container VM/10.0.0.9/Linux
Elk-Server/ELastic Stack Server/10.2.0.4 & 104.210.59.141/Linux   

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the ____Jump box _ machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- _TODO: Add whitelisted IP addresses_MY Personal IP Address (not posted for security reasons)

Machines within the network can only be accessed by __Jumpbox___.
- _TODO: Which machine did you allow to access your ELK VM? What was its IP address?_Ansible docker container in the Jumpbox

A summary of the access policies in place can be found below.

NAME/PUBLICLY ACCESSIBLE (YES or NO)/ALLOWED IP ADDRESSES

Jump Box/YES/Whitelisted IP (My Personal IP)
WebVM1/NO
WebVM2/NO
WebVM3/NO
Elk-Server/YES/104.210.59.141:6501

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...Scaling and Security
- _TODO: What is the main advantage of automating configuration with Ansible?_

The playbook implements the following tasks:
- _TODO: In 3-5 bullets, explain the steps of the ELK installation play. E.g., install Docker; download image; etc._
(Please see install_elk.yml file in the Ansible folder)
- Configure Elk VM with Docker
- Install docker.io
- Install pip3
- Install Docker python module
- Increase virtual memory
- Use more memory
- download and launch a docker elk container

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.
do sudo docker start then run docker ps
![sudo docker ps results](https://drive.google.com/file/d/1ftcie_bzPdfLm1G86Bi7k4Q8JecUq43L/view?usp=sharing)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- _TODO: List the IP addresses of the machines you are monitoring_
10.0.0.6 (WebVM1)
10.0.0.7 (WebVM2)
10.0.0.9 (WebVM3)

We have installed the following Beats on these machines:
- _TODO: Specify which Beats you successfully installed_
filebeat, metricbeat and packetbeat

These Beats allow us to collect the following information from each machine:
- _TODO: In 1-2 sentences, explain what kind of data each beat collects, and provide 1 example of what you expect to see. E.g., `Winlogbeat` collects Windows logs, which we use to track user logon events, etc._
- Metric beat monitors system-level CPU usage, memory, file system, disk IO, and network IO statistics for every process running on your systems.
- Filebeat is the logging agent generating the log files, tailing them, and forwarding the data to either Logstash for more advanced processing or directly into Elasticsearch for indexing
- Packetbeat provides real-time analytics for web, database and other network protocols

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below: Jumbox - ansible container
- Copy the __config ___ file to ___/etc/ansible__.
- Update the ___Hosts__ file to include the Elk-Server ip address under the elkservers header
- Run the playbook, and navigate to _url to kibana port 5601___ to check that the installation worked as expected.

_TODO: Answer the following questions to fill in the blanks:_
- _Which file is the playbook? Where do you copy it?_ install_elk.yml found in /etc/ansible/
- _Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?_
In the hosts file you would create a new header underneath the webservers header, under which you would enter the IP of the specific machine you want the the Elk server on.
- _Which URL do you navigate to in order to check that the ELK server is running? <elk public ip port 5601> <104.210.59.141:5601>

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._
