## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

https://github.com/jcurzon2317/Lucid_Kepler/blob/main/Diagrams/Network_Diagram.drawio
https://drive.google.com/file/d/1hrtEZqj0Gm896VXj3x2RpwpxT0fr7dep/view?usp=sharing

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the playbook files may be used to install only certain pieces of it, such as Filebeat.

  - _Lucid_Kepler._

This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build

### Diagram 
https://github.com/jcurzon2317/Lucid_Kepler/blob/main/Diagrams/Network_Diagram.drawio.png
https://drive.google.com/file/d/1J_1URhqTgpaRfA_N-nvWosqtNTaAt3QJ/view?usp=sharing

### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting access to the network.
- _The Load Balancer ensures the network is available by redirecting traffic among the three webclients/servers before they becomes overloaded, either by legitimate traffic or an attack such as Denial of Service.
The advantage of using a Jump Box is that only those authorized access to the Jump Box can access certain aspects of the network, in theory._

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the file system and system settings.
- _Filebeat monitors log files, gathers log events and forwards them.
- _Metricbeat records system and applications metrics, to be viewd later, such as cpu, memory, disk space and network usage. It also allows monitoring of other installed applications.

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name        | Function  | IP Address | Operating System |
|----------   |---------- |------------|------------------|
| Jump Box    | Gateway   | 10.0.0.1   | Linux            |
| Web1 (DVWA) | Web Server| 10.0.0.5   | Linux            |
| Web2 (DVWA) | Web Server| 10.0.0.6   | Linux            |
| Web3 (DVWA) | Web Server| 10.0.0.8   | Linux            |
| ELK_VM      | Monitoring| 10.1.0.4   | Linux            |


### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump Box machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- 72.19.130.229

Machines within the network can only be accessed by other computers onn the network.
- Web1,Web2,Web3 all can send data over port 9200 to the ELk_VM
- The Jump Box can access any computer on the Network
-The load balancer forwards to one of three Web VMs running DVWA

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible     | Allowed IP Addresses       |
|----------|-------------------------|----------------------------|
| Jump Box | Yes/No                  |   72.19.130.229            |
|  Web1    |Yes through Load Balancer| 10.0.0.1-254 10.1.0.1-254  |
|  Web2    |Yes through Load Balancer| 10.0.0.1-254 10.1.0.1-254  |
|  Web3    |Yes through Load Balancer| 10.0.0.1-254 10.1.0.1-254  |
|  ELK_VM  | No                      | 10.0.0.1-254 10.1.0.1-254  |


### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because it allows us to configure new machines that may be added later or replicate this setup at another time.

The playbook implements the following tasks:
- Installs Docker
- Installs python-pip3
- Increases the amount of system memory to be used
- Sets up and ensures it runs after system boots up

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

https://github.com/jcurzon2317/Lucid_Kepler/blob/main/Ansible/Images/docker_ps_output.png
https://drive.google.com/file/d/17KrrVUUuoNeTK-2RJdB3ee7sVVtqTz5u/view?usp=sharing

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- Web1 10.0.0.5
- Web2 10.0.0.6
- Web3 10.0.0.8

We have installed the following Beats on these machines:
- Filebeat
- Metricbeat

These Beats allow us to collect the following information from each machine:
- Filebeats monitors log files and their file paths and collects and then forwards them to, in this case, Elasticsearch.
- Metricbeat collects metrics from the OS and services that are running and forwards them to, in this case, Elasticsearch.
### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the playbook file to Ansible control node.
- Update the configuration file to include the appropriate remote_user and then update the hosts file for the appropriate [Webservers] and [ELK] servers private IP addresses.
- Run the playbook, and navigate to the server (Webserver or ELK server, whichever you were running playbooks for) to check that the installation worked as expected.


- _Which file is the playbook? Where do you copy it? The playbook files are as follows: 
- Docker/Ansible installer: install_docker_dvwa.yml
- Filebeat: filebeat-playbook.yml
- Metricbeat: metricbeat-playbook.yml
- ELK setup: install_ELK.yml
- Copy these files to your Ansible control node, for my setup it is the contrainer lucid_kepler on my Jump Box
- _Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?
- Copy the hosts file to your ansible control node, then under the [Webservers] section enter the private IP addresses for the machines you want to filebeat and metricbeat. First you will need to run the install_docker_dvwa.yml.
-In the same hosts file, just below the [Webservers] section, there is a [ELk] section which you will put the private IP for the ELK machine.
 _Which URL do you navigate to in order to check that the ELK server is running?
 - To check if the ELK server is running, in a web browers navigate to (publicIP):5601/app/kibana. 

### Testing
- *** SSH Login Attempts 
-First I ran a continuing ssh login attempt using the command "watch -n 1 ssh azadmin@10.0.0.5" to verify Kibana was picking up the login attempts. I did this from the Jump Box but not inside the container. It picked up the attempts displaying the message "Connection closed by authenticating user azadmin 10.0.0.4 port 56200 [preauth]"  but the port number would change. Trying this on the other two Web VMs showed the same messages.
-*** CPU Stress Test
- Logged into each VM and did 'sudo apt install stress' then ran 'sudo stress --cpu-1 to stress test the VM. Screen shots in [...Lucid_Kepler/Ansible/Images]
[...Lucid_Kepler/Ansible/Images/CPU_Stress1.png][...Lucid_Kepler/Ansible/Images/CPU_Stress2.png][...Lucid_Kepler/Ansible/Images/CPU_Stress3.png] Each had their virtual CPUs maxed out but after stopping the stress they went back to normal operating levels.
-*** Wget
-Used the 'watch -n 1 wget 10.0.0.5' to test the network by running a wget request and generating an index.html file every 1 second(s). It affected both inbound and outbound traffic on the VM. Pairing it with cancatonating I was able to do it on several VMs at once 'wget 10.0.0.5 ; wget 10.0.0.6 ; wget 10.0.0.8'. This generated a lot of files, so to remove them all I used 'rm index.html ; rm index.html.*' in a single line on the terminal. Running 'wget -O /dev/null 10.0.0.5' allowed my to run wget without generating an index.html file with the option -O being the option for output and the /dev/null filepath to a null or void section that prevents saving any output. Example: With output 'wget 10.0.0.5 ; wget 10.0.0.6 ; wget 10.0.0.8' without output 'wget -O /dev/null 10.0.0.5 ; wget -O /dev/null 10.0.0.6 ; wget -O /dev/null 10.0.0.8'

-



_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._
- Download: git clone https://github.com/jcurzon2317/Lucid_Kepler
- Before running the playbooks, edit the ansible.cfg file. You need to change the remote_user on line 107 to your servers remote user name you previously assigned. Edit the hosts file for the appropriate private IP addresses/
- nano ansible.cfg
- nano hosts
- Run playbook: ansible-playbook ./(filepath)
	For my setup, I have everything saved to the file path /etc/ansible and within that directory I have a files folder and a roles folder. 
	The filebeat-config and metrioc-beat-config go in the files directory, so the playbook know where the source is to copy them to the destination computers.
	The roles folder has the playbooks in them for organizational purposes.
	When in the directory they are saved to, run:
	ansible-playbook ./install_docker_dvwa.yml
	anisble-playbook ./install_ELK.yml
	ansible-playbook ./filebeat-playbook.yml
	ansible-playbook ./metricbeat-playbook.yml
-

