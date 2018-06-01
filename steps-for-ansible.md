# Ansible with Cisco VIM

In this section, attendees will use Ansible to integrate with Cisco VIM

## Step 1: SSH to Build node

* From your PC initiate a SSH connection to Management/Build node. **Note:** The speakers will provide the credentials (username and password) for the SSH connection.


		ssh 10.201.241.229 -l {user-id} 

Below is an example of "***user1***" initating successful ssh to Management node:

	FCHAUDHR-M-P0EL:config faisalc$ ssh 10.201.241.229 -l user1
	user1@10.201.241.229's password:
	Last login: Sun May 27 07:18:37 2018 from 10.60.164.55
	(venv)[user1@rcdn-nfvi-mgmt-03 ~]$

## Step 2: Verify virtual-environment

Once you successfully SSH into Management node, verify:

* You are in the virtual-environment named `venv` as shown below. **Note:** below shows an example for "user1":

		(venv)[user1@rcdn-nfvi-mgmt-03 ~]$

* Verify that environment variables related to your user by issuing `env | grep -i OS` command.  Below example shows environment variables for user1, your respective user (1, 2, ... 8) will have appropriate OS_USERNAME and OS_TENANT_NAME:

		(venv)[user1@rcdn-nfvi-mgmt-03 ~]$ env | grep -i OS
		HOSTNAME=rcdn-nfvi-mgmt-03
		OS_REGION_NAME=RegionOne
		OS_PASSWORD=D_JmKC6o_wU23OYf
		OS_AUTH_URL=http://10.201.241.227:5000/v2.0
		OS_USERNAME=user1
		OS_TENANT_NAME=tenant1
		(venv)[user1@rcdn-nfvi-mgmt-03 ~]$

*  Switch to correct directory by issuing `cd /ciscolive/working` command as shown below:

		(venv)[user1@rcdn-nfvi-mgmt-03 ~]$ cd /ciscolive/working/

You may view the ansible playbooks (.yml files) by using `less` or `more` command.  

**NOTE: Do not modify any of YAML files** 


## Step 3: Run Ansible module to create Flavor 

*  Verify that no flavor for your respective tenant/project exists on Cisco VIM by issuing below command:
		
		(venv)[user2@rcdn-nfvi-mgmt-03 working]$ nova flavor-list
		+--------------------------------------+------------+-----------+------+-----------+------+-------+-------------+-----------+
		| ID                                   | Name       | Memory_MB | Disk | Ephemeral | Swap | VCPUs | RXTX_Factor | Is_Public |
		+--------------------------------------+------------+-----------+------+-----------+------+-------+-------------+-----------+
			|  |  |      |    |         |      |      |          |       |
		+--------------------------------------+------------+-----------+------+-----------+------+-------+-------------+-----------+
		(venv)[user2@rcdn-nfvi-mgmt-03 working]$ 

* Run your first ansible playbook to create a nova flavor in your respective project/tenant on CVIM by issuing `ansible-playbook 7.flavor.yml` command as shown below:

		(venv)[user2@rcdn-nfvi-mgmt-03 working]$ ansible-playbook 7.flavor.yml
		
		PLAY [Deploy on OpenStack] *****************************************************
		
		TASK [Create tiny flavor] ******************************************************
		changed: [localhost]
		
		PLAY RECAP *********************************************************************
		localhost                  : ok=1    changed=1    unreachable=0    failed=0
		
		(venv)[user2@rcdn-nfvi-mgmt-03 working]$
		

* Verify that flavor is created by issuing `nova flavor-list` command:

		(venv)[user2@rcdn-nfvi-mgmt-03 working]$ nova flavor-list
		+--------------------------------------+------------+-----------+------+-----------+------+-------+-------------+-----------+
		| ID                                   | Name       | Memory_MB | Disk | Ephemeral | Swap | VCPUs | RXTX_Factor | Is_Public |
		+--------------------------------------+------------+-----------+------+-----------+------+-------+-------------+-----------+
		| 4f2d2b16-46a7-42dd-87c1-97e70ad0394c | tiny-user2 | 1024      | 10   | 10        |      | 1     | 1.0         | True      |
		+--------------------------------------+------------+-----------+------+-----------+------+-------+-------------+-----------+
		(venv)[user2@rcdn-nfvi-mgmt-03 working]$
		

## Step 4: Run Ansible module to create network



* Run ansible playbook to create a neutron network in your respective project/tenant on CVIM by issuing `ansible-playbook 7.network.yml` command as shown below:

		(venv)[user2@rcdn-nfvi-mgmt-03 working]$ ansible-playbook 7.network.yml
		
		PLAY [Network creation on OpenStack] *******************************************
		
		TASK [os_network] **************************************************************
		changed: [localhost]
		
		PLAY RECAP *********************************************************************
		localhost                  : ok=1    changed=1    unreachable=0    failed=0
		
		(venv)[user2@rcdn-nfvi-mgmt-03 working]$ 

*  Verify that the network is created by issuing `neutron net-list` command

## Step 5: Run Ansible module to create Neutron subnet

* Run ansible playbook to create a neutron network in your respective project/tenant on CVIM by issuing `ansible-playbook 7.subnet.yml` command as shown below: 

		(venv)[user2@rcdn-nfvi-mgmt-03 working]$ ansible-playbook 7.subnet.yml
		
		PLAY [local] *******************************************************************
		
		TASK [setup] *******************************************************************
		ok: [localhost]
		
		TASK [set a fact1] *************************************************************
		ok: [localhost]
		
		TASK [set a fact for user2] ****************************************************
		ok: [localhost]
		
		TASK [set a fact for user3] ****************************************************
		ok: [localhost]
		
		TASK [set a fact for user4] ****************************************************
		ok: [localhost]
		
		TASK [set a fact for user5] ****************************************************
		ok: [localhost]
		
		TASK [set a fact for user6] ****************************************************
		ok: [localhost]
		
		TASK [set a fact for user7] ****************************************************
		ok: [localhost]
		
		TASK [set a fact for user8] ****************************************************
		ok: [localhost]
		
		TASK [set a fact for user9] ****************************************************
		ok: [localhost]
		
		TASK [set a fact for user10] ***************************************************
		ok: [localhost]
		
		PLAY [Subnet creation on OpenStack] ********************************************
		
		TASK [os_subnet] ***************************************************************
		changed: [localhost]
		
		PLAY RECAP *********************************************************************
		localhost                  : ok=12   changed=1    unreachable=0    failed=0
		
		(venv)[user2@rcdn-nfvi-mgmt-03 working]$ 


*  Verify that the network is created by issuing `neutron subnet-list` command:


## Step 5: Run Ansible module to create an Instance (VM)

* Run ansible playbook to create an instance (i.e. a Virtual Machine/VM) in your respective project/tenant on CVIM by issuing `ansible-playbook 7.instance.yml` command as shown below: 

		(venv)[user2@rcdn-nfvi-mgmt-03 working]$ ansible-playbook 7.instance.yml
		
		PLAY [Instance creation on OpenStack] ******************************************
		
		TASK [Deploy an instance] ******************************************************
		changed: [localhost]
		
		PLAY RECAP *********************************************************************
		localhost                  : ok=1    changed=1    unreachable=0    failed=0
		
		(venv)[user2@rcdn-nfvi-mgmt-03 working]$ 


*  Verify that instance is created by issuing `nova list` command as shown below:

		(venv)[user2@rcdn-nfvi-mgmt-03 working]$ nova list
		+--------------------------------------+-----------------+--------+------------+-------------+----------------------------+
		| ID                                   | Name            | Status | Task State | Power State | Networks                   |
		+--------------------------------------+-----------------+--------+------------+-------------+----------------------------+
		| d29a64f0-6ac1-4aca-80f1-5fa44bcfa668 | webserver_user2 | ACTIVE | -          | Running     | ext_net_user2=192.168.30.9 |
		+--------------------------------------+-----------------+--------+------------+-------------+----------------------------+
		(venv)[user2@rcdn-nfvi-mgmt-03 working]$


---

Congrats: you have successfully finished running Ansible modules with Cisco VIM (CVIM)!


