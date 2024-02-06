In this project we are going to seamlessly migrate an application and its database to a multi-cloud environment, strategically leveraging the strengths of both Amazon Web Services (AWS) and Google Cloud Platform (GCP). At the heart of this project lies a robust and versatile application that generates and manages valuable data in PDF format. 

_Key highlights of our project:_

**Data Storage:** We are set to store the application's data in the form of PDF files. These files will find their home in secure AWS S3 buckets, harnessing the robust storage capabilities of AWS.

**Containerization:** Our application will undergo a transformative journey, being containerized and residing within the Google Container Registry. This approach not only enhances portability but also streamlines deployment, ensuring rapid and reliable scaling.

**Deployment:** Google Kubernetes Engine (GKE) emerges as our deployment platform of choice, offering the orchestration and management needed to facilitate the seamless operation of containerized applications.

**Database Residency:** The database underpinning our application will take root in the Google Cloud SQL environment. Google Cloud SQL, with its high availability and automatic backups, promises a robust foundation for our data.
**Automating Infrastructure Provisioning with Terraform:** Terraform will enable us to define, deploy, and manage our multi-cloud infrastructure efficiently, reducing manual intervention and enhancing consistency. 

# STEPS TO IMPLEMENT PROJECT

## Amazon Web Services

- Access AWS console and go to IAM service. Under Access management, Click in "Users", then "Add users". Insert the User name terraform-en-1 and click in Next to create a programmatic user.

![terraform1](https://github.com/KennedyNjuguna/kennedynjuguna.github.io/assets/88589168/1ecf85c6-23f9-4015-bd2f-a7456975b691)


- On Set permissions, Permissions options, click in "Attach policies directly" button.

<img width="524" alt="permissions" src="https://github.com/KennedyNjuguna/kennedynjuguna.github.io/assets/88589168/f7fad81b-f510-4f95-a1bd-e19628837222">

- Type AmazonS3FullAccess in Search and select AmazonS3FullAccess.

<img width="530" alt="AmazonS3FullAccess" src="https://github.com/KennedyNjuguna/kennedynjuguna.github.io/assets/88589168/b4af536a-d2ea-4ecc-a26c-a6049ef26aa7">

- Click in Next and Review all details, then click Create user. This user has been created succesfuly and allows Terraform to be able to provision buckets in AWS.

<img width="1112" alt="reviewandcreate" src="https://github.com/KennedyNjuguna/kennedynjuguna.github.io/assets/88589168/17ab59c7-ee51-4efd-8c65-199f1117decb">

- To download the `access key`, Click on the user you have created then navigate to the **Security credentials** tab, scroll down and go to Access keys section and **Create access key**

<img width="1130" alt="createaccesskey" src="https://github.com/KennedyNjuguna/kennedynjuguna.github.io/assets/88589168/c36175f9-0618-4e73-ac6a-f2dab088de59">

- Select **Command Line Interface (CLI)** and **I understand the above recommendation and want to proceed to create an access key** checkbox then click **Next**. Click on **Create access key** and **Download .csv file**

## Google Cloud Platform (GCP)

- Access GCP Console and open Cloud Shell. Upload **accessKeys.csv** and the **Zip file containing all your Terraform configurations**. Check if upload has been successfully completed using the command `ls -la`

```sh
mkdir mission1_en
mv mission1.zip mission1_en
cd mission1_en
unzip mission1.zip
mv ~/accessKeys.csv mission1/en
cd mission1/en
chmod +x *.sh
```

- Run the following commands to prepare AWS and GCP environment and Authorize when asked.

```sh
./aws_set_credentials.sh accessKeys.csv
gcloud config set project <project_id>
```
Execute the command below
```sh
./gcp_set_project.sh
```

- Enable the Container Registry API, Kubernetes Engine API and the Cloud SQL API

```sh
gcloud services enable containerregistry.googleapis.com 
gcloud services enable container.googleapis.com 
gcloud services enable sqladmin.googleapis.com
```

***Ensure your `Bucket_name` is unique while defining it in Terraform***

- Run the following commands to finish provision infrastructure steps

```sh
cd ~/mission1_en/mission1/en/terraform/

terraform init
terraform plan
terraform apply
	Type Yes and go ahead.
```

## SQL Network Configuration

- Naviagate to Google Cloud Console and search **SQL**. Click on the provisioned database and navigate to **connections**. Check the **Private IP** option and under **Associated Networking** click on default.

<img width="961" alt="SQLCONNECTION" src="https://github.com/KennedyNjuguna/kennedynjuguna.github.io/assets/88589168/6f194399-a913-44d5-a90e-5da2f0f2d023">

- Click on **SET UP CONNECTION** and **ENABLE API** and then click on **Use an automatically allocated IP range** and Continue then **Create Connection**. Save the changes you have made.

<img width="958" alt="ENABLEAPI" src="https://github.com/KennedyNjuguna/kennedynjuguna.github.io/assets/88589168/99f6221f-7c1d-45e3-a655-396e463936ca">

## Amazon Web Services


- Access AWS console and go to IAM service. Under Access management, Click in "Users", then "Add users". Insert the User name **en-app1** and click in Next to create a programmatic user. - On Set permissions, Permissions options, click in "Attach policies directly" button.

- Type AmazonS3FullAccess in Search and select AmazonS3FullAccess. Click in Next and Review all details, then click Create user. This user has been created succesfuly and allows the app to be able to access buckets in AWS.

- To download the `access key`, Click on the user you have created then navigate to the **Security credentials** tab, scroll down and go to Access keys section and **Create access key**. Select **Command Line Interface (CLI)** and **I understand the above recommendation and want to proceed to create an access key** checkbox then click **Next**. Click on **Create access key** and **Download .csv file**


- Navigate to **Google Cloud SQL Instance** and create a new user **app** with passowrd on Cloud SQL MYSQL databse.

<img width="857" alt="appuser" src="https://github.com/KennedyNjuguna/kennedynjuguna.github.io/assets/88589168/388e8f28-a836-4701-a919-eebe0f66e6b1">

- Now Connect to Google Cloud Shell and download the app files to Google Cloud Shell using the `wget` command

```sh
﻿cd ~ mkdir app_en cd app_en wget https://tcb-public- events.s3.amazonaws.com/icp/app.zip unzip app.zip
```

- Connect to MYSQL DB running on Cloud SQL and input the password you created if prompted

```sh
mysql --host=<public_ip_cloudsql> --port=3306 -u app -p
```

- Enable Cloud Build API via Cloud Shell

```sh
﻿# Command to enable Cloud Build API gcloud services enable
cloudbuild.googleapis.com
```

- If you see the error below, follow the steps to fix it:

```sh
ERROR: (gcloud.builds.submit) INVALID_ARGUMENT: could not resolve source: googleapi: Error 403: 989404026119@cloudbuild.gserviceaccount.com does not have storage.objects. get access to the Google Cloud Storage object., forbidden To solve it: 1. Access IAM & Admin; 2. Click on check-box Include Google-provided role grants; 3. Click and select your Cloud Build Service Account Example: 989404026119@cloudbuild.gserviceaccount.com Cloud Build Service Account 4. On your Cloud Build Service Account, right side, click on Edit principal 5. Click on Add another role 6. Click on Select Role, and filter by Storage Admin or gcs. Select Storage Admin (Full control of GCS resources). 7. Click on Save and go to Cloud Shell.
```

- Build the Docker Image and push it to Google Container Registry. Replace <PROJECT_ID> with your PROJECT_ID

```sh
﻿cd ~/app_en/app/en/app gcloud builds submit --tag
gcr.io/<PROJECT_ID>/app-en
```

- Open the Cloud Editor and edit the Kubernetes deployment file (app.yaml) and update your <PROJECT_ID> on the Google Container Registry path, AWS Keys (open file en-app1.csv and use Access key ID and Secret access key) and Cloud SQL Database Private IP.


- Connect to the GKE (Google Kubernetes Engine) cluster via Console and deploy the application Luxxy in the Cluster

```sh
cd ~/app_en/app/en/kubernetes kubectl apply -f app.yaml
```

- You should see the app up and running! Congratulations


## Google Cloud Platform - Database Migration

 - Connect to Google Cloud Shell and Download the dump using wget

```sh
cd ~ mkdir database_en cd database_en wget https://tcb-public- events.s3.amazonaws.com/icp/database.zip unzip database.zip
```

- Connect to Cloud SQL MySQL database instance

```sh
mysql --host=<public_ip_address> --port=3306 -u app -p
```

- Import the dump on Cloud SQL

```sh
use dbapp; source ~/databse_en/database/en/db/db_dump.sql
```

- Check if the data got imported correctly using

```sh
select * from records; exit;
```

## Amazon Web Services - PDF Files Migration

Connect to the AWS Cloud Shell. Download the PDF files

```sh
mkdir database_en cd databse_en wget https://tcb-public-
events.s3.amazonaws.com/icp/database.zip unzip database.zip
```

 Sync PDF Files with your AWS S3. Replace the bucket name with yours.

```sh
cd database/en/pdf_files aws s3 sync . s3://Bucket_Name
```

Test the application. Upon migrating the data and files, you should be able to see the entries.




**_CONGRATULATIONS YOU HAVE SUCCESFULLY UTILIZED MULTI-CLOUD TO MIGRATE DATA AND APPLICATION SUCCESSFULLY_**




