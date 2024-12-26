# Configuration of Load balancer & webserver using Ansible Automation
![image](https://github.com/user-attachments/assets/f27fa663-6563-470f-bd12-b331e1f2d12a)

- Refer:
Manual way or using Ad hoc command Configuration of Load balancer & webserver using Ansible Automation in given link:
[Load-Balancer-manual](https://github.com/Pratikshinde55/Load-Balancer.git) 

## Using Ansible Automation (playbook):

For this Configuration i take four instances, Amazon linux EC2 instances.

One for  Ansible-Master node :   54.197.11.139

Two Instances for webserver (Backend): Public IP  backend 1- 54.159.221.239,Backend  2- 44.202.88.227

One for Load Balancer (Frontend):lb: 18.206.228.124

![Screenshot 2024-03-08 185237](https://github.com/Pratikshinde55/Ansible-loadBalancer-webserver-configuration/assets/145910708/3a8443c8-80f3-403d-bc7b-4d5b411f6f56)

In ansible master node created 2 playbooks **One for Loadbalancer** and **One for Webserver** & **One jinja template file** and one webpage.

![Screenshot 2024-03-08 191538](https://github.com/Pratikshinde55/Ansible-loadBalancer-webserver-configuration/assets/145910708/8762d476-644d-497e-b0e4-5b2c24c2b34f)

### Step-1: [In master node 1st create Webpage(index.php): This webpage index.php is deploy to webserver which can client access from browser]
     
     cat > index.php

![Screenshot 2024-03-08 191718](https://github.com/Pratikshinde55/Ansible-loadBalancer-webserver-configuration/assets/145910708/7e46d046-7a9b-4ad0-8a46-f329681c06b5)

### Step-2: [Create Inventory, In Inventory made two groups "Web" group for webserver nodes & "lb" group for LoadBalancer node]
     
     vim /etc/ansible/hosts

![Screenshot 2024-03-08 192439](https://github.com/Pratikshinde55/Ansible-loadBalancer-webserver-configuration/assets/145910708/c7222013-3942-48d7-abf2-80024384fa12)

We can check ansible list of hosts using following Command:

![Screenshot 2024-03-08 192031](https://github.com/Pratikshinde55/Ansible-loadBalancer-webserver-configuration/assets/145910708/c4e3522a-b1ff-432d-bf88-e30377fff6a0)

### Step-3: [Now Create Webserver(Backend) playbook and here hosts for webserver playbook is "web" group, in web group two instances are kept]

In this ansible-playbook:
1. Task install Httpd package,
2. Task is Install php package which is because my Webpage in the form of '.php',
3. Task is Deploy Webpage index.php to destination **"/var/www/html/"**,
4. Task for start httpd service.
     
       vim webserver.yml

![Screenshot 2024-03-08 191743](https://github.com/Pratikshinde55/Ansible-loadBalancer-webserver-configuration/assets/145910708/307945c7-0c30-4c0b-bcee-e35ed336d058)

### Step-4: [Run Webserver.yml playbook - this playbook do Configuration in Web group where Two nodes]
     
     ansible-playbook webserver.yml

![Screenshot 2024-03-08 192223](https://github.com/Pratikshinde55/Ansible-loadBalancer-webserver-configuration/assets/145910708/40949559-4c32-4cd7-abc9-a4f8e64168d7)

### Step-5: [Load Balancer need Registration of backend nodes]

Now for this, create jinja file "pratik.cfg.j2" which pass registration information to LoadBalancer playbook "lb":
      
      vim pratik.cfg.j2

![Screenshot 2024-03-08 191830](https://github.com/Pratikshinde55/Ansible-loadBalancer-webserver-configuration/assets/145910708/ea5a34da-3cc3-470d-95fb-8cc861e69ba1)

- Note:

*here we use **Round Robin** Algorithm that work as Client will connect to Web Server through Load balancer, 1st connect to Web 1 then Web 2 and next Web 3 then 
again go to 1. here also bind the port number as **8080**.*

### Step-6: [Create Load balancer Playbook]

Create Load balancer Playbook "lb.yml", add lb group where only one instance for lb IP:-18.206.228.124

In this playbook:-
1. task for install haproxy package It is loadBalancer that i used,
2. task for rigistration of backend webservre to Load balncer for this pratik.cfg.j2 jinja template used , In haproxy Load balancer configuration file for load 
balancer is kept in "/etc/haproxy/haproxy.cfg"
3. task for Start haproxy service

        vim lb.yml

![Screenshot 2024-03-08 191851](https://github.com/Pratikshinde55/Ansible-loadBalancer-webserver-configuration/assets/145910708/bd60bfcf-8a66-49ae-aa38-3478aa5398a4)

- Note:

**HAproxy is one of the product of load balancer, HAproxy is a high-performance, open source load balancer & reverse proxy for HTTP and TCP.**

### Step-7: [Run Load balancer]

Run Load balancer "lb.yml" playbook , This do load balncer configuration on lb group:

    ansible-playbook lb.yml

![Screenshot 2024-03-08 192329](https://github.com/Pratikshinde55/Ansible-loadBalancer-webserver-configuration/assets/145910708/006b2f23-342c-49b9-a1b2-adc0f95d6685)


## Configuration of Load balancer & WebServer done successfully:

Check on Google or Browser Publich IP load balancer : port no. that bind:

Example:- http://18.206.228.124:8080

![Screenshot 2024-03-08 193442](https://github.com/Pratikshinde55/Ansible-loadBalancer-webserver-configuration/assets/145910708/823b6f21-2453-4580-9369-ef92c9e1ed26)

- Note:

Load balancer node Inbound rules ->> All trafic , Anywhere ( remove Firewall )

![Screenshot 2024-03-08 185340](https://github.com/Pratikshinde55/Ansible-loadBalancer-webserver-configuration/assets/145910708/16ee3cf8-f783-4ea8-8dd4-f019dce74bb8)
