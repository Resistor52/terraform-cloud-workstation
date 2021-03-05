# Step 8 - main.tf - Add Variables, Provider & Data Blocks

In this step, we will create a new file called `main.tf` and add in the blocks for
the variables, the aws provider, and the data for our script.

1. In the CloudShell, make sure you are in the directory of the local git repo. Run
`cd ~/tech` and then press TAB, then ENTER.
2. Create the file by running `vim main.tf`
3. Enter insert mode by pressing the "i" key.
4. Now, lets add in the variable blocks. You will note that there is one block for
each of the parameters set in the `terraform.tfvars` file. Copy and paste the
following code into the terminal:

```
variable "aws_region" {
  description = "The AWS region to deploy the resources to"
}

variable "aws_creds_file" {
  description = "The full path to the .aws/credentials file"
}

variable "aws_profile" {
  description = "The profile in the credentials file to use"
}

variable "aws_pem" {
  description = "The PEM file to use for SSH. This is outputted with the IP for convenience"
}

```

5. Next, specify the provider, which in our case is "aws". Providers are a Terraform
plugin to manage a specific infrastructure platform. [You can read all about providers
in the Terraform documentation](https://www.terraform.io/docs/language/providers/index.html).
As you can see below, some providers require configuration. Copy and paste the following block
into your terminal to add it to the `main.tf` file:

```
provider "aws" {
  region                  = var.aws_region
  shared_credentials_file = var.aws_creds_file
  profile                 = var.aws_profile
}

```   

Did you notice how this block uses the three of the variables defined earlier? And
that the variables are set by reading the `terraform.tfvars` file?  

**INFO:** According to the
 [Terraform documentation](https://www.terraform.io/docs/language/data-sources/index.html),
 _"Data sources allow data to be fetched or computed for use elsewhere in Terraform
 configuration. Use of data sources allows a Terraform configuration to make use
 of information defined outside of Terraform, or defined by another separate
 Terraform configuration."_


6. Add the following data block to the bottom of the script:

```
data "http" "myip" {
  url = "http://ipv4.icanhazip.com"
}

```

This block creates a data structure that contains the public IP address of system
that runs the Terraform script. This will be used to define a security group later
in the code that follows.

7. Add a data block to determine the all of the AWS availability zones. This block
can only be used with the aws provider:

```
data "aws_availability_zones" "all" {}

```

NOTE: Reference the relevant documentation for the above data block
[here](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/availability_zones).

8. Add a data block to get the latest Ubuntu server AMI:

```
data "aws_ami" "ubuntu" {
  most_recent = true

  filter {
    name   = "name"
    values = ["ubuntu/images/hvm-ssd/ubuntu-focal-20.04-amd64-server-*"]
  }

  filter {
    name   = "virtualization-type"
    values = ["hvm"]
  }

  owners = ["099720109477"]
}

```

The above data block applies some filters to select the desired image. Note that the
"owners" attribute specifies the AWS account (controlled by Amazon) that publishes
the AMI. [Here](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/ami)
is the reference for the "aws_ami" data source.

9. Finally, close the Vim editing session by hitting ESC and typing `:wq`


**[Watch the Video](https://youtu.be/DrRZX1FfUdY)**
