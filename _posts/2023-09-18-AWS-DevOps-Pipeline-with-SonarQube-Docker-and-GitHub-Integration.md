

 Welcome to this project, where we will harness the power of Jenkins CI/CD Pipeline, SonarQube, Docker, and GitHub Webhooks on the robust platform of Amazon Web Services (AWS). With Jenkins as our orchestrator, we automate every step of the development lifecycle, ensuring seamless integration, testing, and deployment. SonarQube, our code quality sentinel, scrutinizes every line of code to maintain the highest standards of software excellence. Docker empowers us to containerize our applications, granting us unparalleled portability and scalability. GitHub, the cornerstone of modern software development, is seamlessly integrated through webhooks, allowing us to synchronize our source code and collaborate effortlessly.

So, whether you're an experienced DevOps engineer or just embarking on your journey, join us as we navigate through the intricacies, challenges, and triumphs of our project. Together, we'll uncover how these tools, working in harmony on the AWS cloud, can revolutionize your development pipeline and set you on a path to DevOps excellence. Welcome aboard!

**## STEP ONE**

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

Create an EC2 instance and choose Ubuntu as the instance type. 
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

**## STEP TWO**

SSH into the Jenkin Instance to Install Jenkins into the Instance

* Copy the Public ip of the instance

* Open a terminal and navigate to the directory where you stored the Key.

* SSH into your instance by using ssh -i (name of key) ubuntu/windows/macOs@public ip adress

* update your instance using "sudo apt update"

* Install JavaRuntime Environment in the instance by using "sudo apt install openjdk-11-jre"

* Navigate to the website [Jenkins Site](jenkins.io) and click on Installing Jenkins under Documentation

* Choose your operating system and copy the command

* Navigate back to the ssh insyance and copy the command to the terminal. This installs Jenkins to the instance

* Navigate back to the aws console, in your instance under security click the security group rules so as to edit the inbound rules to allow for port 80

* Add a new rule that allows for inbound from anywhere through port 8080 and save rule

* Use the command "systemctl status jenkins" to verify the installation of Jenkins.

* On running status, navigate to the instance, copy its public ip and paste it in a new browser and access Jenkins through port 8080

* When prompted for password use "sudo cat /var/lib/jenkins/secrets/initialAdminPassword" to access your default passowrd.

* Paste the password and click continue. Click on Install Suggested plugins

* Create a user and create a password then click on save and continue then finish. 

  

  
