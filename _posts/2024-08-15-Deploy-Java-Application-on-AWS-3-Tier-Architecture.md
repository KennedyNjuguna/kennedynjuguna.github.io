![3-Tier-Architecture](https://github.com/user-attachments/assets/67ee557d-365b-42ca-951e-207f1df641e7)

## STEP ONE: Global AMI
We are going to create a Global AMI with the following installations

* **AWS CLI**
* **Cloudwatch agent**
* **Install AWS SSM agent**

Go to AWS Console, Search for EC2 on the searchbar and click on instances. Launch an instance with default settings. Make sure to create a key pair. 
![image](https://github.com/user-attachments/assets/7b8eecf5-179f-4aa8-b097-f8bfa8c0599c)

SSH Into the instance to install
* **AWS CLI**
* **Cloudwatch agent**
* **Install AWS SSM agent**

### Install AWS CLI
Navigate to the Official AWS documentation and follow the instructions given to install AWS CLI depending on the operating system you are using

[How to Install or update to the latest version of the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)

![image](https://github.com/user-attachments/assets/2b20c94f-caff-4f0d-8b64-e85793b70c79)

![image](https://github.com/user-attachments/assets/f529cb84-a98d-4efb-adc5-615432f15ef8)


### Install Cloudwatch agent
Navigate to the Official AWS documentation and follow the instructions given to install CloudWatch Agent

[How to Install the CloudWatch agent](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/install-CloudWatch-Agent-on-EC2-Instance.html)

![image](https://github.com/user-attachments/assets/51da8e61-b4ba-42b4-ac4c-dca896cd299b)


### Install AWS SSM agent
Navigate to the Official AWS documentation and follow the instructions given to install AWS SSM agent

[How to Manually install and uninstall SSM Agent on EC2 instances for Linux](https://docs.aws.amazon.com/systems-manager/latest/userguide/manually-install-ssm-agent-linux.html)

![image](https://github.com/user-attachments/assets/864fcb80-ad77-4d12-ade5-1599e8575859)


### Creating AMI from the instance
Navigate back to the instance dashboard and click on the instance. Go to *actions* then *images and templates* then to *create image* 

![image](https://github.com/user-attachments/assets/8aec1ff3-d829-4a2e-b260-c234de54fc9e)

## STEP TWO: Create Golden AMI using Global AMI for Nginx application

Launch an instance using the *Global AMI* we created, by navigating to *AMIs* selecting the *Global AMI* then selecting *Launch instance from AMI* 
![image](https://github.com/user-attachments/assets/bfd45b83-dcd9-4266-80ee-d32bf045f10e)

SSH into the instance so as to: 

* **Install Nginx**
* **Push custom memory metrics to Cloudwatch**

### Install Nginx
Navigate to the Official Nginx Website and check on the documentation to install Nginx on the Instance

[How to Install nginx on Amazon Linux 2023](https://docs.nginx.com/nginx/admin-guide/installing-nginx/installing-nginx-open-source/)

### Push custom memory metrics to Cloudwatch


[How to push custom metrics to CloudWatch](https://repost.aws/knowledge-center/cloudwatch-push-custom-metrics)

You can use SSM to install the CloudWatch agent then use the wizard to write the configuration file

```sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-config-wizard```

After you have finished with the configuration, start the cloudwatch agent with 

```sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -a fetch-config -m ec2 -c file:configuration-file-path -s```

Your configuration file path is listed after finishing the configuration with the wizard



## STEP THREE: Create Golden AMI using Global AMI for Apache Tomcat Application

Use the AMI created to launch another instance. 

SSH into the instance so as to: 

* **Install Apache Tomcat**
* **Configure Tomcat as Systemd service**
* **Install JDK 11**

### Install Apache Tomcat

You can use the instructions from the official AWS Website

[How toInstall Apache Tomcat](https://repost.aws/questions/QUxJyO-GtaSXesjCpv7InqXg/amazon-linux-2023-tomcat-versions-available)

To  Configre Tomcat as Systemd service
Step1: Create the systemd file: nano /etc/systemd/system/tomcat.service
Step2: Copy 
```
[Unit]
Description=Apache Tomcat
After=network.target

[Service]
Type=forking

User=tomcat10
Group=tomcat10

ExecStart=/opt/apache-tomcat-10.1.28/bin/startup.sh
ExecStop=/opt/apache-tomcat-10.1.28/bin/shutdown.sh
```
* **Make sure you edit that and use your installation folder**
Step3: Add a user ``` useradd tomcat10 ```
Step4: Change user password ```passwd tomcat10```
Step5: Run the cmd ```sudo systemctl daemon-reload```

From now on, you can use following commands to start or stop the tomcat:
```
sudo service tomcat start
sudo service tomcat stop
```


### Install JDK 11

[How to install Java 11 on Amazon Linux 2023](https://linux.how2shout.com/how-to-install-java-on-amazon-linux-2023/)

### Push custom memory metrics to Cloudwatch

[How to push custom metrics to CloudWatch](https://repost.aws/knowledge-center/cloudwatch-push-custom-metrics)

You can use SSM to install the CloudWatch agent then use the wizard to write the configuration file

```sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-config-wizard```

After you have finished with the configuration, start the cloudwatch agent with 

```sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -a fetch-config -m ec2 -c file:configuration-file-path -s```

Your configuration file path is listed after finishing the configuration with the wizard


## STEP FOUR: Create Golden AMI using Global AMI for Apache Maven Build Tool




