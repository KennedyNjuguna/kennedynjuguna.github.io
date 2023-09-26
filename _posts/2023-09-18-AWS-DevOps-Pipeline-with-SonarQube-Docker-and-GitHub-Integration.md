

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

Create an EC2 instance and choose Ubuntu as the instance type. Make sure the Instance type has reached the minimum requirements for SonarQube Installation. (atleast t2.medium)
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

Navigate back to the ssh instance and copy the command to the terminal. This installs Jenkins to the instance
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
![Jenkins-Instance](/assets/images/favicon/InstanceIP.PNG)

![Jenkins-Instance](/assets/images/favicon/sshSonarqube.PNG)

Install Java JRE 11 
![Jenkins-Instance](/assets/images/favicon/InstallJRE.PNG)

Navigate to SonarQube website. Click on the download version of your choice and copy the link. Navigate to the terminal and use the wget command and paste the link to download SonarQube
![Jenkins-Instance](/assets/images/favicon/DownloadSonarQube.PNG)

As the file is in the .zip format Install UnZip app
![Jenkins-Instance](/assets/images/favicon/InstallUnzip.PNG)

Unzip the file and navigate to the sonarqube folder
![Jenkins-Instance](/assets/images/favicon/Unzip.PNG)

![Jenkins-Instance](/assets/images/favicon/SonarqubeFolder.PNG)

In the SonarQube folder go to the bin subfolder then depending on your OS go to your folder either macos, windows or linux
![Jenkins-Instance](/assets/images/favicon/FoldersandSubfolders.PNG)

Execute the batch file in the folder (sonar.sh)
![Jenkins-Instance](/assets/images/favicon/ExecuteBashFile.PNG)

Navigate back to the aws console, in your instance under security click the security group rules so as to edit the inbound rules to allow for port 9000
![Jenkins-Instance](/assets/images/favicon/SecurityGroup.PNG)

Add a new rule that allows for inbound from anywhere through port 9000 and save rule
![Jenkins-Instance](/assets/images/favicon/EditInboundRules.PNG)

![Jenkins-Instance](/assets/images/favicon/Rule.PNG)

On operational status, navigate to the instance, copy its public ip and paste it in a new browser and access SonarQube through port 9000. Use admin as username and password to login. Update the password on the next prompt
![Jenkins-Instance](/assets/images/favicon/SonarWebsite.PNG)

Click "Create project manually" and give a project name and key
![Jenkins-Instance](/assets/images/favicon/CreateProjectManually.PNG)

![Jenkins-Instance](/assets/images/favicon/ProjectNameandKey.PNG)

As our CI tools is Jenkins choose it as the "How do you want to analyze your repository" option and select your DevOps platform
![Jenkins-Instance](/assets/images/favicon/AnalyzewithJenkins.PNG)

![Jenkins-Instance](/assets/images/favicon/DevopsPlatform.PNG)

![Jenkins-Instance](/assets/images/favicon/OtheronJenkinsfile.PNG)

Navigate to the "My Account" option and click on "security". Create a token
![Jenkins-Instance](/assets/images/favicon/MyAccount.PNG)

![Jenkins-Instance](/assets/images/favicon/Token.PNG)
Navigate back to the Jenkins website and click on "manage Jenkins" then click on "Plugins" to install plugins
![Jenkins-Instance](/assets/images/favicon/Plugins.PNG)

Install "SonarQube Scanner" and "SSH2 Easy" Plugins
![Jenkins-Instance](/assets/images/favicon/InstallPlugin.PNG)

![Jenkins-Instance](/assets/images/favicon/InstallPlugin2.PNG)

Navigate back to the Jenkins website and click on "manage Jenkins" then click on "Tools" and go to "Add SonarQube Scanner" 
![Jenkins-Instance](/assets/images/favicon/AddSonarqubeScanner.PNG)

Navigate back to the Jenkins website and click on "manage Jenkins" then click on "System" and go to "SonarQube Servers" and ADD SonarQube. Make sure to copy the SonarQube URL to the server url option then save. 
![Jenkins-Instance](/assets/images/favicon/SonarQubeScanner.PNG)

Navigate back to the Jenkins-pipeline you created and click on configure. Add a build step to execute the SonarQube Scanner and paste the key to the "Analysis properties"
![Jenkins-Instance](/assets/images/favicon/PipelineConfigure.PNG)

![Jenkins-Instance](/assets/images/favicon/ADDBuildStep.PNG)

![Jenkins-Instance](/assets/images/favicon/AnalysisProperties.PNG)

Navigate back to the Jenkins website and click on "manage Jenkins" then click on "System" so as to add the token we created and paste the token and give it an ID and save it. Now select the token and click on save.
![Jenkins-Instance](/assets/images/favicon/AddToken.PNG)

![Jenkins-Instance](/assets/images/favicon/SecretTextToken.PNG)

Go back to the pipeline and build and verify its working. Navigate back to the sonarqube website and refresh the page and ensure the code passed.
![Jenkins-Instance](/assets/images/favicon/BuildWorking.PNG)

![Jenkins-Instance](/assets/images/favicon/SonarqubePassed.PNG)

## STEP THREE: DEPLOYING CODE TO DOCKER SERVER

SSH into the docker instance
![Jenkins-Instance](/assets/images/favicon/SSHDocker.PNG)

Navigate to the Install docker website and choose your OS then follow the "Install using the Apt repository" instructions
![Jenkins-Instance](/assets/images/favicon/DockerWebsite.PNG)

Navigate back to the Jenkins website and click on "manage Jenkins" then click on "System" to add the Docker SerVER. Look for "Server Groups Center" and add a group. Input the details and click on save
![Jenkins-Instance](/assets/images/favicon/GroupAdd.PNG)

![Jenkins-Instance](/assets/images/favicon/ServerGroupAdd.PNG)

Navigate back to the Jenkins website and click on "manage Jenkins" then click on "System" to add the Docker SerVER. Look for "Server Groups Center" and add a server. Input the servername and use the Docker-Instance IP as "Server IP"
![Jenkins-Instance](/assets/images/favicon/ServerAdd.PNG)

![Jenkins-Instance](/assets/images/favicon/ServerAdd2.PNG)

Navigate back to the Jenkins-pipeline you created and click on configure. Add a build step to execute the remote shell.
![Jenkins-Instance](/assets/images/favicon/RemoteShell.PNG)

![Jenkins-Instance](/assets/images/favicon/RemoteShell2.PNG)
  
Navigate back to the GitHub Repository and create a new DockerFile
![Jenkins-Instance](/assets/images/favicon/DockerFile.PNG)

Navigate back to the configure tab and add build step "Execute Shell". and type the command "scp ./ubuntu@(Docker-Instance-ip):~/(path of folder you want to save contents)
![Jenkins-Instance](/assets/images/favicon/ExecuteShell.PNG)

![Jenkins-Instance](/assets/images/favicon/ExecuteShell2.PNG)

***IT IS NOT RECOMMENDED SAVING THE "DOCKERFILE" IN SAVE REPO AS WEBSITE***

Verify contents of repo are in the folder
![Jenkins-Instance](/assets/images/favicon/repocontents.PNG)


Navigate back to the Jenkins-pipeline you created and click on configure. Add a build step to execute the remote shell. Remember to allow for ports configured in the Instance
![Jenkins-Instance](/assets/images/favicon/RemoteShellBuild.PNG)


***YOUR WEBSITE SHOULD NOW BE WORKING***
![Jenkins-Instance](/assets/images/favicon/congrats.PNG)


