# Ansible with Cisco VIM

In this section, attendees will use Ansible to integrate with Cisco VIM

## Summary of steps:

* Login: **`ssh 10.201.241.229 -l {user-id}`**  
* Verify environment variables: **`env | grep -i OS`**  
* Change directory: **`cd /ciscolive`**   
* Verify existing flavor: **`nova flavor-list`**   
* Create network: **`ansible-playbook network.yml`**   
* Verify network: **`neutron net-list`**    
* Create subnet: **`ansible-playbook subnet.yml`**    
* Verify subnet: **`neutron subnet-list`**  
* Create Instance: **`ansible-playbook instance.yml`**  
* Verify Instance: **`nova list`**   

* Delete instance, subnet and network: **`ansible-playbook all.delete.yml`** 


---
# Detailed steps:


## Step 1: SSH to Build node

* From your PC initiate a SSH connection to Management/Build node. **Note:** The speakers will provide the credentials i.e. {user-id} and password for the SSH connection.

	**`ssh 10.201.241.229 -l {user-id}`** 

	Below is an example of "***user2***" initating successful ssh to Management node:

		FCHAUDHR-M-P0EL:config faisalc$ ssh 10.201.241.229 -l user2
		user2@10.201.241.229's password:
		Last login: Sun May 27 07:18:37 2018 from 10.60.164.55
		(venv)[user2@rcdn-nfvi-mgmt-03 ~]$

---
## Step 2: Verify virtual-environment

Once you successfully SSH into Management node, verify:

* You are in the virtual-environment named `venv` as shown below. **Note:** below shows an example for "user2":

		(venv)[user2@rcdn-nfvi-mgmt-03 ~]$

* Verify that environment variables related to your user by issuing below command:
	
	 **`env | grep -i OS`** 
	 
	Below output shows environment variables for user2.  Your respective user (1, 2, ... 8) will have appropriate OS\_USERNAME and OS\_TENANT\_NAME:

		(venv)[user2@rcdn-nfvi-mgmt-03 ~]$ env | grep -i OS
		HOSTNAME=rcdn-nfvi-mgmt-03
		OS_REGION_NAME=RegionOne
		OS_PASSWORD=D_JmKC6o_wU23OYf
		OS_AUTH_URL=http://10.201.241.227:5000/v2.0
		OS_USERNAME=user2
		OS_TENANT_NAME=tenant2
		(venv)[user2@rcdn-nfvi-mgmt-03 ~]$

*  Switch to correct directory by issuing below command:

 **`cd /ciscolive/working`**

	The output of above command is shown below: 

		(venv)[user2@rcdn-nfvi-mgmt-03 ~]$ cd /ciscolive/working/

**NOTE: Do not modify any of files.**  You may view the ansible playbooks or related files by using `less` or `more` command.  


---
## Step 3: Verify Flavor 

*  Verify that a flavor named tiny exists Cisco VIM by issuing below command:

 **`nova flavor-list`** 
 
  The output of above command is shown below:
  
		  (venv)[user2@rcdn-nfvi-mgmt-03 ciscolive]$ nova flavor-list
		+--------------------------------------+------+-----------+------+-----------+------+-------+-------------+-----------+
		| ID                                   | Name | Memory_MB | Disk | Ephemeral | Swap | VCPUs | RXTX_Factor | Is_Public |
		+--------------------------------------+------+-----------+------+-----------+------+-------+-------------+-----------+
		| a11f95b1-c7d0-43c5-9143-91272486af69 | tiny | 1024      | 10   | 10        |      | 1     | 1.0         | True      |
		+--------------------------------------+------+-----------+------+-----------+------+-------+-------------+-----------+
		(venv)[user2@rcdn-nfvi-mgmt-03 ciscolive]$

				
---
## Step 4: Run Ansible module to create network

* Run ansible playbook to create a neutron network in your respective project/tenant on CVIM by issuing:
  
 **`ansible-playbook network.yml`** 
 
 The output of above command is shown below:

~~~
(venv)[user2@rcdn-nfvi-mgmt-03 ciscolive]$ ansible-playbook network.yml

PLAY [Network creation on OpenStack] *******************************************

TASK [os_network] **************************************************************
changed: [localhost]

PLAY RECAP *********************************************************************
localhost                  : ok=1    changed=1    unreachable=0    failed=0

(venv)[user2@rcdn-nfvi-mgmt-03 ciscolive]$ neutron net-list
+--------------------------------------+---------------+-----------------------------------------+
| id                                   | name          | subnets                                 |
+--------------------------------------+---------------+-----------------------------------------+
| 6d8b571d-6c65-4818-b856-6377b2e87c93 | ext-net       | 62bb5327-6713-4f68-9ac5-082560df0b17    |
|                                      |               | 10.201.241.96/27                        |
| fa1b2aa4-abd3-40e2-a884-19c839ea2c15 | ext_net_user2 |                                         |
+--------------------------------------+---------------+-----------------------------------------+
(venv)[user2@rcdn-nfvi-mgmt-03 ciscolive]$ neutron subnet-list

~~~

*  Verify that the network is created by issuing below command   **`neutron net-list`** 


---
## Step 5: Run Ansible module to create Neutron subnet

* Run ansible playbook to create a neutron network in your respective project/tenant on CVIM by issuing below command:

 **`ansible-playbook subnet.yml`** 
 
  The output of above command is shown below:
  
~~~
(venv)[user2@rcdn-nfvi-mgmt-03 ciscolive]$ ansible-playbook subnet.yml

PLAY [local] *******************************************************************

TASK [setup] *******************************************************************
ok: [localhost]

TASK [set a fact1 for user1] ***************************************************
skipping: [localhost]

TASK [set a fact for user2] ****************************************************
ok: [localhost]

TASK [set a fact for user3] ****************************************************
skipping: [localhost]

TASK [set a fact for user4] ****************************************************
skipping: [localhost]

TASK [set a fact for user5] ****************************************************
skipping: [localhost]

TASK [set a fact for user6] ****************************************************
skipping: [localhost]

TASK [set a fact for user7] ****************************************************
skipping: [localhost]

TASK [set a fact for user8] ****************************************************
skipping: [localhost]

TASK [set a fact for user9] ****************************************************
skipping: [localhost]

TASK [set a fact for user10] ***************************************************
skipping: [localhost]

TASK [debug] *******************************************************************
ok: [localhost] => {
    "msg": "user value is user2, cidr_val 192.168.22.0/24, gateway_ip_val 192.168.22.1 "
}

PLAY [Subnet creation on OpenStack] ********************************************

TASK [os_subnet] ***************************************************************
changed: [localhost]

PLAY RECAP *********************************************************************
localhost                  : ok=4    changed=1    unreachable=0    failed=0

(venv)[user2@rcdn-nfvi-mgmt-03 ciscolive]$

~~~


*  Verify that the network is created by issuing below command:

 **`neutron subnet-list`** 


---
## Step 6: Run Ansible module to create an Instance (VM)

* Run ansible playbook to create an instance (i.e. a Virtual Machine/VM) in your respective project/tenant on CVIM by issuing below command:

 **`ansible-playbook instance.yml`** 

 The output of above command is shown below:

~~~
(venv)[user2@rcdn-nfvi-mgmt-03 ciscolive]$ ansible-playbook instance.yml

PLAY [Instance creation on OpenStack] ******************************************


TASK [Deploy an instance] ******************************************************
changed: [localhost]

PLAY RECAP *********************************************************************
localhost                  : ok=1    changed=1    unreachable=0    failed=0

(venv)[user2@rcdn-nfvi-mgmt-03 ciscolive]$

~~~


*  Verify that instance is created by issuing below command:

 **`nova list`** 

 The output of above command is shown below:


		(venv)[user2@rcdn-nfvi-mgmt-03 working]$ nova list
		+--------------------------------------+-----------------+--------+------------+-------------+----------------------------+
		| ID                                   | Name            | Status | Task State | Power State | Networks                   |
		+--------------------------------------+-----------------+--------+------------+-------------+----------------------------+
		| d29a64f0-6ac1-4aca-80f1-5fa44bcfa668 | webserver_user2 | ACTIVE | -          | Running     | ext_net_user2=192.168.30.9 |
		+--------------------------------------+-----------------+--------+------------+-------------+----------------------------+
		(venv)[user2@rcdn-nfvi-mgmt-03 working]$


---
## Step 7:  Delete in one Ansible playbook:
* Run ansible playbook to delete instance, subnet and network on CVIM by issuing below command:

 **`ansible-playbook all.delete.yml`** 

 The output of above command is shown below:

~~~
(venv)[user2@rcdn-nfvi-mgmt-03 ciscolive]$ ansible-playbook all.delete.yml

PLAY [Deploy on OpenStack] *****************************************************

TASK [Instance deletion] *******************************************************
changed: [localhost]

TASK [subnet deletion] *********************************************************
changed: [localhost]

TASK [Network deletion] ********************************************************
changed: [localhost]

PLAY RECAP *********************************************************************
localhost                  : ok=3    changed=3    unreachable=0    failed=0

(venv)[user2@rcdn-nfvi-mgmt-03 ciscolive]$ nova flavor-list

~~~

---
Congrats: you have successfully finished running Ansible modules with Cisco VIM (CVIM)!


