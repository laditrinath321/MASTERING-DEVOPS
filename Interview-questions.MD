#### 1.Q: Have you ever come across a situation where a process in Linux is automatically killed?

Yes, this typically happens when the system runs out of memory. The OOM Killer (Out Of Memory Killer) in Linux terminates processes to free up RAM. You can check this using:
dmesg | grep -i "killed process"

#### 2.Q:  Write code for CPU and RAM utilization using Python?
import psutil
#### CPU usage
cpu_percent = psutil.cpu_percent(interval=#1)
print(f"CPU Usage: {cpu_percent}%")
#### RAM usage
memory = psutil.virtual_memory()
print(f"RAM Usage: {memory.percent}%")

#### 3.Q: How do you read and write key-value pairs from a text.json file in Python?
import json
#### Read JSON file
with open('text.json', 'r') as f:
    data = json.load(f)
#### Access key-value pairs
for key, value in data.items():
    print(f"{key}: {value}")
#### Modify or add a new key-value pair
data['new_key'] = 'new_value'
#### Write back to the file
with open('text.json', 'w') as f:
    json.dump(data, f, indent=# 4)
    
#### 4.Q: How can you create user groups in Linux?
#### Create a group
sudo groupadd devteam
#### Add user to group
sudo usermod -aG devteam username
####Verify
groups username

#### 5.Q: How do you create an RDS instance in a specific VPC using Terraform? 
resource "aws_db_instance" "example" {
  identifier         = "mydb"
  engine             = "mysql"
  instance_class     = "db.t3.micro"
  allocated_storage  = 20
  name               = "mydb"
  username           = "admin"
  password           = "admin123"
  vpc_security_group_ids = [aws_security_group.rds_sg.id]
  db_subnet_group_name   = aws_db_subnet_group.example.name
  skip_final_snapshot    = true
}
resource "aws_db_subnet_group" "example" {
  name       = "my-subnet-group"
  subnet_ids = [aws_subnet.subnet1.id, aws_subnet.subnet2.id]
}

#### 6.Q: How do you create handlers in Ansible?
#### playbook.yml
tasks:
  - name: Install nginx
    apt:
      name: nginx
      state: present
    notify: restart nginx

handlers:
  - name: restart nginx
    service:
      name: nginx
      state: restarted
    
#### 7.Q: What is the typical directory structure of an Ansible project?
project/

├── inventory

├── playbook.yml

├── roles/

│   └── webserver/

│       ├── tasks/

│       │   └── main.yml

│       ├── handlers/

│       │   └── main.yml

│       ├── templates/

│       ├── files/

│       ├── vars/

│       └── defaults/

#### 8.Q: How can you set up password-less authentication in Linux using SSH?
1.Generate SSH key
    ssh-keygen
2.Copy public key to remote server:
    ssh-copy-id user@remote-server
3. Now you can SSH without a password:
    ssh user@remote-server
#### 9.Q: What is SELinux and how does it work?
SELinux (Security-Enhanced Linux) is a security module in Linux that provides mandatory access control (MAC). It defines access policies for users, processes, and files.

Check status:
               Sestatus
Modes: 
Enforcing: Enforces policies.
Permissive: Logs violations, doesn’t enforce.
Disabled: Completely off.

#### 10.Q: What is AWS Serverless and what are its key benefits?

AWS Serverless refers to a cloud-native development model that allows you to build and run applications without managing servers. Key services include:
•	AWS Lambda – run code in response to events.
•	Amazon API Gateway – expose APIs.
•	Amazon DynamoDB – NoSQL database.
•	AWS Step Functions – orchestrate workflows


#### 11. What is the difference between Declarative and Scripted Pipeline in Jenkins?
Declarative Pipeline: Uses a more structured and predefined syntax. Easier to write and read, especially for beginners.
Scripted Pipeline: Uses Groovy-based syntax. Offers more flexibility and is suitable for complex logic.

#### 12, How can you upgrade Jenkins?
1, Backup Jenkins (Recommended) 
Backup your Jenkins home directory (usually /var/lib/jenkins):(sudo cp -r /var/lib/jenkins /var/lib/jenkins_backup)
2, Upgrade Jenkins (Using Package Manager)
sudo yum check-update
sudo yum upgrade jenkins
sudo systemctl restart jenkins

#### 13. What is Jenkins Master-Slave Architecture?
Master: The main Jenkins server that manages the overall environment — it schedules builds, handles the UI, and delegates tasks to agents.
Slave (Agent): Remote machines that connect to the master and run the actual build/test jobs. They can be Linux, Windows, or container-based systems.

#### How it works:
Master receives a trigger (e.g., code push).
Assigns the job to an available slave.
Slave executes the job and reports back the result.
Connection methods: SSH, JNLP, or WebSocket.

#### Why use it?
Load distribution
Run jobs in parallel
Use different environments for different jobs (e.g., Java on one, Python on another)


#### 14. What is SonarQube?
SonarQube is a code quality and security analysis tool.
It inspects code for bugs, vulnerabilities, and code smells.
Integrates with Jenkins, GitHub, and other CI/CD tools.

#### 15. How to integrate GitHub with Jenkins?
Install Git and GitHub plugin in Jenkins.
Create a GitHub Personal Access Token.
In Jenkins, go to Manage Jenkins > Configure System > GitHub and add credentials.
Set up webhooks in GitHub to trigger builds on code push.

#### 16. What is the command to check memory in Linux?
free -h

#### 17. What is the command to check file size in Linux?
du -sh filename

#### 18. How can you search for a word in Linux?
grep 'word' filename
To search recursively in all files: grep -r 'word' /path/to/directory

#### 19. What are Shared Libraries in Jenkins?
Shared libraries allow you to reuse common code across multiple Jenkins pipelines.
They are stored in a separate Git repo or directory structure and loaded using:
@Library('library-name') _( in shell script)




#### 20. what is the difference between git revert and git reset?
git revert creates a new commit that undoes changes of a specific commit without altering commit history (safe for shared branches).
git reset moves the HEAD and possibly updates the index or working directory (can rewrite history; not safe for shared branches).

#### 21. what isthe difference between git fetch and git pull?
git fetch downloads changes from the remote repository but doesn’t apply them.
git pull is equivalent to git fetch followed by git merge; it updates your current branch.

#### 22. How do you troubleshoot if an application is running slow on Linux?
Check CPU/memory usage: top, htop, vmstat
Check disk I/O: iostat, iotop, df -h
Check network issues: netstat, ping, traceroute
Check logs: /var/log/syslog, application-specific logs
Check processes: ps aux --sort=-%mem

#### 23. Write a shell script to check if drive storage is full and send an alert to the manager.
#!/bin/bash

THRESHOLD=90
EMAIL="manager@example.com"

df -H | grep -vE '^Filesystem|tmpfs|cdrom' | awk '{ print $5 " " $1 }' | while read -r output; do
  usep=$(echo "$output" | awk '{ print $1}' | tr -d '%')
  partition=$(echo "$output" | awk '{ print $2 }')

  if [ "$usep" -ge "$THRESHOLD" ]; then
    echo "Running out of space on $partition ($usep%) as on $(date)" | \
    mail -s "Disk Alert: $partition usage is at $usep%" "$EMAIL"
  fi
done


#### 24. What is AWS VPC peering?
A: VPC peering is a networking connection between two VPCs that enables routing traffic between them using private IPs. Peering works across regions and accounts but does not support transitive peering.

#### 25: What is terraform taint used for?
A: It marks a specific resource for destruction and recreation on the next terraform apply.

#### 26: What are Terraform Workspaces?
A: Workspaces allow you to manage multiple state files within a single configuration directory. Useful for managing different environments like dev, staging, and prod.

#### 27. Explain Jenkins architecture.
A: Jenkins follows a master-agent architecture.
The master schedules builds, manages agents, and handles web UI.
Agents execute build jobs on different platforms or environments.

#### 28. What is the difference between Jenkins agents and labels?
A: Agents are machines (nodes) that run jobs.
Labels are tags you assign to agents to group them for job scheduling (e.g., linux, docker).

#### 29. What is a Docker multi-stage build file?
A: Multi-stage builds allow you to use multiple FROM statements in a Dockerfile to separate build-time and runtime environments, reducing final image size.

#### 30 What are Kubernetes Stateful Services?
A: Stateful services retain state across restarts. StatefulSets manage pods with stable network identity, persistent storage, and ordered, graceful deployment.

#### 31, what is the kubernetes architecture?
o	Master Node: Controls the Kubernetes cluster. It contains several components:
o	API Server: Exposes the Kubernetes API.
o	Controller Manager: Ensures that the cluster is in the desired state (e.g., creating new pods when needed).
•	Scheduler: Assigns workloads to nodes.
•	ETCD: is a distributed key-value store used to store all cluster data, including configuration data, secrets, and state information.
•	Worker Node: Runs the containerized applications. Components include:
•	Kubelet: Ensures the containers are running in a Pod.
•	Kube Proxy: Maintains network rules for Pod communication.
•	Container Runtime: Runs the containers (e.g., Docker).

#### 32.What are different types of Kubernetes Services?
A:
ClusterIP – Default, internal-only access
NodePort – Exposes service on a port on each node
LoadBalancer – Uses external load balancer
ExternalName – Maps service to external DNS name
Headless Service – Created by setting clusterIP: None, used for direct pod access, useful in StatefulSets and DNS discovery.

#### 33. Q: What is AWS Serverless and what are its key benefits?
A:
AWS Serverless refers to a cloud-native development model that allows you to build and run applications without managing servers. Key services include:
•	AWS Lambda – run code in response to events.
•	Amazon API Gateway – expose APIs.
•	Amazon DynamoDB – NoSQL database.
•	AWS Step Functions – orchestrate workflows.
Benefits:
•	No server provisioning or management.
•	Auto-scaling and high availability.
•	Pay only for what you use (event-driven).

#### 34. Q: What is AWS Fargate and how does it differ from EC2-based ECS?
A:
AWS Fargate is a serverless compute engine for containers that works with ECS and EKS.
Key Differences from EC2-based ECS:
•	Fargate: No need to provision or manage EC2 instances.
•	EC2: You manage the EC2 infrastructure, networking, patching, etc.
Use case: When you want to run containers without managing the underlying infrastructure.

#### 35. Q: What is AWS Glue and what is it used for?
A:
AWS Glue is a fully managed ETL (Extract, Transform, Load) service used for:
•	Discovering, cataloging, and transforming data.
•	Preparing data for analytics and machine learning.
Components:
•	Glue Crawlers – automatically detect schema and create metadata tables.
•	Glue Jobs – run PySpark or Python scripts for ETL.
•	Glue Data Catalog – central metadata repository.

#### 36. Q: What is AWS EventBridge Scheduler and how is it used?
A:
Amazon EventBridge Scheduler (formerly CloudWatch Events Scheduler) is a fully managed scheduler for running tasks or workflows at defined times or intervals.
Use cases:
•	Schedule Lambda functions or Step Functions.
•	Start/stop EC2 or RDS instances.
•	Trigger Glue jobs or ECS tasks periodically.
Example:
json
CopyEdit
{
  "ScheduleExpression": "rate(5 minutes)",
  "Target": {
    "Arn": "arn:aws:lambda:region:account-id:function:MyFunction"
  }
}

#### 37. Q: What is Amazon ECS (Elastic Container Service)?
A:
Amazon ECS is a fully managed container orchestration service that lets you run and scale Docker containers.
Key Concepts:
•	Cluster – logical grouping of tasks or services.
•	Task Definition – blueprint for your container (image, CPU, memory, etc.).
•	Services – keep tasks running, support scaling and load balancing.
Modes:
•	EC2 launch type
•	Fargate launch type (serverless)

#### 38. Q: What is Amazon ECR (Elastic Container Registry)?
A:
Amazon ECR is a fully managed Docker container registry that makes it easy to store, manage, and deploy container images.
Features:
•	Integrated with ECS, EKS, and Lambda.
•	Secure access via IAM.
•	Supports image versioning and scanning.
Common Commands:
#### Authenticate Docker to ECR
aws ecr get-login-password | docker login --username AWS --password-stdin <account_id>.dkr.ecr.<region>.amazonaws.com

#### Push Docker image
docker build -t my-image .
docker tag my-image:latest <account_id>.dkr.ecr.<region>.amazonaws.com/my-image:latest
docker push <account_id>.dkr.ecr.<region>.amazonaws.com/my-image:latest

#### 39. Q: What AWS resources have you worked with?
A:
I’ve worked with a wide range of AWS resources, including:
•	EC2 (virtual machines)
•	S3 (object storage)
•	RDS (managed databases)
•	ECS/Fargate (containers)
•	Lambda (serverless compute)
•	IAM (access control)
•	CloudWatch (monitoring/logs)
•	VPC (networking)
•	Route 53 (DNS)
•	Auto Scaling Groups
•	ALB/NLB (load balancers)
•	Elastic Beanstalk, CloudFormation, and Terraform for provisioning

#### 40. Q: What AWS services do you use for scaling instances?
A:
For automatic and manual scaling, I’ve used:
•	Auto Scaling Groups (ASG): Automatically add/remove EC2 instances based on CPU, memory, or custom metrics.
•	Elastic Load Balancer (ELB): Distributes traffic across instances to balance load.
•	CloudWatch Alarms: Used to trigger scaling actions.
•	ECS with Fargate or EC2: Task-based scaling based on request load or queue depth.

#### 41. Q: Which command is used in Linux to check disk usage?
A:
df -h       #### Shows disk usage in human-readable format
du -sh *    #### Shows folder sizes in the current directory
#### 42. Q: How can you delete the contents of a file in Linux without deleting the file itself?
A:
> filename           #### Truncates the file
: > filename         #### Same as above
truncate -s 0 filename  # Explicitly sets file size to 0
#### 43. If we get a lock while executing terraform plan, how to unlock it?
 Answer:
Use the following command to manually unlock the Terraform state:
terraform force-unlock <LOCK_ID>
Only use it if you're sure no other process is actively using the state.

#### 44. If we get an error while pipeline execution, how do you solve it?
Answer:
•	Check pipeline logs to identify the exact stage and error message.
•	Reproduce the issue locally (if possible).
•	Fix config or script issues (e.g., syntax, credentials).
•	Rerun the pipeline and monitor.

#### 45 . What are the steps to push your code to a central repo (e.g., GitHub)?
Answer:
git init                                     #### Initialize local repo
git add .                                 #### Stage changes
git commit -m "message"     #### Commit changes
git remote add origin <repo-url>             #### Link to central repo
git push -u origin main                 #### Push code

#### 46 . How can you create infrastructure at a time in two AWS accounts using Terraform?
Answer:
•	Define two different provider blocks with different credentials:
provider "aws" {
  alias  = "account1"
  region = "us-east-1"
  profile = "account1-profile"
}
provider "aws" {
  alias  = "account2"
  region = "us-west-2"
  profile = "account2-profile"
}
•	Use provider = aws.account1 and provider = aws.account2 in resources.

#### 47. How can we secure Docker containers?
Answer:
•	Use minimal base images (e.g., alpine).
•	Run containers as non-root users.
•	Regularly scan images for vulnerabilities.
•	Use Docker secrets for sensitive data.
•	Enable network and runtime restrictions.
•	Keep Docker and host OS updated.

#### 48 . count vs for_each in Terraform
Answer:
Feature	                     count	                            for_each
Use Case	            Repetition by number	                  Repetition by map/set
Indexing	             Uses count.index	                  Uses each.key / each.value
Best for	                      Lists                                               	                 Maps or sets (with named keys)

#### 49 . Inbuilt Terraform functions you use
Answer:
•	length() – get list length
•	lookup() – safe map value lookup
•	join() – join strings
•	split() – split string to list
•	merge() – merge maps
•	format() – formatted strings
•	element() – get item by index
•	file() – read local file

#### 50 . Difference between ALB and NLB
Answer:
Feature	ALB (Application Load Balancer)	NLB (Network Load Balancer)
Layer	Layer7 (HTTP/HTTPS)	Layer4 (TCP/UDP)
Features	Path-based, host-based routing	Fast TCP handling, static IP
Use Case	Web apps, HTTP APIs	Low latency apps, real-time systems

#### 51 . How do you connect to a private subnet?
Answer:
•	Use a bastion host (jump box) in the public subnet.
•	Or use Session Manager (SSM) if agents are installed.
•	Optionally use a VPN or Direct Connect.

#### 52 . AWS Lambda vs AWS Fargate
Answer:
Feature	AWS Lambda	AWS Fargate
Type	Serverless functions	Serverless containers
Use Case	Short event-driven tasks	Long-running container apps
Timeout	Max15 minutes	No hard timeout
Pricing	Per request + duration	Based on vCPU and memory used

#### 53. How can you print output line by line in shell scripting?
Answer:
while read line; do
  echo "$line"
done < filename.txt

#### 54. What is OAI (Origin Access Identity) in CloudFront?
Answer:
OAI is used to restrict access to an S3 bucket so only CloudFront can fetch content, preventing direct access via S3 URL.

#### 55. How can you use if-else in Terraform?
Answer:
Use ternary operator:
variable "env" {}
output "instance_type" {
  value = var.env == "prod" ? "t3.large" : "t2.micro"
}



#### 56. What is rollback mechanism in Jenkins?
Answer:
•	Use Git to revert to a previous commit and re-deploy.
•	In pipeline, define a rollback stage to deploy last stable artifact (e.g., using Nexus/S3).
•	Tools like ArgoCD, Ansible, or Helm can assist with rollbacks in CD.

#### 57. What is Maven lifecycle?
Answer:
Maven has 3 built-in lifecycles:
•	clean – cleans previous build (mvn clean)
•	default – main build (compile, test, package, install, deploy)
•	site – generates documentation

#### 58. How can you skip test phase in Maven?
Answer:
mvn install -DskipTests
Or skip completely:
mvn install -Dmaven.test.skip=true

#### 59. Difference between Maven, Ant, and Jenkins?
Answer:
Tool Purpose Key Feature
Maven Build tool Convention over configuration
Ant	Build tool Procedural (manual steps)
Jenkins	CI/CD automation Executes pipelines, integrates tools
________________________________________
#### 60. What are Kubernetes namespaces?
Answer:
Namespaces logically isolate resources in a cluster. Example: dev, test, prod environments in the same cluster.

#### 61. What is Argo CD?
Answer:
Argo CD is a GitOps tool for Kubernetes. It continuously syncs your Kubernetes cluster state with Git repositories.

#### 62. What is Ingress in Kubernetes?
Answer:
Ingress exposes HTTP and HTTPS routes from outside the cluster to services inside using rules and host/path-based routing.

#### 63. Difference between Docker and Kubernetes?
Answer:
Feature	Docker	Kubernetes
Purpose	Containerization	Orchestration of containers
Focus	Single container	Managing many containers
Deployment	Manual or Compose	Declarative YAML with auto-scaling

#### 64. How many ways can you create Docker images?
Answer:
#### 1.	Using a Dockerfile (recommended)
#### 2.	Using docker commit from a running container
#### 3.	Programmatically with Docker SDK

#### 65. What are other lines than shebang in shell scripts?
Answer:
•	Comments (#)
•	Variable declarations
•	Logic/commands (if, echo, loops)
•	Function definitions

#### 66. What does a Shebang line represent?
Answer:
Starts with #!, tells the system which interpreter to use. Example:

#!/bin/bash
#### 67. How can you modify permissions in Linux?
Answer:
Use chmod:
chmod 755 file.sh
Use chown to change ownership:
 chown user:group file

#### 68. What is crontab, and what are allow/deny files?
Answer:
•	crontab schedules jobs.
•	/etc/cron.allow – only users in this file can use crontab.
•	/etc/cron.deny – users listed here cannot use crontab.

#### 69. How can you kill a process in Linux?
Answer:
kill <PID>
kill -9 <PID>       #### force kill

#### 70. How can you check CPU utilization?
Answer:
Use:
top
htop
mpstat

#### 71. How can you check if a port is open and listening?
Answer:
netstat -tuln | grep 80
ss -tuln | grep 80

#### 72. How can you check if the network is working using traceroute?
Answer:
traceroute google.com
It shows the path and delays to the destination.

#### 73. How can you capture last 20 lines of a file?
Answer:
tail -n 20 filename.log

#### 74. What are the features of Jenkins?
Answer:
•	Open-source CI/CD tool
•	Supports pipeline-as-code
•	Plugins for Docker, Kubernetes, Git, etc.
•	Can build, test, deploy automatically

#### 75. How can you build pipelines in Jenkins?
Answer:
•	Use Declarative or Scripted pipeline in a Jenkinsfile
•	Use UI to create pipeline jobs and add stages
•	Example:
groovy
pipeline {
  stages {
    stage('Build') {
      steps {
        echo 'Building...'
      }
    }
  }
}

#### 76. How can you check script failures in shell scripts?
Answer:
Use:
set -x  # Prints each command before execution

set -e  # Stops on error

#### 77. What do $#, $@, $*,& mean in shell scripts?
Answer:
Actually:
•	$# – Number of script arguments
•	$@ – All arguments as separate words
•	$* – All arguments as one word
•	& – Run process in background

#### 78. How do you execute commands in Linux?
Answer:
•	Directly in shell: ls, echo, cd
•	Or in script files with .sh extension
•	Use bash script.sh or ./script.sh

#### 79. In which environments do you work?
Answer:
•	Dev – Development
•	QA – Testing
•	UAT – User Acceptance Testing
•	Prod – Live/production

#### 80. Do you know about UAT?
Answer:
Yes. User Acceptance Testing is the final testing phase where end-users validate the software before going to production.

#### 81. What is Minikube?
Answer:
Minikube runs a local Kubernetes cluster inside a VM or container for learning and development purposes.

#### 82. If storage is full on a Linux server, how do you manage it?
Q: What steps do you take when a Linux server runs out of disk space?
A: I check disk usage with df -h and identify large files or directories using du -sh * | sort -h. I clear logs (/var/log), old Docker images (docker image prune), and cached packages (apt clean, yum clean all). If necessary, I increase the volume size or mount additional storage.

#### 83. If you are getting an SSH error, what do you do?
Q: How do you troubleshoot SSH connection issues?
A: I check network reachability using ping or telnet, confirm correct IP, key permissions (chmod 400 for PEM), verify SSH service status, and review /var/log/auth.log or /var/log/secure. I also confirm the security group/firewall allows port 22.

#### 84. Difference between ALB and NLB?
Q: What’s the difference between Application Load Balancer (ALB) and Network Load Balancer (NLB)?
A: ALB works at Layer 7 (HTTP/HTTPS), supports path-based routing, host-based routing, and WebSockets. NLB works at Layer 4 (TCP/UDP), is faster and handles millions of requests with low latency. Use ALB for web apps, NLB for performance-critical TCP services.

#### 85. How can you recover a deleted S3 object?
Q: How do you restore a deleted S3 object?
A: If versioning was enabled, I can retrieve the deleted object using a previous version. Without versioning, the object is permanently deleted unless S3 backup (e.g., replication or lifecycle rule to Glacier) is configured.

#### 86. How do you differentiate between a public and a private subnet in AWS?
Q: How do you identify a public vs private subnet?
A: A public subnet has a route to the internet via an internet gateway (IGW). A private subnet lacks this route and usually uses a NAT gateway for internet access. I verify this by checking route tables.

#### 87. How to recreate infrastructure in Terraform?
Q: How do you recreate Terraform-managed resources?
A: I use terraform taint <resource> to mark a resource for recreation, or terraform destroy followed by terraform apply to recreate everything. I also use terraform state rm if needed to remove a resource from state before recreating.

#### 88. What is a remote module in Terraform?
Q: What is a remote Terraform module and how do you use it?
A: A remote module is a reusable configuration hosted in a repo (like GitHub, Terraform Registry). It’s used via a module block with a source URL. Example:
hcl
CopyEdit
module "vpc" {
  source = "terraform-aws-modules/vpc/aws"
  version = "3.0.0"
  ...
}

#### 89. What are modules in Terraform?
Q: Why do you use modules in Terraform?
A: Modules group and reuse Terraform configurations, promoting clean and DRY code. They help manage large infrastructure by encapsulating resources like VPCs, EC2s, or databases into logical components.

#### 90. How can you pass passwords securely in Terraform?
Q: How do you handle sensitive values like passwords in Terraform?
A: I use terraform.tfvars or environment variables and mark them as sensitive in the variable block. For storage, I use tools like AWS Secrets Manager or HashiCorp Vault and access them using data blocks.

#### 91. What is the difference between CMD and ARG in a Dockerfile?
Q: What’s the difference between CMD and ARG in Docker?
A: ARG defines variables at build-time, while CMD provides defaults for runtime. Example:
dockerfile
CopyEdit
ARG VERSION=1.0
CMD ["node", "app.js"]

#### 92. How do you secure Docker images?
Q: What steps do you take to secure Docker images?
A: I use minimal base images (like Alpine), scan images with tools like Trivy or Docker Scout, avoid hardcoding secrets, use .dockerignore, and sign images using Docker Content Trust or Notary.

#### 93. How do you secure a 3-tier architecture?
Q: How do you secure a 3-tier app (web, app, DB)?
A: I use security groups and NACLs to isolate layers:
•	Web tier: Public subnet with limited inbound (HTTP/HTTPS).
•	App tier: Private subnet, allows traffic only from web tier.
•	DB tier: Private subnet, accessible only by app tier.
Enable encryption (TLS, KMS), IAM roles, and monitoring (CloudWatch, GuardDuty).

#### 94. How do you expose your app to the internet in Kubernetes?
Q: How do you expose an application in Kubernetes to the internet?
A: I use a Service of type LoadBalancer or Ingress. For complex routing and HTTPS, I prefer using an Ingress controller (like NGINX or ALB Ingress).

#### 95. How do you connect Jenkins to the cloud (AWS)?
Q: How do you connect Jenkins to cloud environments like AWS?
A: I configure Jenkins with AWS CLI/SDK or IAM credentials (via credentials plugin). I install plugins like AWS EC2, use IAM roles (on EC2 agents), and store secrets in AWS Secrets Manager or Jenkins credentials.

#### 96. How many VPCs can you create per region?
Answer:
By default, you can create 5 VPCs per region per AWS account. This limit can be increased by requesting a quota increase from AWS.

#### 97. What is the difference between a private and public subnet?
Answer:
•	Public Subnet: A subnet that is associated with a route table that has a route to an Internet Gateway (IGW). Resources in this subnet can access the internet.
•	Private Subnet: A subnet that does not have a route to the Internet Gateway. Used for internal resources like databases.

#### 98. What is a Transit Gateway?
Answer:
An AWS Transit Gateway enables you to connect multiple VPCs and on-premises networks through a central hub, simplifying your network architecture and reducing the number of peering connections.

#### 99. What is VPC Peering?
Answer:
VPC Peering allows direct communication between two VPCs in the same or different AWS accounts/regions. It’s non-transitive and is used for point-to-point connectivity.

#### 100. What is a VPC Endpoint?
Answer:
A VPC Endpoint allows private connection between your VPC and AWS services (like S3, DynamoDB) without using the internet, improving security and performance.

#### 101. How can you restore an RDS snapshot with a custom database name?
Answer:
You cannot directly rename a database when restoring a snapshot. Instead:
#### 1.	Restore the snapshot.
#### 2.	Create a new DB instance.
#### 3.	Use tools like pg_dump/mysqldump, or AWS DMS to export and import data into a DB with the desired name.

#### 102. How can you create an HTTPD application on 100 EC2 instances using an Ansible playbook?
Answer:
Define the inventory of 100 EC2 instances.

Write a playbook to install and start HTTPD:

yaml
---
- hosts: webservers
  become: yes
  tasks:
    - name: Install httpd
      yum:
        name: httpd
        state: present
    - name: Start httpd service
      service:
        name: httpd
        state: started
        enabled: yes

Run the playbook: ansible-playbook -i inventory.ini playbook.yml

#### 103. How can you troubleshoot if an application is not working on an EC2 instance?
Answer:
•	Check EC2 instance status (Running/Reachable).
•	Verify Security Groups and NACLs (port access).
•	Check application logs (/var/log/, journalctl, etc.).
•	Confirm service status (systemctl status).
•	Check CPU/memory/disk usage.
•	Test network connectivity (ping, telnet, curl).


#### 104. How can you connect S3 to an EC2 instance?
Answer:
•	Attach an IAM Role to EC2 with S3 access permissions (e.g., AmazonS3ReadOnlyAccess).
•	Use AWS CLI or SDK on EC2:
aws s3 ls s3://your-bucket-name

#### 105. What is the difference between Security Group and Network ACL (NACL)?
Answer:
Feature	Security Group	NACL
Level	Instance-level	Subnet-level
Stateful	Yes	No
Rules	Allow only	Allow and Deny
Applies to	EC2 Instances	Subnets
Default Behavior	Deny all unless allowed	Allow all unless changed

#### 106. What are EC2 instance types?
Answer:
EC2 instances are categorized based on use case:
Instance Type	Use Case	Examples
t-series	Burstable general purpose	t2.micro, t3.small
m-series	General purpose	m5.large, m6g.medium
c-series	Compute optimized	c5.large, c6g.xlarge
r-series	Memory optimized	r5.large, r6g.xlarge
i-series	Storage optimized	i3.large, i4i.xlarge
g/p-series	GPU/Accelerated computing	g4dn.xlarge, p3.2xlarge

#### 107. What should you do if a Pod crashes?
Answer:
•	Check logs: kubectl logs <pod-name>
•	Describe pod: kubectl describe pod <pod-name>
•	Check events and container status for error messages.
•	Investigate issues like:
o	CrashLoopBackOff
o	ImagePull errors
o	OOMKilled (Out of Memory)
o	Misconfigurations in YAML (ports, env vars, etc.)

#### 108. What are the different scaling strategies in Kubernetes?
Answer:
•	Manual Scaling: Using kubectl scale command or editing the deployment.
•	Horizontal Pod Autoscaler (HPA): Scales pods based on CPU/memory utilization.
•	Vertical Pod Autoscaler (VPA): Adjusts CPU/memory requests/limits.
•	Cluster Autoscaler: Automatically adds/removes nodes based on pod needs.

#### 109. What are the Deployment strategies in Kubernetes?
Answer:
•	Rolling Update (default): Gradually replaces old pods with new ones.
•	Recreate: Deletes old pods before creating new ones.
•	Blue/Green Deployment: Deploys new version alongside old one, then switches.
•	Canary Deployment: Gradually rolls out to a small subset before full rollout.

#### 110. How can you pause a container?
Answer:
Kubernetes doesn't directly support pausing containers, but you can:
•	Use kubectl rollout pause deployment/<deployment-name> to pause updates.
•	Use Linux SIGSTOP/SIGCONT signals in advanced container runtime setups.

#### 111. What are Init Containers?
Answer:
Init containers are special containers that run before app containers in a Pod. They:
•	Run sequentially.
•	Are used for initial setup tasks (e.g., configs, waiting for DB readiness).
•	Must complete successfully for the main container to start.

#### 112. What are Sidecar Containers?
Answer:
Sidecars are helper containers that run alongside the main container in the same pod.
Examples:
•	Logging agent
•	Data synchronizer
•	Proxy (like Envoy for service mesh)

#### 113. What are the different types of containers in Kubernetes?
Answer:
•	App Containers: Primary application logic.
•	Init Containers: Run before app containers for setup tasks.
•	Sidecar Containers: Provide supporting features (logging, monitoring).
•	Ambassador Containers: Help with service communication/proxying.

#### 114. How can you troubleshoot if a Namespace is renamed?
Answer:
•	Namespaces can’t be renamed, only deleted and recreated.
•	If a resource disappears:
o	Check with kubectl get all --all-namespaces
o	Validate configs still reference the correct namespace.

#### 115. What is etcd?
Answer:
etcd is a distributed key-value store used by Kubernetes to store all cluster state data (like config, secrets, nodes, etc.). It must be highly available and backed up.

#### 116. What is a CRD (Custom Resource Definition)?
Answer:
A CRD lets you define a custom resource (e.g., MySQLCluster) and use it like a native Kubernetes object. It extends Kubernetes capabilities without modifying the core.

#### 117. What are common Kubernetes Operations (Day2 Ops)?
Answer:
•	Scaling workloads
•	Monitoring & logging
•	Rolling updates
•	Backup & restore (etcd, volumes)
•	Debugging pods
•	Resource limits and quota management

#### 118. What is a Service Mesh?
Answer:
A Service Mesh manages communication between services. Features:
•	Traffic management
•	Security (mTLS)
•	Observability (metrics, tracing)
Examples: Istio, Linkerd

#### 119. What is Sidecar Injection?
Answer:
Sidecar injection is the process of automatically adding a sidecar container (like an Envoy proxy) to pods. This is used in service meshes (e.g., Istio) for traffic interception.

#### 120. What is Envoy Proxy?
Answer:
Envoy is a high-performance proxy used in service meshes (e.g., Istio) for:
•	Load balancing
•	Traffic routing
•	TLS termination
•	Observability

#### 121. What is a Pod Disruption Budget (PDB)?
Answer:
PDB ensures a minimum number of pods are always available during voluntary disruptions (like node drain). You can define:
•	minAvailable
•	maxUnavailable

#### 122. What are Probes in Kubernetes?
Answer:
Used to check pod health:
•	Liveness Probe: Restarts container if it's stuck.
•	Readiness Probe: Controls pod availability to services.
•	Startup Probe: For slow-starting apps.

#### 123. What is the difference between Voluntary and Involuntary Disruption?
Answer:
•	Voluntary: Triggered by user (e.g., kubectl drain, rolling update).
•	Involuntary: System-triggered (e.g., node crash, OOM).
#### 124. What is Safe Eviction vs Hard Eviction?
Answer:
•	Safe Eviction: Graceful shutdown respecting Pod Disruption Budgets and lifecycle hooks.
•	Hard Eviction: Forced eviction due to resource pressure or system errors.

#### 125. What are Pod Security Policies (PSP)?
Answer:
(Deprecated in Kubernetes v1.25)
PSPs controlled security-related settings like:
•	Privileged mode
•	Host namespaces
•	Volume types
•	User IDs
Use Pod Security Admission (PSA) instead in newer versions.

#### 126. What is CrashLoopBackOff error?
Answer:
This occurs when a container keeps crashing repeatedly. Causes:
•	Application error
•	Misconfiguration
•	Unavailable dependencies
Fix:
•	Check logs, describe pod, check readiness/liveness probes.

#### 126. What is a StatefulSet?
Answer:
Used to manage stateful applications:
•	Each pod has a persistent identity.
•	Ordered, graceful deployment and scaling.
•	Stable network names and storage (e.g., databases).

#### 127. What is a Headless Service?
Answer:
A service with clusterIP: None:
•	Doesn't assign a cluster IP.
•	DNS returns the pod IPs directly.
•	Used with StatefulSets for service discovery.

#### 128. What is a ReplicaSet?
Answer:
Maintains a stable set of pod replicas.
•	Ensures desired number of pods are running.
•	Used by Deployments internally.


#### 129. What is a Deployment object in Kubernetes?
Answer:
A Deployment is used to:
•	Manage ReplicaSets
•	Perform rolling updates
•	Rollback to previous versions
•	Scale pods

#### 130. What is a DaemonSet?
Answer:
Ensures that a copy of a pod runs on all (or selected) nodes.
Use cases:
•	Log collection (e.g., Fluentd)
•	Monitoring agents (e.g., Prometheus Node Exporter)

#### 131. What is the SLA of each and every incident in your current organisation
####A:
In our organisation we have 4 types of incidents based on priority and urgency i.e., P1, P2, P3, P4
The SLA is 2, 8, 72 and 96 hours respectively.

#### 132. Did you ever be involved in change management, what is the process of creating CRQ's in your current organisation ?
Regarding change management, we generally raise a change when ever there is some configuration or patching is being done on production environment servers. 
We have to raise a change and get in approved in CAB (Change Advisory Board) call. In CAB meeting we discuss the potential risks and dependencies involved to implement the change and all the respective stakeholders holders or teams linked to that change are informed about the activity. Later it should be approved by manager or team lead, the multiple level of approvals depends on individual teams and organisations.

#### 133. Do you have any idea about the prod and pre pod environment.
Prod environment is generally production environment, this is what the end consumers get to see, like all the websites and links that we usually get accesses like bank websites, Flipkart and Amazon apps

Pre prod environment is where all the testing is done before going live in production environment. If everything goes well in Pre prod then we push the updates and patches to production environment.

#### 134. What is GitHub Actions Pipeline?
Answer:
GitHub Actions is a CI/CD automation tool provided by GitHub. It allows you to define workflows in .github/workflows/*.yml files to automate processes like:
Code build
Test
Deployment

Structure:

yaml

name: CI Pipeline
on: [push]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Run build
        run: echo "Build complete"
		
#### 135. How do you deploy from staging to preprod using CI/CD?
Answer:

Define environments: staging, preprod
Use manual approval (environments protection rules) for preprod.
Sample GitHub Actions deployment step:

yaml

jobs:
  deploy-staging:
    ...
  deploy-preprod:
    needs: deploy-staging
    environment:
      name: preprod
      url: https://preprod.example.com
    steps:
      - name: Deploy to preprod
        run: ./deploy.sh
Use a deployment strategy like:
Use if conditions to check branch or tag.
Use environment with required reviewers in GitHub.

#### 136. How can you run a specific task in Ansible?
Answer:
Use tags in the playbook:

yaml

tasks:
  - name: Install Apache
    apt:
      name: apache2
      state: present
    tags: install
Run specific task with:
ansible-playbook site.yml --tags install

#### 137. How do you SSH into a remote server and run a command?
Example:

ssh username@ip "ls -l /path/to/dir"
Replace username@ip with the actual user and server IP (e.g., ubuntu@192.168.1.10).

#### 138. Bash: How do you print the last word of a sentence?
Question: Write a bash command to print the last word.

Answer:

echo "This is a sentence" | awk '{print $NF}'
$NF = Number of Fields, i.e., last field.

#### 139. This is incorrect syntax: Echo | awk -F ‘{pritn($NF)} — Why?
Issue:

Wrong quote characters
pritn is a typo, should be print
-F not needed unless using a specific delimiter
Corrected version:
echo "Hello world" | awk '{print $NF}'

#### 140. Bash: Write a script to print the first word of a string
Answer (Script):

#!/bin/bash
read -p "Enter a sentence: " sentence
echo "$sentence" | awk '{print $1}'

Usage:
$ ./firstword.sh
Enter a sentence: Hello from DevOps
Hello
