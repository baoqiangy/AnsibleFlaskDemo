# AnsibleFlaskDemo

**Step1:**
Update packages on your base CentOS VM, then use the updated VM as the base machine for the following link clones.

**Step2:**
Link clone 2 Copies of the updated Cent OS VM with some static IPs and the following hostnames. If you choose to use different hostnames. You need to figure out by yourself which files to change.
- flask-nocontainer
- flask-container

If you already have the Ansible controller set up, keep using it. Otherwise, you need to link clone another copy of the Cent OS VM and give it a static IP and a hostname **'controller'**.

Add the IP-hostname mappings of the two new VMs to the controller's /etc/hosts.

Make sure these 3 VMs are in the same network (NatNetwork preferred).
 
 **Step3:**
On the controller, 

 1. Make sure you have Ansible installed.
 2. Install the following collections and the **geerlingguy.mysql** role from ansible galaxy, if you haven't done so.
	- ansible.posix
	- community.general
	- community.mysql
 3. Clone this AnsibleFlaskDemo repo.
 4. Set up the two key-pairs as demonstrated in class (**one named ansible without passphrase, one for your user account with a passphrase**), push the public-key of your **current user account** (not the ansible public key) to **'flask-container'** and **'flask-nocontainer'**.

 **Step4:**
On the 'controller', 

 1. Cd into the AnsibleFlaskDemo directory.
 2. Run the following commands to provision an Ansible agent user account on **'flask-container'** and **'flask-nocontainer'**. 
 (**Replace byan with your own username, and replace id_ed25519 with your own private key file.**)
	

    *$ansible-playbook -u student --key-file ~/.ssh/ansible --extra-vars="host=all" playbooks/prepare.yml -K*	

   (**If you get errors when executing the tasks in prepare.yml, remove the base role from prepare.yml, then try again. The reason why the errors occurr might be because the daemons are already running.**)

3. Then execute the **deploy_noncontainerized_flask.yml** playbook using the following command.

     *$ansible-playbook playbooks/deploy_noncontainerized_flask.yml*

	Once it is completed, set up the port forwarding so you try the flask app on your host machine. Register an user and login.
	(**If the forwarding is not working, just try it on the flask-nocontainer directly. **)

4. Then execute the **deploy_containerized_flask.yml** playbook using the following command.

     *$ansible-playbook playbooks/deploy_containerized_flask.yml*
	
	Once it is completed, set up the port forwarding so you try the flask app on your host machine. Register an user and login.
	(**If the forwarding is not working, just try it on the flask-container directly. **)

