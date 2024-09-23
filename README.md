# CICD_CapstoneProject

# Setting Up a Secure CI/CD Pipeline with Jenkins, Docker, and Ansible on AWS

# Step 1: Setting Up AWS Instances for Jenkins and Docker
Log into AWS and create two instances, as shown in the screenshots below, to separate the CI/CD process from the container runtime environment. This setup enhances security and resource management. 

![image](https://github.com/user-attachments/assets/cf863782-e4cc-43ca-99c5-b6cef8c4af80)
![image](https://github.com/user-attachments/assets/8b0fc1d7-7e1b-422c-b6f6-5912ddcdef57)
![image](https://github.com/user-attachments/assets/faf652ef-ad8f-4ebe-b3f0-7a7094e42ec7)

Key Pair Creation: While creating the instances, generate a key pair to securely connect to them. The key pair will automatically download to our local system’s downloads folder.
![image](https://github.com/user-attachments/assets/73b7e7b2-0a3b-4ac6-8748-f9d08ba07ec7)
![image](https://github.com/user-attachments/assets/5060da8e-ff6a-4db4-8fa3-6c64fa5a24d8)
![image](https://github.com/user-attachments/assets/87dbf225-1135-4d36-96b7-d1fc703ce238)
![image](https://github.com/user-attachments/assets/be39ef05-63bc-431f-916e-1d6e743f76ff)
Once the instances are created, rename the second instance to 'docker-container'.
![image](https://github.com/user-attachments/assets/adbb489d-29e5-46f9-98ff-b9ad9f5928db)
![image](https://github.com/user-attachments/assets/511dcc11-d051-4958-8d85-1cf0d476ea29)
Next, access the security groups for the Jenkins instance and add an inbound rule in AWS to allow custom TCP traffic on port 8080 for communication.
![image](https://github.com/user-attachments/assets/8e95d8d4-9170-4d3a-9b4e-205110ed83c0)
![image](https://github.com/user-attachments/assets/c16ff460-7a26-4d76-85a4-7cf339df9992)
![image](https://github.com/user-attachments/assets/0d85c1d3-69e8-444a-be0a-205bccf85b7b)
![image](https://github.com/user-attachments/assets/eed947fc-ba6e-44d7-9731-db628ae54cee)

# Step 2: Connect to the Jenkins server securely by using SSH key pair securely in local machine:
Open the terminal and connect to the Jenkins instance on an AWS EC2 server securely by using the following command with specified SSH key pair.  
This allows remote access and management of the Jenkins server from our local machine.    

![image](https://github.com/user-attachments/assets/8579affc-e75b-4012-b9ab-11fc2b56a02f)
Change the hostname of the machine to "jenkins"  
$ sudo hostnamectl set-hostname Jenkins  
Change the shell to bash to reflect the updated hostname.  
$ /bin/bash  
![image](https://github.com/user-attachments/assets/da5c2ca7-0559-4865-84e8-e754e1c338fa)

# Step 3: Installing Jenkins:
Update the package lists for available upgrades.  
$ sudo apt update  
![image](https://github.com/user-attachments/assets/ccbcc077-66a0-4f68-88ff-0c75cd4f8ed9)
Follow the installation instructions from the Jenkins documentation.
Install Java, a prerequisite for Jenkins.  
$ sudo apt install openjdk-17-jre  
![image](https://github.com/user-attachments/assets/91275107-1881-4210-a5c3-b010ab082f3f)
Download the Jenkins repository GPG key and Install Jenkins.
![image](https://github.com/user-attachments/assets/457c522e-c904-4937-b6d6-042ca8d050ad)
Check the status of Jenkins service and retrieve the initial password.  
$ systemctl status jenkins  
![image](https://github.com/user-attachments/assets/7cf30191-bc50-4169-876d-ecb61114f684)
Open Jenkins in the browser using the public IP address (e.g., http://54.208.2.242:8080/) and unlock it with the secret password.
![image](https://github.com/user-attachments/assets/a9939ec6-b736-4f7b-a142-1bbe44c6b8d9)
Install the suggested plugins and create the first admin user. 
![image](https://github.com/user-attachments/assets/cd3e7a51-7cea-4a0d-8cd0-d711d31787c3)
![image](https://github.com/user-attachments/assets/80e15a41-689b-4aab-9c0e-fcd49467047b)
![image](https://github.com/user-attachments/assets/3be71216-61db-4317-80a5-9af37925ff9d)
Click on ‘Start using Jenkins’.
![image](https://github.com/user-attachments/assets/279054d2-6c2f-4c51-af7b-e1f5817e408d)

# Step 4: Setting up Jenkins Pipeline:
Create a new pipeline named 'ansible-jenkins-pipeline' and configure a Freestyle project with Git SCM. 
![image](https://github.com/user-attachments/assets/89fa012d-f31d-4be7-b115-88bb5e6830d3)
![image](https://github.com/user-attachments/assets/c294e2ff-fc95-4469-a137-78e8c604697a)
Provide the GitHub repository URL in the SCM section. 
![image](https://github.com/user-attachments/assets/b6c9a9c3-e625-4d6b-9d31-d54b830af042)
Specify the branches to build under 'Build Triggers'.
![image](https://github.com/user-attachments/assets/ade2972e-b26a-46df-94ea-57aa69915b37)
Enable GitHub Webhooks to trigger events in Jenkins upon code changes, automating the CI process. 
![image](https://github.com/user-attachments/assets/290c324f-0d80-4cc0-852d-35b862b87871)
"Build Steps - Execute Shell", provide custom shell commands or scripts to run during the build process. This is useful for automating tasks such as running tests, deploying applications, copying files, building Docker images, or executing Ansible playbooks as part of our CI/CD pipeline.
Provide two custom commands to copy files from the Jenkins workspace to the specified directory on the Docker server and execute Ansible playbooks for deployment.
Click on Apply and Save to create the pipeline.
![image](https://github.com/user-attachments/assets/7e971711-cd80-49a5-823f-a1d05324d138)

# Step 5: GitHub Configuration:
Go to your GitHub settings and add a webhook with the Payload URL (e.g., http://54.208.2.242:8080/github-webhook/) to enable communication.
![image](https://github.com/user-attachments/assets/5af34964-63d4-4afe-bd56-2eddfa1b1e08)
![image](https://github.com/user-attachments/assets/efbb8f68-e89f-40ed-83b7-cdda29cfedf1)
![image](https://github.com/user-attachments/assets/5d309ecb-3494-4b62-9175-cad2330e1ca1)
![image](https://github.com/user-attachments/assets/49607ebb-30dd-428f-a67c-0902034a9e62)

# Step 6: Testing the Pipeline:
After creating the pipeline, manually click on 'Build Now' to ensure it builds successfully without errors. 
![image](https://github.com/user-attachments/assets/6d614fef-39b7-43a5-b82b-31de2fc5288e)
![image](https://github.com/user-attachments/assets/0a7ded2a-d2b2-4546-9d58-a4aad693735f)
Next, make changes to the GitHub repository and commit the code to verify that the pipeline triggers and builds automatically in Jenkins. 
![image](https://github.com/user-attachments/assets/6d923cd4-ab99-4040-9d57-13bca1423c28)

The pipeline triggered automatically.
![image](https://github.com/user-attachments/assets/fae84ebf-5e89-4540-a7bb-464430573abb)
![image](https://github.com/user-attachments/assets/175fb12e-9c41-47f0-81c6-176177f092cd)

# Step 7: Connect to the Docker server securely by using SSH key pair securely in local machine:
Open the second terminal and connect to the Docker instance on an AWS EC2 server securely by using the following command with specified SSH key pair.   
This allows remote access and management of the Docker server from our local machine  
![image](https://github.com/user-attachments/assets/21f33daf-4dbf-49f0-8020-d8672b210dbc)
Change the hostname of the machine to "docker"  
$ sudo hostnamectl set-hostname docker  
Change the shell to bash to reflect the updated hostname.  
$ /bin/bash  
![image](https://github.com/user-attachments/assets/a7262635-f96f-49e8-a299-0b7604170e4e)

# Step 8: Installing Docker:
Follow the commands from the Docker documentation to install Docker and related tools.
![image](https://github.com/user-attachments/assets/876cfc94-8d2c-4c72-8c50-c9dd876c56f1)
![image](https://github.com/user-attachments/assets/6190f30b-bcc2-41d0-bffe-a4a9c498d094)
![image](https://github.com/user-attachments/assets/76334b29-3c75-4676-9f5e-61c46d9c88e7)

Add the current user to the docker group to authenticate to use any docker commands.  
$ sudo usermod -aG docker ubuntu  
Refresh group membership to apply the changes.  
$ newgrp docker  
Check the running containers.  
$ docker ps  
![image](https://github.com/user-attachments/assets/fb741f5c-1066-4672-bc3e-98efd8bdacd9)
Switch to the root user and generate SSH key pairs for secure access.  
Navigate to the '.ssh/' directory, open the authorized_keys file, and paste the public key from the Jenkins instance for passwordless SSH authentication. This allows the Jenkins server to securely connect to the Docker instance without needing to enter a password each time. This setup is essential for automating deployment tasks, as it facilitates seamless communication between the Jenkins server and the Docker instance during the CI/CD process.
![image](https://github.com/user-attachments/assets/c56ea02f-7f56-4add-a2cd-291c0aff0efb)
![image](https://github.com/user-attachments/assets/a8088e25-73b4-4640-bb59-8b3b3037538e)

Creating the target Directory on Docker instance:
Create a new directory named 'project' to serve as the target folder by using below commands.  
mkdir project  
cd project  
ls  --> Initially, no files will be present.  
![image](https://github.com/user-attachments/assets/79a7c4c1-0172-4225-9015-ca3da926864e)
After executing the Ansible playbooks, files will appear in the project folder, which will later be used to build Docker images and containers. 
![image](https://github.com/user-attachments/assets/f9c313ae-4b79-4b77-88ed-1ee71d42869a)

# Step 9: Installing Ansible on Jenkins Instance:
Install Ansible on the Jenkins instance by following the instructions from the Ansible documentation:
![image](https://github.com/user-attachments/assets/6248d0df-a709-4dae-a1cb-25e6af3410b9)
![image](https://github.com/user-attachments/assets/fac76007-67f9-4071-a4ce-660b0fbdc4fd)

To run Docker commands on the Jenkins instance, install additional dependencies:  
$ sudo apt install python3-pip  
$ pip install docker  
![image](https://github.com/user-attachments/assets/c23a03fc-b257-47fa-a33a-de3137dcb771)
![image](https://github.com/user-attachments/assets/face1dbe-c051-43fb-8164-24eb86f5109a)
![image](https://github.com/user-attachments/assets/cf9bcf82-a678-4e57-8186-1c4bf116ea08)
Configuring Ansible Hosts:
Navigate to the '/etc/ansible/' directory and open the hosts file to add the Docker server details, including its name and public IP address. 
![image](https://github.com/user-attachments/assets/159102dd-2ef4-4db4-974e-fbeacd84c097)
![image](https://github.com/user-attachments/assets/cbea8da6-529a-4814-8e60-dddf72b0f704)
Switch to the root user and then Jenkins user, to generate an SSH key for Jenkins to communicate with Docker.
![image](https://github.com/user-attachments/assets/5694bc52-29ee-4e1b-989a-2e868557586c)
Navigate to the ‘.ssh/’ directory and copy the public key which will get pasted in the authorized key file of dockers instance:  
ssh public key: ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIMypipQBMPdugO8+g/rcVFLBkg/Q7Fq6zxQexC7V62vM jenkins@jenkins
![image](https://github.com/user-attachments/assets/1f7cddf7-2086-452b-816c-04aff52faee7)
After pasting the public key into the dockers instance’s authorized_keys, test the connection to the Docker server from the Jenkins instance. 
![image](https://github.com/user-attachments/assets/2025f0cf-e39d-4e14-bf9b-e743b3c6d344)
![image](https://github.com/user-attachments/assets/10b95d0d-09ba-4e00-ab0e-e762004835b0)

# Step 10: Creating Deployment Playbook:
Create a 'playbook' directory on the Jenkins instance and a deployment.yaml file containing scripts for building and deploying Docker containers. 
![image](https://github.com/user-attachments/assets/cc0f944a-705d-48b5-a9ee-f62b35cdbfe0)

This playbook includes deployment tasks such as stopping and removing containers, building Docker images, and creating containers on the target remote server (Docker server).
![image](https://github.com/user-attachments/assets/1d3d864a-ed8a-456c-a979-1cd9f69b9b1c)

Run the ansible playbook “deployment.yaml” to deploy the application manually.
![image](https://github.com/user-attachments/assets/61a1c7fb-00ea-4059-965a-22c5fdf830ad)
After successful execution of playbook, Docker images and containers will be created on the Docker server. 
![image](https://github.com/user-attachments/assets/c981ae2e-aec6-403a-86eb-b24c9d5003a6)
Delete the files from the project directory and test the pipeline by making changes in the GitHub repository to trigger it automatically.
![image](https://github.com/user-attachments/assets/a9d2e419-6f24-4cbe-9122-f2ab9e65db16)

# Step 11: Final Testing:
Modify the code in the GitHub repository to check that the pipeline triggers and builds the latest changes automatically. For example, change the title and some content in the index.html file and commit the changes.
![image](https://github.com/user-attachments/assets/6c6f6946-1a39-4c8c-bb8a-c71855d64e36)
![image](https://github.com/user-attachments/assets/293aeef9-741c-466d-85ee-ce42d24036c7)
After committing, the pipeline should trigger automatically, executing the Ansible playbook to deploy the application in the Docker container. 
![image](https://github.com/user-attachments/assets/8b49e54e-262b-489f-a4bf-ea44ec96590c)
Before committing, verify the web page URL content. 
![image](https://github.com/user-attachments/assets/1f473cf4-1cd5-4704-8ea1-3019128c73fc)
After committing, check for the updated changes.
![image](https://github.com/user-attachments/assets/44a623fb-1b7e-42d0-85d9-19f2a9e855f3)
