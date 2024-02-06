# APPLICATION MIGRATION TO AWS


Welcome to this project!! In this Project we are going to lift and shift an application to the AWS cloud using the Appplication Migration Service. The objective is to show how an on-premise application is transferred to an the AWS Cloud seamlessly with its associated data and configurations. This ensures that your application experiences minimal disruptions while also leveraging the features that AWS has to offer.

***NB:*** **AS I AM NOT CURRENTLY HOSTING ON-PREM SERVERS, WE WILL LIFT AND SHIFT AN APPLICATION SERVER FROM THE AZURE CLOUD PLATFORM TAKING THE VMs CREATED IN AZURE AS THE ON-PREM SERVERS**

## Table of Contents

- [STEPS I WILL TAKE](#steps-i-will-take)
  * [Creating Servers On-Prem](#creating-servers-on-prem)
  * [Connecting to the On-Prem server to install apps](#connecting-to-the-on-prem-server-to-install-apps)

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




















