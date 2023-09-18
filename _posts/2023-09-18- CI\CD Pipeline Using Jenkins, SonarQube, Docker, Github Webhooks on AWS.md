# AWS DevOps Pipeline with SonarQube, Docker, and GitHub Integration"

 Welcome to this project, where we will harness the power of Jenkins CI/CD Pipeline, SonarQube, Docker, and GitHub Webhooks on the robust platform of Amazon Web Services (AWS). With Jenkins as our orchestrator, we automate every step of the development lifecycle, ensuring seamless integration, testing, and deployment. SonarQube, our code quality sentinel, scrutinizes every line of code to maintain the highest standards of software excellence. Docker empowers us to containerize our applications, granting us unparalleled portability and scalability. GitHub, the cornerstone of modern software development, is seamlessly integrated through webhooks, allowing us to synchronize our source code and collaborate effortlessly.

So, whether you're an experienced DevOps engineer or just embarking on your journey, join us as we navigate through the intricacies, challenges, and triumphs of our project. Together, we'll uncover how these tools, working in harmony on the AWS cloud, can revolutionize your development pipeline and set you on a path to DevOps excellence. Welcome aboard!

## STEP ONE

Login in to the AWS Management Console and create three EC2 Instances:

* EC2 Instance for Jenkins

Create an EC2 instance and choose Ubuntu as the instance type. 

![Jenkins-Instance](assets/images/favicon/EC2JENKINS.PNG)

![Jenkins-Instance](assets/images/favicon/EC2jENKINS2.PNG)

Create a Key pair to allow for SSH access to your instance
![Jenkins-Instance](assets/images/favicon/EC2jENKINS3.PNG)

Launch Instance.
![Jenkins-Instance](assets/images/favicon/EC2jENKINS4.PNG)

* EC2 Instance for SonarQube

Create an EC2 instance and choose Ubuntu as the instance type. 
![Jenkins-Instance](assets/images/favicon/EC2SONARQUBE1.PNG)
![Jenkins-Instance](assets/images/favicon/EC2SONARQUBE2.PNG)

Use the Key we generated for Jenkins and Lauch Instance.
![Jenkins-Instance](assets/images/favicon/EC2SONARQUBE3.PNG)

*EC2 Instance for Docker

Create an EC2 instance and choose Ubuntu as the instance type.
![Jenkins-Instance](assets/images/favicon/EC2DOCKER.PNG)
![Jenkins-Instance](assets/images/favicon/EC2DOCKER2.PNG)

Use the Key we generated for Jenkins and Lauch Instance.
![Jenkins-Instance](assets/images/favicon/EC2DOCKER3.PNG)
