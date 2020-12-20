# Eddie-Byrd
UoMBootCamp

# Eddie-Byrd
UoMBootCamp

## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![TODO: Update the path with the name of your diagram](Images/diagram_filename.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the .yml file may be used to install only certain pieces of it, such as Filebeat.

  - Enter the filebeat playbook file. 

  ![TODO: Update the path with the name of your filebeat file](Images/filebeat_playbook.png)

![TODO: Update the path with the name of your elk file](Images/install_elkfile.png)
![TODO: Update the path with the name of your pentest file](Images/Pentest_ymlfile.png)

This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build

### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting access to the network.
- What aspect of security do load balancers protect?
    The load balancers helps with the availabiity of security
 What is the advantage of a jump box?
    The advange of the jump box is to have an orginal lauching off point for administrative tasks which creates a secure place from which to work from. 

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the logs and system traffic.
- What does Filebeat watch for?_
    Filebeat watches for log files, locations and collects log evets
- What does Metricbeat record?_
    Metricbeat records the metrics and any statisitical data from the OS and what services the servers are running. 

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.1.0.4   | Linux            |        
| Web1/DVWA| Docker   | 10.1.0.5   | Linux            |
| Web2/DVWA| Docker   | 10.1.0.6   | Linux            |
| ELKServr | ELK      | 10.2.0.5   | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jumpbox machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses: Personal IP Address 


Machines within the network can only be accessed by SSH.
- Which machine did you allow to access your ELK VM? What was its IP address?_
    The only machine that should be connected to the ElK-Server (10.2.0.5) is the Jumpbox (10.1.0.4)

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box |     No              | Personal IP| 
| Web1/DVWA|     No              | 10.1.0.5             |
| Web2/DVWA|     No              | 10.1.0.6             |
| ELKServr |     No              | 10.2.0.5& Personal IP|
### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...

Automating configuration through ansible allows for ease of use for those who are less technical. Playbooks are used to configure multiple machines through a single command once it is configured. 

The playbook implements the following tasks:
- In 3-5 bullets, explain the steps of the ELK installation play. E.g., install Docker; download image; etc._
1. Creating a new virtural machine, elkserver noting the Private IP (10.2.0.5) and Public IP (00.000.00.000). The Private IP will be used to SSH into the virtural machines and the Public IP to connect to the Kibana Portal to view the Metrics and Syslogs.
2. The elk/docker container should be downloaded and configured into the hosts.conf file adding a new NSG elkservers and Private IP, 10.2.0.5. Create a new ansible-playbook elk.yml that will download, install, configure the elkserver which will map ports, 5601, 9200, 5044 and start the container.
3. After installing the elk server, start and attach the container. To verify the container is up and running, SSH into the container from the Jumpbox.  When in the elk server run the "sudo docker ps"
4. Create a new inbound security rule that allows ports, 5601 and 9200 which will allow access from your personal network. 
5. Enter your "Public IP:5601" into the browser to access the Kibana Portal Site.   


The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![TODO: Update the path with the name of your screenshot of docker ps output](Images/docker_ps_output.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- List the IP addresses of the machines you are monitoring_
  10.1.0.5, & 10.1.0.6,

We have installed the following Beats on these machines:
- Specify which Beats you successfully installed_
  Filebeat 

These Beats allow us to collect the following information from each machine:
- In 1-2 sentences, explain what kind of data each beat collects, and provide 1 example of what you expect to see. E.g., `Winlogbeat` collects Windows logs, which we use to track user logon events, etc._
Filebeat is a lightweight shipper used for forwarding and centralizing log data. It monitors log files or locations depending on what is specified, collects log events, and forwards them to either Elasticsearch or Logstash for indexting


### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy filebeat.yml to the "/etc/ansible/roles/files" directory.
- Update the configuration file so it includes the private IP of the elkserver to the Elasticsearch and Kibana sections of the configuration file.
- Run the playbook and navigate to the elkserver to confirm that the installation worked as expected by using "sudo docker ps" in the command line

Answer the following questions to fill in the blanks:_
- _Which file is the playbook?
  The playbook is called filebeat-playbook.yml
- _Where do you copy it?_
  The file is copied in the "/etc/ansible/hosts" directory. 
- _Which file do you update to make Ansible run the playbook on a specific machine? 
  The filebeat.yml must be updated. This is a configuration file responisble for running the ansible-playbook on the elk server.   
-_How do I specify which machine to install the ELK server on versus which to install Filebeat on?_
  When you update the host.cfg file in the ansible directory you will also need to create a new group (elkserver) and add the Private IP of the elkserver to that group. After configuraing the Filebeat.yml file you then to need add the Private IP of the elk server in two lines of the .yml file. 
- _Which URL do you navigate to in order to check that the ELK server is running?
  To verify if the elk server is running you use the Public IP, 0.0.0.0:5601. 

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._
 1. SSH sysadmin@Jumpbox(PrivateIP Address)
 2. sudo docker container list -a (used to locate ansible container)
 3. sudo docker start (name of container)
 4. sudo docker attach (name of container)
 5. cd /etc/ansible
 6. ansible-playbook elk.yml to configure the ele server 
 5. cd /etc/ansible/roles/
 6. ansible-playbook-filebeat-playbook.yml (installs filebeat)
 7. In a new web browser use the Elk server's Public IP, (0.0.0.:5601)
