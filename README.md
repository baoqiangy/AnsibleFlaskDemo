# CSC324_Exam3

**Step1:**
Update packages on your base CentOS 8 VM, then use the updated VM as the base machine for the following link clones.

**Step2:**
Link clone 2 Copies of the updated Cent OS 8 VM with some static IPs and the following hostnames. If you choose to use different hostnames. You need to figure out by yourself which files to change.
- flask-nocontainer
- flask-container

If you already have the Ansible controller set up, keep using it. Otherwise, you need to link clone another copy of the Cent OS 8 VM and give it a static IP and a hostname **'controller'**.

Add the IP-hostname mappings of the two new VMs to the controller's /etc/hosts.

Make sure these 3 VMs are in the same network (NatNetwork preferred).
 
 **Step3:**
On the controller, 

 1. Make sure you have Ansible installed.
 2. Install the collections as mentioned in previous assignments and the **geerlingguy.mysql** role from ansible galaxy.
 3. Clone this CSC324_Exam3 repo.
 4. Set up the two key-pairs as mentioned in previous assignments, push the public-key of your current user account to **'flask-container'** and **'flask-nocontainer'**.

 **Step4:**
On the 'controller', 

 1. Cd into the CSC324_Exam3 directory.
 2. Run the following commands to provision an Ansible agent user account on **'flask-container'** and **'flask-nocontainer'**. 
 (**Replace byan with your own username, and replace id_ed25519 with your own private key file**)
	

    *$ansible-playbook -u byan --key-file ~/.ssh/id_ed25519 --extra-vars="host=flask-container" playbooks/prepare.yml -K*
	

    *$ansible-playbook -u byan --key-file ~/.ssh/id_ed25519 --extra-vars="host=flask-nocontainer" playbooks/prepare.yml -K*

 3. Then execute the **deploy_noncontainerized_flask.yml** playbook using the following command.

     *$ansible-playbook playbooks/deploy_noncontainerized_flask.yml*

	Once it is completed, set up the port forwarding so you try the flask app on your host machine. Register an user and login.

4. Then execute the **deploy_containerized_flask.yml** playbook using the following command.

     *$ansible-playbook playbooks/deploy_containerized_flask.yml*
	
	Once it is completed, set up the port forwarding so you try the flask app on your host machine. Register an user and login.

Students might get complaints about "DNF" when executing the docker role. I never had this issue, but I believe the reason why this error pops up is because DNF is the package manager in the output of the gather_facts, as shown below. Then why didn't it complain to me? I don't know the reason yet. I repeated the lab with a fresh copy of the base machine that I never did package update on and it still worked for me. 

> $ansible -m setup flask-container -a 'filter=ansible_pkg_mgr'
> flask-container | SUCCESS => {
>     "ansible_facts": {
>         "ansible_pkg_mgr": "dnf",
>         "discovered_interpreter_python": "/usr/libexec/platform-python"
>     },
>     "changed": false }
