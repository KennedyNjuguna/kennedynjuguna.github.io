# APPLICATION MIGRATION TO AWS


Welcome to this project!! In this Project we are going to lift and shift an application to the AWS cloud using the Appplication Migration Service. The objective is to show how an on-premise application is transferred to an the AWS Cloud seamlessly with its associated data and configurations. This ensures that your application experiences minimal disruptions while also leveraging the features that AWS has to offer.

***NB:*** **AS I AM NOT CURRENTLY HOSTING ON-PREM SERVERS, WE WILL LIFT AND SHIFT AN APPLICATION SERVER FROM THE AZURE CLOUD PLATFORM TAKING THE VMs CREATED IN AZURE AS THE ON-PREM SERVERS**

## STEPS I WILL TAKE
- [Create Servers On-Prem (using Azure as the on-prem environment)](#Creating-Servers-On-Prem)
- [Connect to the On-Prem server to install apps](#Connecting-to-the-On-Prem-server-to-install-apps)
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






















