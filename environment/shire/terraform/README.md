# Shire AWS Deployment

## Initial Requirements

* AWS Administrator Account
* Terraform

## Install Terraform & AWS Libraries

Use the Mordor script `install-terraform-awscli.sh` to install all you need to the Mordor Shire environment in AWS. This will install the following:

* wget
* unzip
* awscli
* awsebcli
* terraform (0.12.2)

Run the following commands:

```
./install-terraform-awscli.sh
```

## Configure AWS Credentials

Run the aws binary with the `configure` parameter
```
aws configure
```

Fill out your credentials. Replace abc123 with your own `AWS Access Key ID` and your `AWS Secret Access Key`. This will avoid you commiting your AWS keys in terraform variable files.

```
AWS Access Key ID [None]: abc123 
AWS Secret Access Key [None]: abc123
Default region name [None]: us-west-1
Default output format [None]:
```

## Create Private Key

Make sure the name of the key is `linux`. The terraform scripts call for that specific key name. In addition, if you already have another pir of keys in that folder, the new key pair will not substitute your current keys.

```
ssh-keygen -b 2048 -f ~/.ssh/linux
```

## Get Your Public IP Address

Run the following command to use the Google Servers to get your current public IP

```
dig TXT +short o-o.myaddr.l.google.com @ns1.google.com | awk -F'"' '{ print $2}'
```

## Enter Public IP Address to Terraform Scripts

Once you get your public IP address, you will have to update the `terraform.tfvars file`. Before any changes the file will have the following information:

```
wardog$ cat terraform.tfvars 
region = "us-west-1"
profile = "terraform"
shared_credentials_file = "~/.aws/credentials"
public_key_name = "linux"
public_key_path = "~/.ssh/linux.pub"
private_key_path = "~/.ssh/linux"
ip_whitelist = ["0.0.0.0/32"] #Change this to your public ip
```

## Run Terraform Commands

```
wardog$ terraform init

Initializing the backend...

Initializing provider plugins...
- Checking for available provider plugins...
- Downloading plugin for provider "aws" (terraform-providers/aws) 2.15.0...
...
......
```

```
terraform Apply

...
.....
Plan: 16 to add, 0 to change, 0 to destroy.

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: yes

aws_key_pair.auth: Creating...
aws_vpc.default: Creating...
..
...
```
