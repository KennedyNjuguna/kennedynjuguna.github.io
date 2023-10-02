In the rapidly evolving landscape of software development, where innovation is the lifeblood of progress, effective version control is no longer optional—it's essential. AWS CodeCommit offers a robust and secure platform for teams of all sizes, whether you're a startup aiming to scale rapidly or an enterprise with complex development workflows. AWS CodeCommit is a highly scalable, managed source control service that hosts private Git repositories. CodeCommit stores your data in Amazon S3 and Amazon DynamoDB giving your repositories high scalability, availability, and durability. You simply create a repository to store your code. There is no hardware to provision and scale or software to install, configure, and operate.

## Create an AWS CodeCommit repository

At the top of the AWS Management Console, in the search bar, search for and choose `CodeCommit`

On the AWS CodeCommit page, choose `Create repository`

On the Create repository page:
- For Repository name, enter `name-repo`
- For Description enter `Description-of-Repo`

Choose Create.

An empty repository named `name-repo` is created. :thumbsup:You have successfully created a new CodeCommit repository.

## Connect to the Amazon EC2 instance

Create an Instance

Connect to the instance using EC2 Instance Connect or SSH into the instance.

:thumbsup:You have successfully connected to a terminal session on the EC2 instance.


## Create a local repository using Git

This will provide an example of how you would use AWS CodeCommit to synchronize to any local code repository that you might create in your normal production development environment.

In the terminal session, run the following command to install the Git client:

```bash
sudo yum install -y git
```

Run the following commands to configure the Git credential helper with the AWS credential profile, and allow the Git credential helper to send the path to repositories:
```bash
git config --global credential.helper '!aws codecommit credential-helper $@'
git config --global credential.UseHttpPath true
```
**These commands have no output**

Return to your web browser tab with the AWS CodeCommit console, which should be on the `name-repo` page and at the upper-right of the page, choose `Clone URL` , and then choose **Clone HTTPS**.


The repository URL is copied to your clipboard and should look similar to this: _https://git-codecommit.us-east-1.amazonaws.com/v1/repos/name-repo_


Return to your web browser tab with the terminal session. Run the following command to clone the `name-repo` repository to the instance:
- Replace the CLONE_HTTPS_URL placeholder value with the Clone HTTPS URL that you copied previously.
```bash
git clone CLONE_HTTPS_URL
```

The output should indicate that you are cloning the My-Repo repository, and that the repository is empty, similar to this:
```bash
Cloning into 'My-Repo'...
warning: You appear to have cloned an empty repository.
```

:thumbsup:You have successfully connected to and synchronized with the AWS CodeCommit repository.

## Making a code change and first commit to the repo

 Run the following command to change to the **name-repo** directory
 ```bash
cd ~/My-Repo
```

Run the following command to create two files in your local repo:

```bash
echo "Am excited to be using AWS Code Commit in this demo." >demo.txt
echo "Development teams uses AWS CodeCommit to securely store and manage code repositories in the cloud. " >example.txt
```
**These commands have no output.**

To see list of files in current directory use
```bash
ls
```
The output should show the two files you created, similar to this:
```bash
demo.txt example.txt
```

Run the following command to stage the changes in your local repo:

```bash
git add demo.txt example.txt
```

Run the following command to view the status of your repo:
```bash
git status
```
The output should show the branch you are current working in (master) and that the two files are ready to be committed to the repository, similar to this:
```bash
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   demo.txt
        new file:   example.txt
```

Run the following command to commit the changes in your local repo:
```bash
git commit -m "Added demo.txt and example.txt"
```

The output displays a message stating that the name and email address of the committer were configured automatically. In a production environment, you would use the commands listed to set your name and email address, which are then applied to each commit you do. The output also shows that two files were changed and inserted, similar to this:
```bash
Committer: EC2 Default User <ec2-user@ip-10-1-12-142.ec2.internal>
Your name and email address were configured automatically based
on your username and hostname. Please check that they are accurate.
You can suppress this message by setting them explicitly:

    git config --global user.name "Your Name"
    git config --global user.email you@example.com

After doing this, you may fix the identity used for this commit with:

    git commit --amend --reset-author

 2 files changed, 2 insertions(+)
 create mode 100644 demo.txt
 create mode 100644 example.txt
```

To view details about the commit you just made, run the following command:

```bash
git log
```
The output shows that there is one commit to the master branch, the name of the author, the date the commit was made, and the files that were added, similar to this:

```bash
commit 772d16037b2e0d7ee7e97aa9218e571346bebe0e (HEAD -> master)
Author: EC2 Default User <ec2-user@ip-10-1-12-142.ec2.internal>
Date:   Wed Jul 20 19:20:06 2022 +0000

    Added demo.txt and example.txt
```

Now that you have an initial commit in your local repo, you can push the commit from your local repo to your AWS CodeCommit repository.

## Push your first commit

Run the following command to push your commit through the default remote name Git uses for your AWS CodeCommit repository (origin), from the default branch in your local repo (master):
```bash
git push -u origin master
```

The output displays the details of the process to create the branch and push the files to the remote repository, similar to this:
```bash
Enumerating objects: 4, done.
Counting objects: 100% (4/4), done.
Delta compression using up to 2 threads
Compressing objects: 100% (4/4), done.
Writing objects: 100% (4/4), 447 bytes | 447.00 KiB/s, done.
Total 4 (delta 0), reused 0 (delta 0), pack-reused 0
To https://git-codecommit.us-east-1.amazonaws.com/v1/repos/My-Repo
 * [new branch]      master -> master
Branch 'master' set up to track remote branch 'master' from 'origin'.
```
**After you have pushed code to your AWS CodeCommit repository, you can view the contents using the AWS CodeCommit console.**

:thumbsup:You have successfully pushed the changes from your local repository to the remote CodeCommit repository.





