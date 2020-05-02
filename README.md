# Collection of useful commands and other handy info

## k8 related stuff


### kubectl get pods
`kgp` 

This repository provides the [terraform](https://www.terraform.io/) configurations for provisioning an [EKS cluster](https://aws.amazon.com/eks/) on AWS.

NOTE: For completeness and full code examples, this codebase does not make use of the [Terraform registry](https://registry.terraform.io/)

A number of common modules (EG. The AWS VPC) _could_ be replaced by re-usable ones from the registry. For example the VPC could be configured utilising the verified [AWS VPC module](https://registry.terraform.io/modules/terraform-aws-modules/vpc/aws/2.33.0)

## Pre-Requisites

The repository assumes that you have installed Terraform and configured it to allow access to your (or the target) AWS account.

## Directory Structure

```
├── LICENSE
├── README.md
├── environments
│   ├── dev.tfvars
│   └── prod.tfvars
├── main.tf
└── modules
    └── vpc
        ├── variables.tf
        └── vpc.tf
        ...
└── state
...
```

The **main.tf** file is where terraform will start its executing. 

The **modules** directory contains re-usable modules for each part of our infrastructure. Each module has configurable variables which are then configured according to each environment - for example **dev.tfvars**

The **state** directory contains the terraform configuration for initialising the remote state.

## Instructions

### Preparation

**Output your public key**

`cat ~/.ssh/id_rsa_eks_cluster.pub`

**Update code with your public key**

Locate the environments/dev/variables.tf file

Replace the `ssh-rsa REPLACE_ME` with your public key.

So it looks something like below.

NOTE: There are no new lines in the value.

```
variable "ec2_key_public_key" {
    default = "ssh-rsa Adfdf3NzaC1yc2EAAAADAQABAAABAQCvAHsugzgbU1Sp2X5LsvI4Vp2iCSAUWpNTIdDv/x9mTPEA+kex98nrcYzuipu5iu50ay07SFWQlh8WYsxw03I7Tyu9Hj55Nt+kbTqsZbOoZNrGVNZjTvS6s24cdXVj6qV1p088SySXrfdfhdf6cbdgd7/3FXoiM1IFGlcmev1CC+6Dycacdhd66fbf GTgpFHLApSXe+0OekUORmBYPVrpWqSfyImSskVAED5DnfGU0mHoeCQpSd+G2dErqGvo1lAXinWBf2TphsQVGkiG45y8S75iiH5jt4We/ someperson@somelaptop"
}
```

### Remote State Creation

**Navigate into the state directory**

```
cd state
```

**Initialise the terraform AWS provider**

```
terraform init
```

**Run a plan**

It'll ask you a bucket name - enter something unique such as using your name.

```
terraform plan
```

**Apply that plan to create your state buckets**

It'll ask you a bucket name - use the same name as the previous step.

```
terraform apply
```

**Make a note of your bucket name**


```
terraform output terraform_bucket_name
```

Now wait a few moments for the S3 bucket to be fully ready before moving on.

### Provisioning our cluster and all required services

Finally we can create our cluster

**Navigate to dev environment folder**

```
cd ../environments/dev
```

**Update bucket name**

Edit the **backend.tf** file and update the bucket name by replacing the 'bucket' with the output of your previous `terraform output...` command.

**Initialise Terraform**

This will pull down the required providers (AWS and HTTP)

```
terraform init
```
***note you might need to use -reconfigure if you come across forbidden issues

**Run a plan**

```
terraform plan
```

**Apply your changes**

This might take between 5 and 15 minutes depending on AWS.

```
# commands
