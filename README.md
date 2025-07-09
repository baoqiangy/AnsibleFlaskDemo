# AnsibleFlaskDemo

**Step1:**
Update packages on your base CentOS VM, then use the updated VM as the base machine for the following link clones.

**Step2:**
Link clone 4 Copies of the updated Cent OS VM with some static IPs, all on the default NAT network, and assign the following hostnames to them. If you choose to use different hostnames. You need to figure out by yourself which files to change.
- controller
- server1
- server2
- server3

Power on all VMs, add the IP-hostname mappings of all these 4 VMs to their /etc/hosts files.

**Step3:**
On the controller, 

 1. Make sure you have Ansible installed.
 2. Install the following collections and the **geerlingguy.mysql** role from ansible galaxy, if they are not installed yet.
	- ansible.posix
	- community.general
	- community.mysql
 3. Clone this AnsibleFlaskDemo repo.
 4. Set up the two key-pairs as demonstrated in the PPT slides and the lecture videos (**one named ansible without passphrase, one for your user account with a passphrase**), push the public-key of your **current user account** (not the ansible public key) to all the server VMs.
 5. Follow the steps on slide 33 to provision a sudoer user **student** that can login with the ansible private key that is NOT protected by a passphrase.  

**Step4:**
On the 'controller', 

 1. Cd into the AnsibleFlaskDemo directory.
 2. Run the following commands to provision an Ansible agent user account on all the servers. 
 	
    *$ansible-playbook -u student --key-file ~/.ssh/ansible --extra-vars="host=all" playbooks/prepare.yml -K*

    Make sure there is no previous mysql database server running on the the servers by running the following command. 
    (Use your own paths to the inventory and yml files. Change the inventory, if needed, to include server2 and server3 in the database server group.)
    *$ansible-playbook -i ~/Documents/AnsibleDemo/inventory_servergroup ~/Document/AnsibleDemo/uninstall_web_db.yml -K*
    
    Take a snapshot on all VMs now. You can repeat the following steps by first restoring these snapshots on the servers then running these commands again on the controller.

4. Then execute the **deploy_noncontainerized_flask.yml** playbook using the following command.

     *$ansible-playbook playbooks/deploy_noncontainerized_flask.yml*

	Once it is completed, set up the port forwarding so you can try the flask app on your host machine. Register an user and login.
	(**If the forwarding is not working, just try it on server1 directly. **)

5. Then execute the **deploy_containerized_flask.yml** playbook using the following command.

     *$ansible-playbook playbooks/deploy_containerized_flask.yml*
	
	Once it is completed, set up the port forwarding so you try the flask app on your host machine. Register an user and login.
	(**If the forwarding is not working, just try it on server2 and server3 directly. **)

**Note:**
If you have problems executing step 5, try restoring the snapshot you created in step 4.2 then repeat step 4.3 and 4.4.
