# APPLICATION MIGRATION TO AWS


Welcome to this project!! In this Project we are going to lift and shift an application to the AWS cloud using the Appplication Migration Service. The objective is to show how an on-premise application is transferred to an the AWS Cloud seamlessly with its associated data and configurations. This ensures that your application experiences minimal disruptions while also leveraging the features that AWS has to offer.

***NB:*** **AS I AM NOT CURRENTLY HOSTING ON-PREM SERVERS, WE WILL LIFT AND SHIFT AN APPLICATION SERVER FROM THE AZURE CLOUD PLATFORM TAKING THE VMs CREATED IN AZURE AS THE ON-PREM SERVERS**

## Table of Contents

- [STEPS I WILL TAKE](#steps-i-will-take)
  * [Creating Servers On-Prem](#creating-servers-on-prem)
  * [Connecting to the On-Prem server to install apps](#connecting-to-the-on-prem-server-to-install-apps)
  * [Create a replication server in AWS and download agent](#create-a-replication-server-in-aws-and-download-agent)
  * [Install agent on the On-Prem Windows Server](#install-agent-on-the-on-prem-windows-server)
  * [Launch the test process](#launch-the-test-process)
  * [Cutover Process](#cutover-process)

## STEPS I WILL TAKE
- Create Servers On-Prem (using Azure as the on-prem environment)
- Connect to the On-Prem server to install apps
- Create a replication server in AWS and download agent
- Install agent on the On-Prem Windows Server
- Launch replication; edit replication template, launch template and cutover template
- Launch the test process
- Cutover Process

## Creating Servers On-Prem
Login to the Azure Console and using the search option look for Virtual Machine and Click on Create Virtual Machine

<img width="952" alt="create vm" src="https://github.com/KennedyNjuguna/kennedynjuguna.github.io/assets/88589168/2bcf7845-0b01-4171-af64-f7f38ffb6253">

Make sure to allow for HTTP, HTTPS and RDP access to your server.

<img width="944" alt="allowtraffic" src="https://github.com/KennedyNjuguna/kennedynjuguna.github.io/assets/88589168/7bc4fb1c-4e8b-4bda-926f-1bee9c9da32d">


## Connecting to the On-Prem server to install apps
Navigate to the resource created and click on the connect option on the side bar menu and navigate to the "Download RDP file" option.
<img width="949" alt="connect" src="https://github.com/KennedyNjuguna/kennedynjuguna.github.io/assets/88589168/9911f0d8-26c1-43ae-96aa-f1ec38a746e8">

Now connect to the RDP using the username and password you set during VM creation. You are now connected to your VM.

***For the purpose of this project demonstration, we will be installing apps and creating some files as the "server data" so that we can validate that the lift-shift operation is sucessfull***

Download and install  Notepad++ and create a file that we expect to see on migrating the server. You can choose to add more applications or create folders and files that you expect to see on performing the migration operation.

## Create a replication server in AWS and download agent

Navigate back to the aws management console and search for Application Migration Service.
<img width="947" alt="MGN" src="https://github.com/KennedyNjuguna/kennedynjuguna.github.io/assets/88589168/3878edf3-da39-42bf-b2d5-f8eceb1656cc">

***NOTE: IF YOU HAVE NEVER INTERACTED WITH THE AWS MGN, MAKE SURE YOU HAVE ADMIN PRIVELEGES SO AS TO BE ABLE TO START UP THE SERVICE***

On the side bar locate Replication Template and edit the configurations of the template.
Edit the; 
* Network - VPC and Subnet
* Instance Type
* EBS Volume Type
* Security Group - Defines access to the server

On the side bar locate Launch Template and edit the configurations of the template accordingly and save the template.
<img width="943" alt="dd" src="https://github.com/KennedyNjuguna/kennedynjuguna.github.io/assets/88589168/42628b89-604c-4b60-8484-7eb0e058eb13">

On the side bar locate Post-Launch Template and edit the configurations of the template and save the template. 
<img width="948" alt="q" src="https://github.com/KennedyNjuguna/kennedynjuguna.github.io/assets/88589168/a59a2adb-3f94-4db7-a3c8-2164de0d9983">

Now navigate to the Source Servers on the side bar and fill in the configurations of the server you want to replicate. 
***NOTE: YOU WILL HAVE TO CREATE AN IAM USER WITH THE [AWSApplicationMigrationAgentInstallationPolicy policy] ATTACHED***.


## Install agent on the On-Prem Windows Server
Feel in the ***IAM access key ID** and the **IAM secret access key** then go ahead and copy the **agent installer** link and download the agent in your server
<img width="944" alt="aa" src="https://github.com/KennedyNjuguna/kennedynjuguna.github.io/assets/88589168/741b2dba-41fd-4599-b438-4f896041efcd">
<img width="959" alt="wa" src="https://github.com/KennedyNjuguna/kennedynjuguna.github.io/assets/88589168/779f9225-02ac-453b-8269-77947e2e3fb1">


Now run your cmd from the folder containing your download agent then paste in the copied command from the aws console.
<img width="949" alt="cc" src="https://github.com/KennedyNjuguna/kennedynjuguna.github.io/assets/88589168/b869f32b-b24a-4e9c-9901-9ca4b473b817">
<img width="949" alt="as" src="https://github.com/KennedyNjuguna/kennedynjuguna.github.io/assets/88589168/e86803dd-bdcd-4815-ad08-80bcb2d19c6b">


The agent is succesfully installed and the instance is discovered and displayed in the aws console
<img width="944" alt="wassa" src="https://github.com/KennedyNjuguna/kennedynjuguna.github.io/assets/88589168/d46f1c31-0237-4cc6-b9a5-9b8e920c364e">
<img width="952" alt="Screenshot 2024-02-07 141113" src="https://github.com/KennedyNjuguna/kennedynjuguna.github.io/assets/88589168/8faf1841-71b2-44c3-bb84-0cdd11f33d05">

The replication is ongoing in the aws console and depending on the Lifecycle stage (Not ready, Ready for testing, Test in progress, Ready for cutover, Cutover in progress, Cutover complete) you are able to perform various actions
<img width="947" alt="Untitled" src="https://github.com/KennedyNjuguna/kennedynjuguna.github.io/assets/88589168/21cbfaec-8c6d-4cfc-b547-961edeca9856">

<img width="944" alt="Screenshot 2024-02-07 141932" src="https://github.com/KennedyNjuguna/kennedynjuguna.github.io/assets/88589168/5d5c42bb-c2ad-4737-b1fa-509caf80a903">


##  Launch the test process
When the replication is at 100% and the lifecycle switches to "ready for testing" you can launch a test instance according to the launch testing we set. You can observe a running instance in the EC2 Dashboard that is running
<img width="947" alt="asss" src="https://github.com/KennedyNjuguna/kennedynjuguna.github.io/assets/88589168/761efe90-52b9-4d1f-8673-d2510fa39cb5">

<img width="946" alt="2" src="https://github.com/KennedyNjuguna/kennedynjuguna.github.io/assets/88589168/e6c5f51a-2491-48a7-9ec2-a65495699a2e">

<img width="950" alt="3" src="https://github.com/KennedyNjuguna/kennedynjuguna.github.io/assets/88589168/ad572fb0-169b-453a-9eb5-fd0ce73ccdd3">

Confirm if it works well then mark it as ***ready for cutover*** manually or let it automatically mark as ready for cutover.


## Cutover Process

After testing and the lifecycle indicates "ready for cutover" you are now ready to initiate the cutover process. The testing instance is deleted and a working instance of your application is now created.












