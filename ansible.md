playbook excutions startgies:
default (linear):
a. host group has 5 servers and play has tasks 2
task1 -----servr1,serevr2,server3,server4,server5(picked up all the machines run tasks all the task parllel once compleated moved to other task )
task2 ----- servr1,serevr2,server3,server4,server5 

what if something is corrupted or went wrong while running tasks so all our instances corrupted.
so we (parllel or fork )startegy:
a. host group has 5 servers
b. play has 2 tasks
c. forks = 3
task 1 : serever1,server2,server3 run tasks parllel once its is completed move to next remaining servers.
task 2 : same process 

Use forks to set the maximum parallelism and serial to control how many hosts to work on at a time within a play.so it will not take all machines.(-f 3)

serial (batch method):
picking all machines and running parllely your tasks instead of that use serial method and pickedup instances and and run task
a. host group has 5 servers
b. play has 2 tasks
c. serial = 3

task1 : server1,server2,server3
task2: server1,server2,server3

task1: server4,server5
task2: server4,server5.
serial: 3 in playbook give 

serial and fork method also u can use at a time i.e serial pickup host and fork for run tasks parllely at a time.

Parameter	   Scope	n              Controls	                            Example Usage
serial	       In playbooks	       How many hosts to process at once	serial: 3
forks	        In config	       Max parallel tasks (global)	        forks = 10

------------------------------------------

serial: This means Ansible will run the play on 2 hosts at a time. When those are done, it will move to the next 2.

Great for rolling deployments, zero-downtime updates, or when you don't want to hit all servers at once (e.g., restarting services, applying updates).

 Fork: This tells Ansible to run up to 10 tasks in parallel across your inventory. It affects overall speed but doesn't control batch sizes like serial. fork = 10.


** gathering requirements, desiging it, based on design how do u put the architecture, how do u excute them.



22. seprate user accuounts for server environment is best practice.{dev,prod,pre pod,qa etc}
instead of pswrd u can use password less option and login with username and publickey.
if i want connect to dev serevers i need to maintain inventory of all servers and eun the tasks.

ansible-playbook sanmple.yaml -i inventory file name --become-user dev-user(specific user) --check
o/p: ansible connect to all machines and perform tasks.

[dev]
dev.example.com ansible_user=devuser ansible_ssh_private_key_file=/home/youruser/.ssh/dev_key.pem
Replace the hostnames (e.g., dev.example.com) and paths to private keys with your actual server and key file paths.
like this for all environments.
** maintain ansible controller different environments.for better approach to run.

23. u can access bucket golabaly.
3. give unique name to bucket
4. u can store any type of orginazation data  
5. create bucket based on region so latency issue resolved.
6. by default encrypt data or objects.
7. enable/ disable public access
8. bucket versioning also avaialble.
** upload file in s3. we called it as objects.
server acess loging, event notifications.
 
** eventbridge,object lock options are available.

*** permssion restrict to i am user to acess the s3 and host static website through s3 bucket object.

----------------------------


24. create inventory file through teraform using local file resource and templatefile i.e inside template folder we have host values and file ends with tpl


25. . remote-src: yes
Context: Typically used with the Ansible copy module.

Meaning:
If remote-src is set to yes, the file you are copying is assumed to be already present on the remote machine.

26. enabled: yes
Context: Used in various modules like service, systemd, or resource definitions.

Meaning:

Indicates that a service or configuration should be enabled at boot/startup.
For example, in the systemd or service module, it ensures the service starts automatically on reboot.

27. In Ansible, you can use tags to control which parts of a playbook are executed. When running your playbook from the command line, you can use the --tags or --skip-tags options to include or exclude specific tagged tasks or plays.

 Run only installation tagged tasks: ansible-playbook playbook.yml --tags "installation"
ansible-playbook playbook.yml --tags "deployment"

✅ Best Practices
Use tags to break large playbooks into manageable, repeatable sections.
Common tag names: install, configure, deploy, cleanup, restart, etc.


28. 🧵 Parallelism in Ansible
In Ansible, parallelism refers to how many hosts are managed at the same time during playbook execution. This is controlled using the forks setting.

[defaults]
forks = 10

This sets Ansible to run tasks on 10 hosts in parallel.

Default value of forks is 5.

This doesn’t affect parallelism within a host (e.g., multi-threaded tasks). Ansible is still sequential per host.

