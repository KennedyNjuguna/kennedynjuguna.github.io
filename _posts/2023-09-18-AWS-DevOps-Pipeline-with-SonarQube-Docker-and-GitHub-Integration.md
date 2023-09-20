

 Welcome to this project, where we will harness the power of Jenkins CI/CD Pipeline, SonarQube, Docker, and GitHub Webhooks on the robust platform of Amazon Web Services (AWS). With Jenkins as our orchestrator, we automate every step of the development lifecycle, ensuring seamless integration, testing, and deployment. SonarQube, our code quality sentinel, scrutinizes every line of code to maintain the highest standards of software excellence. Docker empowers us to containerize our applications, granting us unparalleled portability and scalability. GitHub, the cornerstone of modern software development, is seamlessly integrated through webhooks, allowing us to synchronize our source code and collaborate effortlessly.

So, whether you're an experienced DevOps engineer or just embarking on your journey, join us as we navigate through the intricacies, challenges, and triumphs of our project. Together, we'll uncover how these tools, working in harmony on the AWS cloud, can revolutionize your development pipeline and set you on a path to DevOps excellence. Welcome aboard!

## STEP ONE

Login in to the AWS Management Console and create three EC2 Instances:

* **EC2 Instance for Jenkins**

Create an EC2 instance and choose Ubuntu as the instance type. 

![Jenkins-Instance](/assets/images/favicon/EC2Jenkins.PNG)

![Jenkins-Instance](/assets/images/favicon/EC2jENKINS2.PNG)

Create a Key pair to allow for SSH access to your instance
![Jenkins-Instance](/assets/images/favicon/EC2JENKINS3.PNG)

Launch Instance.
![Jenkins-Instance](/assets/images/favicon/EC2JENKINS4.PNG)


* **EC2 Instance for SonarQube**

Create an EC2 instance and choose Ubuntu as the instance type. Make sure the Instance type has reached the minimum requirements for Sonarqube Intallation. (atleast t2.medium)
![Jenkins-Instance](/assets/images/favicon/EC2SONARQUBE1.PNG)

![Jenkins-Instance](/assets/images/favicon/EC2SONARQUBE2.PNG)

Use the Key we generated for Jenkins and Lauch Instance.
![Jenkins-Instance](/assets/images/favicon/EC2SONARQUBE3.PNG)

***EC2 Instance for Docker**

Create an EC2 instance and choose Ubuntu as the instance type.
![Jenkins-Instance](/assets/images/favicon/EC2DOCKER.PNG)

![Jenkins-Instance](/assets/images/favicon/EC2DOCKER2.PNG)

Use the Key we generated for Jenkins and Lauch Instance.
![Jenkins-Instance](/assets/images/favicon/EC2DOCKER3.PNG)

## STEP TWO

SSH into the Jenkin Instance to Install Jenkins into the Instance

Copy the Public ip of the instance
![Jenkins-Instance](/assets/images/favicon/IPPIC.PNG)

Open a terminal and navigate to the directory where you stored the Key.SSH into your instance by using ssh -i (name of key) ubuntu/windows/macOs@public ip adress
![Jenkins-Instance](/assets/images/favicon/Terminal.PNG)

Update your instance using "sudo apt update"
![Jenkins-Instance](/assets/images/favicon/Update.PNG)

Install JavaRuntime Environment in the instance by using "sudo apt install openjdk-11-jre"
![Jenkins-Instance](/assets/images/favicon/installJRE.PNG)

Navigate to the website [Jenkins Site](jenkins.io) and click on Installing Jenkins under Documentation
![Jenkins-Instance](/assets/images/favicon/JenkinsWebsite.PNG)

Choose your operating system and copy the command
![Jenkins-Instance](/assets/images/favicon/copycode.PNG)

Navigate back to the ssh insyance and copy the command to the terminal. This installs Jenkins to the instance
![Jenkins-Instance](/assets/images/favicon/COPYJENKINCODE.PNG)

Navigate back to the aws console, in your instance under security click the security group rules so as to edit the inbound rules to allow for port 80
![Jenkins-Instance](/assets/images/favicon/Networking.PNG)

![Jenkins-Instance](/assets/images/favicon/securitygroups.PNG)

Add a new rule that allows for inbound from anywhere through port 8080 and save rule
![Jenkins-Instance](/assets/images/favicon/EDITINBOUDRULES.PNG)

![Jenkins-Instance](/assets/images/favicon/Port8080.PNG)

Use the command "systemctl status jenkins" to verify the installation of Jenkins.
![Jenkins-Instance](/assets/images/favicon/validateinstallation.PNG)

On running status, navigate to the instance, copy its public ip and paste it in a new browser and access Jenkins through port 8080
![Jenkins-Instance](/assets/images/favicon/url.PNG)

When prompted for password use "sudo cat /var/lib/jenkins/secrets/initialAdminPassword" to access your default passowrd.
![Jenkins-Instance](/assets/images/favicon/jenkinspassword.PNG)

Paste the password and click continue. Click on Install Suggested plugins
![Jenkins-Instance](/assets/images/favicon/clickselectedplugins.PNG)


Create a user and create a password then click on save and continue then finish. 
![Jenkins-Instance](/assets/images/favicon/CreateUser.PNG)

![Jenkins-Instance](/assets/images/favicon/saveandfinish.PNG)

# USE JENKINS

Create Pipieline by clicking on new item and give it a name
![Jenkins-Instance](/assets/images/favicon/NewItem.PNG)

Select the project type then click OK
![Jenkins-Instance](/assets/images/favicon/ProjectType.PNG)

Navigate to "Source Code Management" and choose git
![Jenkins-Instance](/assets/images/favicon/SourceCodeManagement.PNG)

Copy the URL for the website repository from github under code,HTTPS and paste it under "Repository URL"
![Jenkins-Instance](/assets/images/favicon/URLRepository.PNG)

Enable the "GitHub hook trigger for GITScm polling" to trigger the pipeline automatically when changes are made to the repository
![Jenkins-Instance](/assets/images/favicon/GithubHookTrigger.PNG)

Navigate back to the Github Repo Settings and go to webhook. Click "Add Webhook"
![Jenkins-Instance](/assets/images/favicon/AddWebhook.PNG)

Copy the Jenkins URL and paste in under "Payload URL" adding "/github-webhook/"
![Jenkins-Instance](/assets/images/favicon/PayloadURL.PNG)

Ensure "Pull Request" and "Pushes" are ticked under "Which events would you like to trigger this webhook?". Add Webhoook
![Jenkins-Instance](/assets/images/favicon/PullRequestsandPushes.PNG)

![Jenkins-Instance](/assets/images/favicon/PullRequestsandPushes2.PNG)




***WE HAVE AUTOMATED THE PROCESS WHEREBY WHEN A DEVELOPER CHANGES A CODE, JENKINS IS AUTOMATICALLY TRIGERED AND PULLS THE CODE FROM GITHUB***


## STEP THREE: CREATING A SERVER FOR THE SONARQUBE

Copy the Instance IP Address if the Sonarqube Instance and SSH into the instance


Install Java JRE 11 


Navigate to SonarQube website. Click on the download version of your choice and copt yhe link

Naviagte to the terminal and use the wget command and paste the link to download SonarQube


As the file is in the .zip format Install UnZip app

Unzip the file and navigate to the sonarqube folder

In the SonarQube folder go to the bin subfolder then depending on your OS go to your folder either macos, windows or linux

Execute the batch file in the folder (sonar.sh)

Navigate back to the aws console, in your instance under security click the security group rules so as to edit the inbound rules to allow for port 9000

Add a new rule that allows for inbound from anywhere through port 9000 and save rule

On operational status, navigate to the instance, copy its public ip and paste it in a new browser and access SonarQube through port 9000. Use admin as username and password to login. Update the password on the next prompt


Click "Create project manually" and give a project name and key


As our CI tools is Jenkins choose it as the "How do you want to analyze your repository" option and select your DevOps platform


Navigate to the "My Account" option and click on "security". Create a token


Navigate back to the Jenkins website and click on "manage Jenkins" then click on "Plugins" to install plugins


Install "SonarQube Scanner" and "SSH2 Easy" Plugins


Navigate back to the Jenkins website and click on "manage Jenkins" then click on "Tools" and go to "Add SonarQube Scanner" 

Navigate back to the Jenkins website and click on "manage Jenkins" then click on "System" and go to "SonarQube Servers" and ADD SonarQube. Make sure to copy the SonarQube URL to the server url option then save. 


Navigate back to the Jenkins-pipeline you created and click on configure. Add a build step to execute the SonarQube Scanner and paste the key to the "Analysis properties"

Navigate back to the Jenkins website and click on "manage Jenkins" then click on "System" so as to add the token we created and paste the token and give it an ID and save it. Now select the token and click on save.


Go back to the pipeline and build and verify its working. Navigate back to the sonarqube website and refresh the page and ensure the code passed.


## STEP THREE: DEPLOYING CODE TO DOCKER SERVER

SSH into the docker instance

Navigate to the Install docker website and choose your OS then follow the "Install using the Apt repository" instructions





  
