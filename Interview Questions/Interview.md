## Activity File: Interview Questions

- This first project covers a wide range of topics including cloud, network security, and logging and monitoring.

- When networking and talking to potential employers, you should be able to reference the work done on this project to answer specific interview questions or demonstrate your skills within a specific domain. 

- You will choose a domain that you're interested in pursuing as a career and answer mock questions based on the suggested response format. 
​
### Instructions 

1. Choose one of the following domains:
    - Network security
    - Cloud security
    - Logging and monitoring

    If you are unsure of which domain you want to focus on, that's okay. You can either choose the one you're most comfortable discussing, or complete the tasks in two or all three domains.

2. Select one domain and one question. 

    - Questions are provided for each domain. Choose one to answer from your chosen domain. 
​
3. Write a one-page response that answers the question using specific examples from your work on Project 1. Your response should flow and read like a presentation while keeping the general structure of the technical question response guidelines. 

    You will submit this one-page response. 

#### Reminder: Response Guidelines
As a reminder,  good responses do the following. 
​
1. Restate the problem.
2. Provide a concrete example scenario.
3. Explain the solution requirements.
4. Explain the solution details.
5. Identify advantages and disadvantages of the solution​.
​
Including each of these components will ensure you prove your competency of subject matter and critical thinking. 
​
### Interview Questions by Domain

Below you will find a list of questions, grouped by specific domains. Select one question to answer. 
​ 

For each question, where appropriate, we have provided you with specific prompts to consider as you structure each section of your response. Feel free to use these prompts or your own examples. 

#### Domain: Network Security

**Question 1:  Faulty Firewall**

Suppose you have a firewall that's supposed to block SSH connections, but instead lets them through. How would you debug it?

Make sure each section of your response answers the questions laid out below.
​
1. Restate the Problem

The problem is, from what I understand, is that you want the firewall to block SSH connections, but it is letting them through. How would I debug it?

2. Provide a Concrete Example Scenario
    - In Project 1, did you allow SSH traffic to all of the VMs on your network?
    - Which VMs did accept SSH connections?
    - What happens if you try to connect to a VM that does not accept SSH connections? Why?
    
    To start, we should create a firewall rule to deny all traffic on all ports. Then, slowly we will open up protocols and ports as necessary. If we need to SSH into the network, we can change the firewall rules to allow SSH traffic on port 22 for a single host machine which could use an asymetric key. Once we are done, we should immediately delete the rule so it will default back to denying all SSH traffic and close port 22. Keep all ports closed that won't be used. If temporarily suspending traffic would be too much interuption, we could set a low priority deny all or a default deny all traffic rule and then monitor traffic over the network specificially for what could be SSH while we attempt to SSH in ourselves.
    
  In my project, I started by making a deny all traffic rule with a low priority. I then slowly ipened up traffic for what I needed, such as port 80 to the load balacner and ports 5601 from my host machine to the ELK VM and port 9200 between my DVWA VMs to the ELK VM. I limited SSH traffic only to the jumpbox. I allowed traffic only from my personal host machine, using an asymetric key, they private key being on my personal computer and the network security groups having the public key for the Jumpbox. The Jumpbox is part of the virtual network and can SSH to the otehr VMs. To do this, I have an ansible container on it with its own, seperate Asymetric key for the four other virtual machines, each of which have the matching public keys. The network security group only allows traffic over port 22 from internal private IP addresses, specifically the Jumpbox IP address. No others. If any other computer or virtual machines try to establish an SSH connection, it will time out because the port is being filtered. For even greater security, I can edit the Network security rules to block all SSH traffic with a higher priority rule, which can be removed as needed for maintentance.

3. Explain the Solution Requirements
    - If one of your Project 1 VMs accepted SSH connections, what would you assume the source of the error is?
    - Which general configurations would you double-check?
    - What actions would you take to test that your new configurations are effective?

In my project, all the VMs allow SSH connections from the JumpBox and the JumpBox allows SSH connections from my personal host computer. I would therefore assume the error is from the JumpBox. I would check the NSG rules to see what rules are in place to ensure that SSH traffic is denied and if there are any ports open that don't need to be. After any and all changes I would try to establish SSH connections for the various computers/VMs. 

4. Explain the Solution Details
    - Which specific panes in the Azure UI would you look at to investigate the problem?
    - Which specific configurations and controls would you check?
    - What would you look for, specifically?
    - How would you attempt to connect to your VMs to test that your fix is effective?

5. Identify Advantages/Disadvantages of the Solution

The advantage of this solution is obvious, it's relatively simple and cheap. All we are doing is denying all traffic and slowly opening up rules to allow access for our needs.

    - Does your solution guarantee that the Project 1 network is now "immune" to all unauthorized access?
    
It doesn't make it immune to all unauthorized access, it just makes it harder to gain access. Whether we limit SSH traffic, close ports that we don't need open, or limit SSH traffic to a single machine, we still run a few risks. The host machine that is allowed to SSH in could become compromised, allowing an attacker to use the private key on the machine.The Azure account, if compromised,could chang the network security group rules and open up SSH traffic and the necessary ports, or even changing the virtual machines access to allow for a set password or their own pubilc key so they can SSH in from their own computer.

    - What monitoring controls might you add to ensure that you identify any suspicious authentication attempts?​
    
    Monitoring should be done on all SSH traffic and any ports that are open to determine if any intrusions have been made. We should also monitor event logs on the Azure account to see if anyone else has gained access to the account and potentially made changes to the NSG rules or the public keys attached to the VMs. Setting up security alerts for both Azure account as well as network traffic could alert us to any unauthorized access.

**Question 2: Unsecured Web Server**

Suppose you find a server running HTTP on port 80, despite compliance guidelines requiring encryption in motion. What do you do?
​​
1. Restate the Problem
A server is running HTTP on port 80, despite compliance guidelines requireing encryption in motion and what I would do to remedy that?

2. Provide a Concrete Example Scenario
    - In my project that I have posted on github, I have my loadbalancer running HTTP on port 80 and it port forwards to the webservers running DVWA. The reason being is they aren't critical and are meant to be used for penetration testing purposes.
    
    - In a real deployment, I would be running the more secure HTTPS on port 443, which will allow encryption in motion.

3. Explain the Solution Requirements
    - Running on port 80 using HTTP isn't as secure because the data isn't encrypted.
    - How would you reconfigure a server to serve HTTP traffic safely?
    - How does this solution fix the problem?

4. Explain the Solution Details
    - Which tools and technologies would you use to implement this solution in Project 1?
    -  How, specifically, would you use these tools to harden your deployment?

5. Identify Advantages and Disadvantages of the Solution
    -  Will your solution break clients that used to communicate with the server over port `80`?
    -  Do you have to do any work to keep this solution running longterm? Or can you simply "set it and forget it?”

© 2020 Trilogy Education Services, a 2U, Inc. brand. All Rights Reserved.  
