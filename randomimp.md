1.  Feature	        Image       	    Snapshot	        Template
Purpose           	Create new servers	Backup/restore	    Reusable VM blueprint
Contains	        OS + software	    Disk data/state	      VM configuration
Used for	        Deployment	          Recovery	         Standardized provisioning
Example          	AMI	                    EBS Snapshot	  VMware Template

template:
OS
VM settings
CPU/RAM configs
Installed software

2. | Feature      | System User     | Normal User    |
| ------------ | --------------- | -------------- |
| Purpose      | Run services    | Human login    |
| UID          | Usually < 1000  | Usually ≥ 1000 |
| Login access | Mostly disabled | Enabled        |
| Example      | mysql, nginx    | tanuja         |

 | User       | Purpose                 |
| ---------- | ----------------------- |
| `root`     | Super administrator     |
| `daemon`   | Background services     |
| `nobody`   | Limited permission user |
| `www-data` | Apache/Nginx web server |
| `mysql`    | MySQL database          |
| `sshd`     | SSH service             |

3. | Instance Type         | Purpose                       |
| --------------------- | ----------------------------- |
| General Purpose       | Balanced CPU, RAM, networking |    t2.micro ,instance family,generation, size
                                                             t3.medium
                                                             m5.large
| Compute Optimized     | High CPU performance          |    c5.large
                                                             c6i.xlarge
| Memory Optimized      | High RAM workloads            |    r5.large
                                                             x1e.xlarge
| Storage Optimized     | High disk throughput          |    i3.large
                                                            d2.xlarge
| Accelerated Computing | GPU/ML workloads              |   p3.large
                                                            g4dn.xlarge
An EC2 instance type defines the hardware capacity of a virtual server, including CPU, memory, storage, and network performance. Different instance families are optimized for different workloads.”

4. | Feature      | Node.js             | Python               | Java               |
| ------------ | ------------------- | -------------------- | ------------------ |
| Best For     | Real-time apps      | Automation & AI      | Enterprise systems |
| Performance  | High I/O            | Medium               | Very high          |
| Thread Model | Event loop          | Multi-thread/process | Multi-thread       |
| Startup Time | Fast                | Fast                 | Slower             |
| Memory Usage | Low                 | Medium               | High               |
| Scalability  | Good                | Medium               | Excellent          |
| DevOps Usage | APIs, microservices | Automation           | Enterprise CI/CD   |


Difference Between Python Script and Shell Script
Feature                    	Python Script	                    Shell Script
Purpose	                    General programming & automation	Linux command automation
File Extension	              .py	                                 .sh
Interpreter	                  Python interpreter	        Shell interpreter (bash/sh)
Speed	                      Slightly slower	                 Faster for OS commands
Complexity Handling	           Better for complex logic	          Better for simple automation
Platform	                    Cross-platform	                Mostly Linux/Unix
Syntax	                         Programming language syntax	Linux shell commands
Error Handling	                   Advanced	               Limited
Libraries	                        Huge library support	        Limited
DevOps Usage	                      APIs, cloud automation	Server tasks, cron jobs