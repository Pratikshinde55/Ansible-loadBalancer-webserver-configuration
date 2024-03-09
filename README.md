# Ansible-loadBalancer-webserver-configuration

🌟 Configuration of Load balancer & webserver using Ansible Automation 🌟

Manual way or using Ad hoc command Configuration of Load balancer & webserver using Ansible Automation in given link:
(https://github.com/Pratikshinde55/Load-Balancer.git)

❄️ By using Ansible Automation (playbook):

For this Configuration i take four instances, Amazon linux EC2 instances.

one for  Ansible-Master node :   54.197.11.139

two instances for webserver (Backend): Public IP  backend 1- 54.159.221.239,Backend  2- 44.202.88.227

one for Load Balancer (Frontend):lb: 18.206.228.124

![Screenshot 2024-03-08 185237](https://github.com/Pratikshinde55/Ansible-loadBalancer-webserver-configuration/assets/145910708/3a8443c8-80f3-403d-bc7b-4d5b411f6f56)

In ansible master node i created 2 playbooks one for Loadbalancer and one for webserver & one jinja template file and one webpage.

![Screenshot 2024-03-08 191538](https://github.com/Pratikshinde55/Ansible-loadBalancer-webserver-configuration/assets/145910708/8762d476-644d-497e-b0e4-5b2c24c2b34f)

Step-1:

In  master node 1st i created webpage (index.php): This webpage index.php is deploy to webserver which can client access from browser:

![Screenshot 2024-03-08 191718](https://github.com/Pratikshinde55/Ansible-loadBalancer-webserver-configuration/assets/145910708/7e46d046-7a9b-4ad0-8a46-f329681c06b5)

Step-2:

Create Inventory , In inventory i made two groups "Web" group for webserver nodes & "lb" group for LoadBalancer node:

![Screenshot 2024-03-08 192439](https://github.com/Pratikshinde55/Ansible-loadBalancer-webserver-configuration/assets/145910708/c7222013-3942-48d7-abf2-80024384fa12)






