# Terraform Beginner Bootcamp 2023

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