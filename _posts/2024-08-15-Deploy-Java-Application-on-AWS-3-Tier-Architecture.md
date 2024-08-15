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

### Install Cloudwatch agent
Navigate to the Official AWS documentation and follow the instructions given to install CloudWatch Agent

[How to Install the CloudWatch agent](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/install-CloudWatch-Agent-on-EC2-Instance.html)

### Install AWS SSM agent
Navigate to the Official AWS documentation and follow the instructions given to install AWS SSM agent

[How to Manually install and uninstall SSM Agent on EC2 instances for Linux](https://docs.aws.amazon.com/systems-manager/latest/userguide/manually-install-ssm-agent-linux.html)

### Creating AMI from the instance














