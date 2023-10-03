# Terraform Beginner Bootcamp 2023


# WEEK 0 (PROJECT PREP)

## Semantic Versioning

This Project is going to utilize semantic versioning for its tagging.
[semver.org](https://semver.org/)

The general format:
 **MAJOR.MINOR.PATCH**, eg `1.0.1`

- **MAJOR** version when you make incompatible API changes
- **MINOR** version when you add functionality in a backward compatible manner
- **PATCH** version when you make backward compatible bug fixes

## Install the Terraform CLI


### Consideration with the Terraform CLI changes
The terraform CLI installation instructions have changed due to gpg keyring changes. So we needed to refer to the latest Install CLI instructions via Terraform Documentation and change the installation script.


[Install Terraform CLI](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli)

### Considerations for Linux Distribution

This project is built against Ubuntu.
Please Consider checking your Linux Distribution and change accordingly to distribution needs

[How-to-Check-Linux-Version-in-Linux-Command-Line](https://www.cyberciti.biz/faq/how-to-check-os-version-in-linux-command-line/)

```
$ cat /etc/os-release

PRETTY_NAME="Ubuntu 22.04.3 LTS"
NAME="Ubuntu"
VERSION_ID="22.04"
VERSION="22.04.3 LTS (Jammy Jellyfish)"
VERSION_CODENAME=jammy
ID=ubuntu
ID_LIKE=debian
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
UBUNTU_CODENAME=jammy
```

### Refactoring into Bash Scripts

While fixing the Terraform CLI gpg depraciation issues we noticed that bash scripts steps were a considerable amount of more code. So we decided to create a bash script to install the Terraform CLI

This bash script is located here: [./bin/Install_Terraform_cli](./bin/Install_Terraform_cli)

- This will keep the Gitpod Task File([.gitpod.yml](.gitpod.yml)) tiddy.
- This allows an easier to debug and execute manually Terraform CLI install
- This will allow better portability for other projects that need to install Terraform CLI

### SHEBANG

A SheBang tells the bash script what program will interpret the script eg. `#!/bin/bash`

RECOMMENDED `#!/bin/env bash`
- For portability for different OS distributions
- Will search the user's PATH for the bash executable

https://en.wikipedia.org/wiki/Shebang_(Unix)


#### Linux Permissions Considerations

In Order to make our bash scripts executable we need to change linux permissions for the fix to be executable at the user mode.

```sh
chmod u+x ./bin/Install_Terraform_cli
```
Alternatively:
```sh
chmod 744 ./bin/Install_Terraform_cli
```
https://en.wikipedia.org/wiki/Chmod


### Github Lifecycle (Before, Init, Command)

Init will not rerun if we restart an existing workspace

https://www.gitpod.io/docs/configure/workspaces/workspace-lifecycle

### Working with Env Vars

We can list out all Environment Variable using the `env` command

We can filter specific env vars using grep eg. `env | grep AWS`


#### Setting and Unsetting Env Var

In the terminal we can set using `export PROJECT_ROOT='/workspace/terraform-beginner-bootcamp-2023'`

In the terminal we unset using `unset PROJECT_ROOT`

We can set an env var temporarily when just running a command

```sh
PROJECT_ROOT='/workspace/terraform-beginner-bootcamp-2023' ./bin/print_message
```

Within a bash script we can set env without writing export eg.

```sh
#!/usr/bin/env bash

PROJECT_ROOT='/workspace/terraform-beginner-bootcamp-2023'

echo PROJECT_ROOT
```

## Printing Env Vars

We can print an env var using echo eg. `echo PROJECT_ROOT`

#### Scoping of Env Var

When you open up new bash terminals in VSCode it will not be aware of env vars that you have set in another windows

If you want to Env Vars to persist across all future bash terminals that are open you need to set env vars in your bash profile eg. `.bash_profile`

#### Persisting Env Vars in Gitpod

We can persist env vars into gitpod by storing them in Gitpod Secrets Storage.

```sh
gp env PROJECT_ROOT='/workspace/terraform-beginner-bootcamp-2023'
```

All future workspaces launched will set the env vars for all bash terminals opened in those workspaces.

You can also set en vars in the `.gitpod.yml` but this can only contain non sensitive env vars.

### AWS CLI Installation

AWS CLI is installed for the project via the bash script [`./bin/install_aws_cli`](./bin/install_aws_cli)

[Getting Started Install (AWS CLI)](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)
[AWS CLI Env Vars](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-envvars.html)

We can check if our AWS credentials is configured correctly by running the following AWS CLI command:

```sh
aws sts get-caller-identity
```

If it is succesful you should see a json payload return that looks like this:

```json
{
    "UserId": "AKIAIOSFODNN7EXAMPLE",
    "Account": "123456789012",
    "Arn": "arn:aws:iam::123456789012:user/terraform-beginner-bootcamp"
}
```

We'll need to generate AWS CLI credentials from IAM user in order to use the AWS CLI


## Terraform Basics

### Terraform Registry

Terraform sources their providers and modules from the Terraform registry which is located at [registry.terraform.io](https://registry.terraform.io/)

- **Providers** is an API that will allow to create resources in Terraform
- **Modules** are a way to refactor or to make large amounts of terraform code modular, portable and sharable

[Random Terraform Provider](https://registry.terraform.io/providers/hashicorp/random)

### Terraform Console

We can see a list of all the Terraform commands by typing `terraform`


### Terraform Init

At the start of a new terraform project we will run `terraform init` to download the binaries for the terraform providers that we'll use in this project.


#### Terraform Plan

`terraform plan`

This generates a changeset, about the state of our infrastructure and what wil be changed.

We can output this changes ie. "plan" to be passed to an apply, but often you can just ignore outputting.

#### Terraform Apply

`terraform apply`

This will run a plan and pass the changeset to be executed by terraform. Apply should prompt us yes or no.

If we want to auto approve we can provide the auto approve flag eg. `terraform apply --auto-approve`


## Terraform Lock Files

`.terraform.lock.hcl` contains the locked versioning for the providers or modules that should be used with this project.

The Terraform Lock File **should be committed** to your Version Control System (VSC) EG. gITHUB

### Terraform State Files

`.terraform.tfstate` cntains information about the current state of your infrastructure.

This file **should not be committed** to your VSC.

This file can contain sensitive data. 

If you lose this file, you lose knowing the state of your infrastructure.

`.terraform.tfstate.backup` is the previous state file state.

### Terraform Directory

`.terraform` directory contains binaries of terraform providers.

### Terraform Destroy

`terraform destroy`

This will destroy resources

## Issues with Terraform Cloud Login and Gitpod Workspace

When attempting to run `terraform login` it will launch bash a wiswig view to generate a token. However it does not work as expected in Gitpod VsCode in the browser.

The workaround is to manually generate a token in Terraform Cloud

```
https://app.terraform.io/app/settings/tokens?source=terraform-login
```

Then create and open the file manually here:

```sh
touch /home/gitpod/.terraform.d/credentials.tfrc.json
open /home/gitpod/.terraform.d/credentials.tfrc.json
```

Provide the following code and (replace your token in the file):

```json
{
    "credentials": {
        "app.terraform.io": {
            "token": "YOUR-TERRAFORM-CLOUD-TOKEN"
        }
    }
}
```



